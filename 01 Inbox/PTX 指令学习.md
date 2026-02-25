---
tags:
  - CUDA
  - PTX
title: PTX 指令学习
date: 2026-02-03 15:11
type: permanent-note
---
---
## 向量操作

### Float4 宏
``` c
#define FLOAT4(value) (reinterpret_cast<float* 4>(&(value))[0])
```

### LD 操作
`ld.global.v4.f32 {%f0, %f1, %f2, %f3}, [%rd0];`
- v4 = 向量化加载 4 个元素
- f32 = 每个元素是 32 位浮点数
- 一次性加载 128 位
### ST 操作
`st.global.v4.f32 [%rd0], {%f0, %f1, %f2, %f3};`

## Butterfly shuffle
`shfl.sync.bfly.b32 dest, src, lane_mask, mask_pred`
- mask_pred: 0xffffffff 参与同步的线程掩码，全为 1 表示 warp 内所有线程都参与；
- src: 要交换的值
- lane_mask: xor 掩码，决定与哪个线程交换数据
- dest: 返回值


---
## Float 4 vs cp.async vs ld.global.v4 加载的区别
### 1. 数据流路径
```c++
float4 / ld.global.v4:
  gmem → L2 → L1(TEX) → 寄存器 → （smem，如果需要再写）

cp.async.cg:
  gmem → L2 ──────────────────→ smem  （硬件DMA，完全绕过寄存器和L1）
```

这是三者最根本的区别。

---

### 2. 完整对比表

| 维度       | float4         | ld.global.v4     | cp.async.cg         |
| -------- | -------------- | ---------------- | ------------------- |
| 层次       | CUDA C++ 类型    | PTX 汇编指令         | PTX 异步拷贝指令          |
| 数据路径     | gmem→L2→L1→寄存器 | gmem→L2→L1→寄存器   | gmem→L2→smem        |
| 寄存器消耗    | 4个float（16字节）  | 4个float（16字节）    | 零                   |
| 执行方式     | 同步阻塞           | 同步阻塞             | 异步非阻塞               |
| 目标位置     | 寄存器（再手动写smem）  | 寄存器（再手动写smem）    | 直接写smem             |
| L1 Cache | 经过，可能污染        | 可控制（.cg绕过）       | 绕过L1                |
| 需要同步原语   | 无需             | 无需               | 需要commit/wait_group |
| 编译器控制    | 可能退化为标量指令      | 强制向量化            | 强制异步拷贝              |
| 支持架构     | 所有             | 所有               | Ampere（SM80）起       |
| 数据类型     | 仅 float        | f32/f16/u32/b32等 | 类型无关（按字节）           |
| 对齐要求     | 16字节对齐         | 16字节对齐           | 16字节对齐（cg模式）        |
| 能否隐藏延迟   | 否              | 否                | 是（与计算重叠）            |

---

### 3. 代码对比

场景：把 gmem 数据搬到 smem
```C++
// ① float4：需要寄存器中转，两步操作
float4 tmp = *reinterpret_cast<float4*>(&gmem[idx]); // 寄存器
*reinterpret_cast<float4*>(&smem[offset]) = tmp;    // 写smem

// ② ld.global.v4：强制向量化，但同样需要寄存器中转
uint32_t r0, r1, r2, r3;
asm("ld.global.v4.b32 {%0,%1,%2,%3}, [%4];"
    : "=r"(r0),"=r"(r1),"=r"(r2),"=r"(r3)
    : "l"(gmem_ptr));
// 再手动存到 smem ...

// ③ cp.async.cg：一步直达，无寄存器消耗
CP_ASYNC_CG(smem_ptr, gmem_ptr, 16);  // 异步发射即返回
CP_ASYNC_COMMIT_GROUP();
// ... 做其他计算 ...
CP_ASYNC_WAIT_GROUP(0);               // 等待完成
```

---

### 4. 寄存器压力影响 Occupancy

GPU 的寄存器是 Block 内所有线程共享的有限资源（每个SM约64K个32位寄存器）。
```c++
假设 Block 有 256 个线程，每次搬运 16 字节：

float4 / ld.global.v4:
  每个线程占用 4 个寄存器（128 位）
  256 线程 × 4 寄存器 = 1024 个寄存器被搬运占用

cp.async.cg:
  0 个寄存器
  → 寄存器全部留给计算，Occupancy 更高
```

---
### 5. 软件流水线：cp.async 的真正价值

float4 和 ld.global.v4 无法做到的事：
```c++
【无流水，float4】
iter 0: [等待 load K0] → [计算 Q@K0^T] → [等待 load K1] → [计算 Q@K1^T]
         ↑ 访存和计算串行，访存延迟完全暴露

【有流水，cp.async，kStage=2】
预取:         [异步发射 load K0] [异步发射 load K1]
iter 0:  [等K0就位] → [计算 Q@K0^T] + [同时异步发射 load K2]
iter 1:  [等K1就位] → [计算 Q@K1^T] + [同时异步发射 load K3]
          ↑ 访存延迟被计算隐藏，吞吐量大幅提升
```

---
### 6. 何时选用哪种
```c++
需要立即用数据做计算     → float4 或 ld.global.v4
  （数据→寄存器→直接参与MMA/算术运算）

需要搬运大块数据到smem   → cp.async.cg
  （prefetch、流水线、不需要寄存器中转）

需要精确控制cache策略    → ld.global.v4（.cg/.cs/.nc后缀）
  或 cp.async（本身就是L2直通）

架构限制 < SM80          → 只能用 float4 / ld.global.v4
```