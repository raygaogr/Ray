---
tags:
  - DiffusionModel
title: DDPM的理解思路
date: 2024-07-16 19:58
---
---

## 1、基础定义
变分扩散模型的结构如下图所示：

![[Pasted image 20240716201359.png]]

它包含一组状态：$\mathbf{X_0},\mathbf{X_1},...,\mathbf{X_{T-1}},\mathbf{X_T}$ ，其中：
$\mathbf{X_0}$  表示原图；
$\mathbf{X_T}$ 是隐变量，形式上是一个高斯白噪声，即$\mathbf{X_T} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$ ;
$\mathbf{X_{t-1}}$  是中间隐变量；

上面的结构是一个双向的操作，其中可以把中间每步看成是一个独立的[VAE](VAE.md)， 所有的`Encoder` 组合在一起理解成最终的`Encoder` , `Decoder` 同理。

## 基础的Block
### Transition Block
第 $t$ 个transition block包含三个状态$\mathbf{X}_{t-1},\mathbf{X}_t,\mathbf{X}_{t+1}$ 

![[Pasted image 20240716205023.png]]

