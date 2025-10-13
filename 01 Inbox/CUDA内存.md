---
tags:
  - CUDA
  - GPU
title: CUDA内存
date: 2025-08-06 09:37
type:
---
---
## Roofline 模型

![[Pasted image 20250806101741.png#pic_center]]

## CUDA 内存类型

![[Pasted image 20250806102744.png#pic_center]]


|     内存类型      | Host 读 | Host 写 | Device 读 | Device 写 |                                                 备注                                                 |
| :-----------: | :----: | :----: | :------: | :------: | :------------------------------------------------------------------------------------------------: |
| Global Memory |   √    |   √    |    √     |    √     |                                    Grid 中所有线程共享（**per-grid** 级）                                    |
| Const Memory  |   √    |   √    |    √     |    ×     |                                    Grid 中所有线程共享（**per-grid** 级）                                    |
| Local Memory  |   √    |   √    |    √     |    √     | **GPU 线程利用全局内存（Global Memory）作为私有存储空间**，用于存储无法放入寄存器的数据，线程间不共享（**per-thread** 级）。包括静态分配数组，溢出寄存器，调用栈 |
|   Register    |   ×    |   ×    |    √     |    √     |                                        线程独有（**per-thread**）                                        |
| Shared Memory |   ×    |   ×    |    √     |    √     |                                        线程块共享（**per-block**）                                        |
## 内存声明

|                   变量声明                   |   内存类型   |  作用域   |    生命周期     |
| :--------------------------------------: | :------: | :----: | :---------: |
|                 非数组的自动变量                 | Register | Thread |    Grid     |
|                  数组自动变量                  |  Local   | Thread |    Grid     |
| \__device\__ \__shared\__ int SharedVar  |  Shared  | Block  |    Grid     |
|        \__device__ int GlobalVar         |  Global  |  Grid  | Application |
| \__device\__ \__constant\__ int ConstVar | Constant |  Grid  | Application |

