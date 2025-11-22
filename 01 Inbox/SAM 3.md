---
tags:
  - SAM
title: SAM 3
date: 2025-10-13 13:38
type: permanent-note
---
---
## 1、Abstract
**我们提出 Segment Anything Model (SAM) 3，这是一种统一模型，能够基于概念提示（concept prompts）在图像和视频中检测、分割和跟踪物体。我们定义的概念提示包括简短的名词短语（如‘黄色校车’）、图像示例，或两者的组合。可提示概念分割（Promptable Concept Segmentation, PCS）接受此类提示，并返回所有匹配物体实例的分割掩码和唯一标识。为推进 PCS 研究，我们构建了一个可扩展的数据引擎，生成了一个包含 400 万个独特概念标签（含困难负样本）的高质量数据集，覆盖图像和视频。我们的模型由一个视觉主干（vision backbone）驱动，该主干在图像级检测器和基于记忆的视频跟踪器之间共享。通过解耦识别与定位（presence head），显著提升了检测精度。SAM 3 在图像和视频的 PCS 任务中比现有系统性能提升 2 倍，并在交互式视觉分割任务中优于之前的 SAM 系列模型。我们已开源 SAM 3 及其新的‘带概念的 Segment Anything’（SA-Co）基准测试。**
## 2、存在的问题
SAM 1 和 SAM 2 主要关注基于视觉提示的方法研究，并且一个提示只能完成一个实例对象的分割。
## 3、Promptable Concept Segmentation (PCS)
### 3.1、任务定义
给定一张图片或者一段短视频，检测、分割和追踪所有的示例对象，这些对象与提供的 *Concept prompt*  相匹配，这个提示由简单的名词短语、图像级的标注示例或者两者的组合构成。约束文本提示中的名词短语是结构简单，由一个名词以及可选的修饰词组成。
其中，名词短语是全局提示，针对所有的 frames，而图像级的标注示例可以为单独 frame 提供正样本和负样本框进行迭代式的 refine。
### 3.2、消除歧义
针对 PCS 任务中的描述的歧义性问题，SAM 3 提出了系统性的处理方法，分别从数据收集、指标设计以及建模等多个维度进行解决：
A. 从三个专家收集测试标注信息；
B. 自适应评估协议，允许多种有效的解释；
C. 设计数据流水线和指导原则来减少标注上的歧义性；
D. 引入歧义评估模块；
## 4、模型架构
SAM 3 是 SAM 2 的更通用版本，在支持 PVS 任务的同时也支持 PCS 任务，既可以有 *concept prompt (simple noun phrases, image exemplars)* 也可以有 *visual prompt (points, boxes, masks)*。
它的基本结构如下图所示：
![[Pasted image 20251013161152.png]]
由一个双路的 encoder-decoder transformer 结构组成，包括一个 image-level 的检测器和 video-level 的追踪器组成，其中检测器和追踪器引入了视觉语言数据信息，由一个视觉语言对齐模块 *Perception Encoder* 产生。
### 4.1、检测器
一个 encoder-decoder transformer 模型
#### 图像和文本编码器
由 *Perception Encoder (PE)* 构成，基于多模态的视觉语言对比训练方式得到，已经进行了视觉语言的特征对齐。
#### 几何和示例编码器
几何编码器用于对 *Point、Box* 提示进行编码，而示例编码器用于对 *Image exemplar* 进行编码，每一个单独的示例编码由 *Position embedding、label embedding、ROI-Pooled visual feature* 组成 。
#### 融合编码器
将无条件依赖的 frame-embedding 以及由 prompt-token 生成的依赖条件进行特征融合，产生有条件依赖的 frame-embedding。
#### 解码器
基于原始的 DETR 的解码器和一些 DETR 实用的变种构成。
#### 存在性 Head


### 4.2、追踪器
继承了 SAM 2 的 encoder-decoder 架构。
## 5、数据引擎
构建了一个**可扩展的**基于**人工以及模型**的**可迭代**的数据引擎来标注大规模和范围广的训练集。引入了以下三种创新：
### 5.1、Media Curation
相较于过于单一的网络数据源引入了种类更多的媒体数据源。
### 5.2、Label Curation
通过利用知识本体（ontology）和多模态大语言模型（LLMs）作为“AI标注者”，我们显著增加了标签的多样性和难度，生成名词短语（noun phrases）和困难负样本（hard negatives）。
### 5.3、Label Verification
通过微调多模态大语言模型（MLLMs）作为高效的“AI验证者”（其性能接近人类），我们将标注效率提升了一倍。从带有噪声的“媒体-短语-掩码”伪标签（pseudolabels）开始，我们的数据引擎通过 AI 验证者和人工验证者共同检查掩码的质量和全面性（exhaustivity），筛选出正确标注的样本，并识别具有挑战性的错误案例。随后，人工标注者专注于手动修正这些错误。这使我们能够生成高质量的训练数据集，包含 **400万个唯一短语** 和 **5200万个掩码**，以及一个合成数据集，包含 **3800万个短语** 和 **14亿个掩码**。
