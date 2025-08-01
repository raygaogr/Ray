---
tags: 
title: SEE2 指令集
date: 2024-12-11 17:02
type:
---
---
### \_mm_shuffle_pd

```c
double a_c[2] = {1.0, 2.0};
double b_c[2] = {3.0, 4.0};

_m128d a = _mm_load_pd(a_c); //低位 1.0 高位 2.0
_m128d b = _mm_load_pd(b_c); //低位 3.0 高位 4.0
_m128d tmp = _mm_shuffle_pd(a, b, _MM_SHUFFLE2(x, y)); // y表示从a中选择低位还是高位，如果y为0则选择a[0],否则a[1], x表示从b中选择低位还是高位，b为0则选择b[0],否则b[1]
```

### \_mm_load_pd

用于从内存中加载两组双精度浮点数（double）到一个 128 位的 SIMD 寄存器（通常是 xmm 寄存器）。
在汇编中，\_mm_load_pd 对应的指令是 MOVAPD（Move Aligned Packed Double-Precision Floating-Point Values）。
```c
movapd (array), %xmm // 从 array 地址开始加载 128 位到 xmm 寄存器中
```
**指令说明**
• MOVAPD **（Move Aligned Packed Double-Precision）**

	• 用于加载两个双精度浮点数（double，每个 64 位，共 128 位）到一个 xmm 寄存器。
	• 要求内存地址是 16 字节对齐的。

### \_mm_setzero_pd
用于将一个 128 位的 SIMD 寄存器置为全 0。
在汇编中，它等价于使用指令 xorpd，将寄存器与自身进行按位异或运算，从而将其清零。
```c
xorpd %xmm, %xmm
```

**含义解析**

• xorpd %xmm0, %xmm0:

	• 将寄存器 %xmm0 的内容与自身进行按位 XOR 操作。
	• XOR 运算中，任意值与自身 XOR 都会得 0，因此 %xmm0 的所有位被置为 0。

