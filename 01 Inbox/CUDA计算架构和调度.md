---
tags:
  - CUDA
  - GPU
title: CUDA相关
date: 2025-08-06 08:33
type:
---
---
## **Nvidia GPU 的基本结构**

![[Pasted image 20250806083931.png#pic_center|900]]

- SM: 流多处理器，GPU 的核心单元
	- Control: 逻辑控制单元，和 CPU 的功能大致相当
	- Cores: 流处理器或者 Cuda cores，GPU 的核心计算单元
	- Memory: 片上内存，如 Shared Memory 这些
- Global Memory: 设备的全局内存

## Block 的调度
当调用一个核函数时，CUDA 运行时会启动大量的线程去执行核函数。这些线程以 block-by-block 的方式被分配到 SM 上，即一个 block 上的所有线程会**同时**分配（拿到寄存器、线程槽等资源）到一个**相同**的 SM。

![[Pasted image 20250806085420.png#pic_center]]

通过 block-by-block 的方式进行调度，可以使得一个 block 中的所有线程进行交互，包括 barrier synchronization 以及 shared memory

## 线程束
当 block 中的线程被分配到 SM 上后，它们会进一步地被划分成一组 warp，目前现有的 GPU 设计的 warp 中的线程数量是 32 个。然后 SM 以 SIMD 的方式来执行 warp 中的所有线程，即 warp 中的所有线程执行同一条指令，但处理不同的数据。在硬件结构上的设计就是，SM 中的 Cores 会以 processing block 的形式进行分组，每组 processing block 公用一个取指和分发单元。然后，线程束上的所有线程会被分配到同一个 processing block 中，获取到指令后然后同时执行。

### 线程束发散
由线程束的设计可以看到，如果一个线程束中存在某些线程执行某条指令而另一些线程不执行时（比如 if-else 语句），则会出现 warp divergence 问题。GPU 的解决策略是分多次去执行每个分支。

![[Pasted image 20250806092311.png#pic_center]]

### 线程束调度和延迟隐藏
GPU 通过 **硬件寄存器直接保存线程束状态** 和 **无需内存访问的快速切换机制**，实现了 **零开销调度**。这种设计是 GPU 高吞吐量的关键，但也依赖于硬件资源的充足性和程序的高并发性。