---
tags:
  - GraphOpt
title: 图优化 Pass
date: 2026-04-20 17:18
type:
---
---
## 1. 程序分析与编译原理（经典理论）

图优化的根基来自传统编译器理论：

| 理论                            | 在图优化中的体现                                     |
| ----------------------------- | -------------------------------------------- |
| **数据流分析**（Data-flow Analysis） | 常量传播、死代码消除、活变量分析 → 判断某个算子是否可删、是否全常量          |
| **公共子表达式消除**（CSE）             | 图层面的 CSE：相同输入+相同参数的算子合并                      |
| **静态单赋值形式**（SSA）              | MLIR 基于 SSA，使得 use-def 链清晰，便于做全局的 def-use 分析 |
| **依赖分析**                      | 判断两个算子之间是否有数据依赖（真依赖、反依赖、输出依赖），决定能否重排或并行      |

**关键结论**：图优化中大量的 Pattern Match + Rewrite，本质上就是**重写系统**（Rewrite System）在 DAG/IR 上的应用。

---

## 2. 等价变换理论（Equality Saturation / E-graphs）

这是近年来图优化领域最有影响力的理论方向之一。

### 核心思想

不再用单向的 rewrite rule（A → B），而是维护一个 **E-graph**（等价图），把**所有已知的等价形式**同时保存在一个紧凑的数据结构中，然后用成本模型从中选出最优的实现。

```
原始表达式:  Conv(x, w1) + Conv(x, w2)
           |
           v  (通过分配律)
         Conv(x, w1+w2)   <-- 可能更优（一次卷积）
```

### 代表工作

- **TASO**（Stanford, 2019）：基于等价图做算子融合优化，用 SAT solver 验证变换的等价性
- **TENSAT**（2021）：将 Equality Saturation 扩展到深度学习图优化
- **Meta's AITemplate / TensorRT**：内部大量使用基于代价的图等价搜索

### 与 TPU-MLIR 的关系

TPU-MLIR 目前使用的是**单向的 Greedy Pattern Rewrite**（`applyPatternsAndFoldGreedily`），属于经典编译器的**局部重写**。这保证收敛性，但可能陷入局部最优。E-graphs 是其潜在的升级方向。

---

## 3. Polyhedral 编译模型（多面体模型）

### 核心思想

将循环嵌套和数组访问用**整数集合和仿射函数**描述，通过**整数线性规划**（ILP）寻找最优的循环变换（tile、fuse、reorder、vectorize）。

```
for i
  for j
    A[i][j] = B[i][j] + C[i][j]
```

→ 用 Presburger 算术表示迭代域和访问关系 → 求解最优调度。

### 在图优化中的角色

虽然 Polyhedral 传统上用于**循环优化**，但在 AI 编译器中：

- **算子融合**（Operator Fusion）可以建模为**迭代域的并集**（loop fusion）
- **TVM 的 Compute/Schedule 分离**（受 Halide 启发）本质上是 Polyhedral 思想的工程化简化
- **MLIR 的 Affine Dialect** 直接基于 Polyhedral 表示

### 代表工作

- **Pluto**（2008）：自动并行化与局部性优化
- **Polly**（LLVM 子项目）：LLVM IR 上的 Polyhedral 优化
- **Tensor Comprehensions**（Facebook）：用 Polyhedral 做 CUDA kernel 自动生成

---

## 4. 成本模型与搜索理论（Auto-tuning）

图优化不仅仅是"能不能做"，更是"做了是否更快"。这需要**成本模型**。

### 类型

|成本模型类型|原理|代表|
|---|---|---|
|**解析模型**（Analytical）|基于算子的 FLOPs、内存带宽、数据排布，建立解析公式|TensorRT 的 layer fusion 决策|
|**黑盒搜索**（Black-box）|把每种图变换后的性能直接跑一遍，选最快的|AutoTVM、Ray Tune|
|**可学习模型**（Learned）|用 GNN/MLP 预测子图在不同后端上的执行时间|Ansor、MetaFlow|

### 理论支撑

- **Knuth 的优化编译器理论**：代码生成作为搜索问题
- **组合优化**：图变换序列的搜索空间是指数级的，需要启发式/近似算法
- **强化学习**（AutoTVM/Ansor）：将 schedule/tile 大小作为动作空间，用 PPO/XGBoost 搜索

---

## 5. 形式化验证（Formal Verification）

图优化最大的风险是：**rewrite 后数值是否等价？**

### 理论基础

- **操作语义**（Operational Semantics）：定义每个算子的精确数学行为
- **霍尔逻辑**（Hoare Logic）：{P} C {Q}，证明变换前后前后条件不变
- **SMT/SAT 求解**：用于验证浮点/整数运算的等价性（TASO 用 Z 3）

### 实践中的折中

TPU-MLIR 这类生产级编译器通常**不做什么形式化证明**，而是依赖：

1. **参考实现对比**（InferenceInterface CPU 模拟 vs TPU 输出）
2. **回归测试**（大量模型端到端精度对比）
3. **保守的 rewrite 条件**（如 Conv 融合时严格检查 padding、group、relu 等）

---

## 6. 抽象解释（Abstract Interpretation）

这是静态程序分析的理论基础，由 Patrick Cousot 提出。

在图优化中：

- **Shape Inference** 本质上是**抽象解释**：把具体张量值抽象为 shape 信息，在每个算子上传播抽象值
- **常量折叠**（Constant Folding）是**常量传播**的特例
- **范围分析**（Range Analysis）：用于量化时的 scale 推导

---

## 7. 有没有"统一理论"？

**目前还没有一个能覆盖所有图优化的大一统理论。** 当前主流框架是**多层理论栈**：

```
┌─────────────────────────────────────────┐
│  应用层：TVM/MLIR/XLA 的 Pass Pipeline   │  ← 工程经验 + 启发式
├─────────────────────────────────────────┤
│  搜索层：AutoTVM/Ansor/Equality Saturation│  ← 组合优化 + ML
├─────────────────────────────────────────┤
│  变换层：Polyhedral / Loop Transform     │  ← 整数线性规划
├─────────────────────────────────────────┤
│  分析层：数据流 / SSA / 抽象解释          │  ← 编译原理
├─────────────────────────────────────────┤
│  验证层：SMT / 操作语义 / 浮点理论        │  ← 形式化方法
└─────────────────────────────────────────┘
```

---

## 8. 推荐文献

如果想深入理论学习，推荐阅读：

| 方向                      | 文献/项目                                                             |
| ----------------------- | ----------------------------------------------------------------- |
| **Rewrite Systems**     | 《Term Rewriting and All That》(Baader & Nipkow)                    |
| **Polyhedral**          | 《Scheduling and Automatic Parallelization》(Darte et al.)          |
| **Halide/Schedule**     | 《Decoupling Algorithms from Schedules》(Ragan-Kelley et al., 2012) |
| **Equality Saturation** | TASO (OSDI'19), TENSAT (PLDI'21)                                  |
| **Auto-tuning**         | Ansor (OSDI'20), AutoTVM (TVM 论文)                                 |
| **MLIR 设计**             | 《MLIR: Scaling Compiler Infrastructure》(Lattner et al., 2020)     |