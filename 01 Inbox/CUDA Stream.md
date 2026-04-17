---
tags:
  - CUDA
  - CUDAStream
title: cudaStream
date: 2026-04-09 15:30
type: permanent-note
---
---
## Blocking 和 Non-Blocking Stream
### 创建方式
#### Blocking 流
```c++
cudaStreamCreate()
```
#### Non-Blocking 流
```c++
cudaStreamWithFlags(&stream, cudaStreamNonBlocking)
```

### 区别
Blocking 流和 Non-Blocking 流的区别主要是在于它们与**默认流的同步方式**
如下，是 Blocking 流的伪代码
```c++
cudaStream_t stream1, stream2; cudaStreamCreate(&stream1); 
cudaStreamCreate(&stream2);

kernel1<<<grid, block, 0, stream1>>>(...); kernel2<<<grid, block>>>(...);
kernel3<<<grid, block, 0, stream2>>>(...);  

cudaDeviceSynchronize();
```

在 Blocking 流中，kernel 2 是在**默认流**上，因此它必须等 kernel 1 执行完后才开始执行，同时，kernel 3 必须等**默认流** kernel 2 执行完后才开始执行，但如果中间没有 kernel 2，kernel 3 和 kernel 1 之间没有隐式的同步机制。

对于 Non-Blocking 流的伪代码：
```c++
cudaStream_t stream1, stream2; cudaStreamCreateWithFlags(&stream1, cudaStreamNonBlocking); cudaStreamCreateWithFlags(&stream2, cudaStreamNonBlocking);  

kernel1<<<grid, block, 0, stream1>>>(...); kernel2<<<grid, block>>>(...); 
kernel3<<<grid, block, 0, stream2>>>(...);  cudaDeviceSynchronize();
```
