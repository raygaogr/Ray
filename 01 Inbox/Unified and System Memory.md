---
tags:
  - CUDA
  - Memory
title: Unified and System Memory
date: 2026-04-09 15:51
type: permanent-note
---
---
## Unified Memory
CUDA 提供能力可以让不同设备之间的内存分配、管理、迁移变得方便

- **Unified Virtual Address Space**: CPU 和 GPU 设备在同一个虚拟地址上拥有不同的地址分区。
- **Unified Memory**: 一种 CUDA 特性，可以让 CPU 和 GPU 自动的进行内存迁移。
	- **Limited Unified Memory**: 一种带约束的 Unified Memory
	- **Full Unified Memory**: 全特性支持的 Unified Memory
	- **Full Unified Memory with Hardware Coherency**: 使用硬件能力支持的Unified Memory
	- **Unified memory hints**: 
- **Page-locked Host Memory**: 锁页内存
	- Unified memory hints