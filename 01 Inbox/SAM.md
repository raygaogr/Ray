---
tags:
  - SAM
title: SAM
date: 2025-10-14 08:43
type:
---
---
## 1、Abstract
**“我们推出了 Segment Anything（SA）项目：一个全新的图像分割任务、模型和数据集。通过在数据收集流程中使用我们高效的模型，我们构建了迄今为止最大的分割数据集（远远超过以往），包含超过 10 亿个掩码标注，覆盖 1100 万张已授权且尊重隐私的图像。该模型被设计并训练为可提示的模型，因此能够零样本迁移到新的图像分布和任务中。我们在多项任务中评估了其能力，发现其零样本性能表现惊人——通常与或甚至优于以往的全监督方法。我们已开源 Segment Anything Model（SAM）及其对应的数据集 SA-1B（包含 10 亿个掩码和 1100 万张图像），网址为 https://segment-anything.com，以推动计算机视觉领域基础模型的研究。”**
## 2、目标
为图像分割任务构建一个基础模型，即开发一种可提示的模型并在大规模数据集上进行预训练确保它强大的泛化能力。
### 2.1、解决方案
1、什么**任务**可以达到 zero-shot 的泛化？（promptable task）
2、相关的**模型**架构怎么设计？(flexible prompts/real-time/ambiguity-aware)
3、什么样的**数据**能够满足这样的任务和模型？(large diverse dataset)
![[Pasted image 20251014090827.png]]

## 3、可提示分割 vs 交互式分割

| 特性     | 交互式分割（Interactive Segmentation） | 可提示分割（Promptable Segmentation）      |
| ------ | ------------------------------- | ----------------------------------- |
| 设计目标   | 服务于人类用户（通过实时交互调整结果）             | 支持多种提示输入（点、框、掩码、文本等），可服务于人机协作或自动化系统 |
| 典型应用场景 | 医学图像标注，图像编辑工具（Photoshop）        | 自动标注、开放词汇分割、数据引擎                    |
| 交互方式   | 用户直接干预（点击、拖动）                   | 提示可由人类提供，也可以由其他算法模块生成               |
| 系统集成能力 | 通常作为独立工具使用                      | 可作为模块嵌入复杂系统（如自动化数据标注流水线）            |
## 4、Segment Anything Model

模型的整体结构如下图所示：
![[Pasted image 20251014100926.png]]

### 4.1、Image Encoder
使用 MAE 的 ViT 作为 Image-encoder，图像编码器只需要对图像执行一次，产生的特征可以作为先验知识用于后面的提示任务。
### 4.2、Prompt Encoder
有两种类型的提示：稀疏（points、boxes、text）和稠密（masks）。
**Point** 被表示成点的位置的 positional encoding **+** 点属于 foreground/background 的语义编码。
**Box** 被表示成一个 embedding pair：
1）框的 top-left 顶点的位置编码 **+**  一个可学习的用来表示 "top-left corner" 的 embedding
2）框的 bottom-right 顶点的位置编码 **+** 一个可学习的用来表示 "bottom-right corner" 的 embedding
**Mask** 用卷积神经网络直接进行 encoding，然后与 image embedding 进行 element-wise 相加，如果没有 mask prompt 一个可学习的表示 no mask 的 embedding 被加到每一个 image embedding 上。
### 4.3、Mask decoder
在常规的 transformer-based 的模块基础上，加上了 prompt 的交互影响
![[Pasted image 20251014164624.png]]
每个 decoder layer 执行 4 个步骤：
1）self-attention on the tokens
2）cross-attention from tokens (token as the queries) to the image embedding
3）MLP update the token
4）cross-attention from the image embedding（as queries）to tokens
## 5、Segment Anything Data Engine
### 5.1、Assisted-manual stage
与交互式分割类似，一组专家通过增加前景和背景的方式与 SAM 模型进行交互，同时对分割结果进行修正。
在这个阶段开始的时候，SAM 基于常见的**公开分割数据集**进行训练。当收集到足够的标注数据后，SAM 基于新数据进行重新训练。随着收集到的分割掩码越来越多，图像编码器变得越来越大，并且其他的结构也开始发生进化。
在这个阶段从 **12 万**张图片中收集了 **430 万**张掩码。
### 5.2、Semi-automatic stage
这阶段的目标是增加图像的多样性，让标注者去关注那些重要性不是那么高的物体区域。
首先，用模型去自动标注高可能性的目标，高可能性的目标挑选是使用了 Faster-RCNN 基于之前的标注结果进行训练得到，然后让标注者去增加额外的他任务可增加目标掩码。
这个阶段从额外的 **18 万**张图中新增了 **590 万**张掩码
### 5.3、Fully-automatic stage
这阶段的标注完全由模型自动进行，具体步骤：
首先，为每张图生成 32 x 32 个提示点，每个提示点会预测一组与有效目标相关的分割 mask。然后使用 IoU 预测模块选择高置信度的 mask。同时只选择和标记 stable mask（stable mask表示当分割阈值在 $0.5 - \delta$ 和 $0.5 + \delta$ 时，它们的 mask 应该相差不大）。最后，当选择高置信度的 mask 和稳定的 mask 之后，使用 NMS 去过滤掉重复的 mask。并且为了提高小目标的 mask 的质量，对输入图片进行了有重叠的多次放大操作，进行 mask 预测。
这个阶段，为 1100 万张图自动生成了 10 亿张分割 mask。