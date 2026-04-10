---
tags:
  - CUDA
  - CUDAGraph
title: CUDA Graph
date: 2026-04-09 11:55
type: permanent-note
---
---
## CUDA Graph 的作用
CUDA Graph 是 NVIDIA CUDA 平台中用于优化 GPU 工作流执行效率的机制。它将一系列 GPU 操作（如内核启动、内存拷贝等）预定义为有向图结构，减少 CPU 与 GPU 之间的交互开销，从而提高性能。其核心价值在于**降低启动开销（Launch Overhead）**：在传统流模式中，每次内核启动都需要 CPU 驱动程序执行一系列准备操作，对于执行时间很短的小内核而言，这种开销可能占据总执行时间的显著比例[](https://docs.nvidia.cn/cuda/cuda-programming-guide/04-special-topics/cuda-graphs.html#cuda-graphs-edge-data)。CUDA Graph 将多个操作打包成一个图，在定义阶段一次性完成所有准备工作，后续多次执行时只需极低的 CPU 开销即可启动整张图。
## CUDA Graph 的流程
### 1、图创建
常见方式，使用流捕获的方法进行 CUDA Graph 的创建：
```c++
// 1.1 开启 stream 的图捕获
cudaStreamBeginCapture(stream, cudaStreamCaptureModeRelaxed);
// 1.2 遍历图中所有节点，提交kernel到stream上，**注意** kernel只被记录但不执行
// ...
// 1.3 完成图捕获，获得一个cudaGraph_t 对象
cudaStreamEndCapture(stream, &cudaGraph_t)
```
### 2、实例化

```c++
cudaGraphInstantiate(&cudaGraphExec_t, cudaGraph_t, NULL, NULL, 0)
// 进行一次图更新
cudaGraphExecUpdate(&cudaGraphExec_t, cudaGraph_t, cudaGraphExecUpdateResultInfo);

```
**作用**：将捕获的"模板图"编译为 GPU 可直接执行的指令序列。

- 解析依赖关系
- 预分配内存
- 优化调度顺序

**何时执行**：仅在第一次捕获后执行一次。

**更新 vs 重新实例化**：

| 操作                     | 适用场景              | 开销      |
| ---------------------- | ----------------- | ------- |
| `cudaGraphExecUpdate`  | kernel 参数变化，但结构不变 | 低（毫秒级）  |
| `cudaGraphInstantiate` | 图结构变化（节点增删）       | 高（百毫秒级） |

**何时需要更新**：

- KV cache 指针变化（每 token 都会变）
- Batch size 不变，但输入地址变化
### 执行
```c++
// 执行图
CudaGraphLaunch (&cudaGraphExec_t, stream);
```
