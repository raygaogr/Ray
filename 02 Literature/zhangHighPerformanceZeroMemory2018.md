---
zotero-key: 2V3AP7U6
zt-attachments:
  - "23"
title: High Performance Zero-Memory Overhead Direct Convolutions
citekey: zhangHighPerformanceZeroMemory2018
---

# High Performance Zero-Memory Overhead Direct Convolutions

[Zotero](zotero://select/library/items/2V3AP7U6) [attachment](file:///Users/ray/Zotero/storage/7C39LM9Z/Zhang%20%E7%AD%89%20-%202018%20-%20High%20Performance%20Zero-Memory%20Overhead%20Direct%20Convo.pdf)

> [!note] Page 1
>
> The problems with such an approach are two-fold. First, these routines incur additional memory overhead which reduces the overall size of the network that can fit on embedded devices with limited memory capacity. Second, these high performance routines were not optimized for performing convolution, which means that the performance obtained is usually less than conventionally expected.
>
> ---
>
> im2col-based 方法缺点：
> 1、引入额外的内存开销；
> 2、现有的矩阵乘法对展开后的 im2col 矩阵没有高效的实现；
> ^Y9GZEYR9a7C39LM9Zp1

> [!note] Page 2
>
> Contributions. Herein lies the contributions of this paper: • High performance direct convolution. We show that a high performance implementation of direct convolution can out-perform a expert-implemented matrix-matrix multiplication based convolution in terms of amount of actual performance, parallelism, and reduced memory overhead. This demonstrates that that direct convolution is a viable means of computing convolution layers. • Data layouts for input/output feature maps and kernel weights. We proposed new data layouts for storing the input, output and kernel weights required for computing a convolution layer using our direct convolution algorithm. The space required for these new data layouts is identical to the existing data storage scheme for storing the input, output and kernel weights prior to any packing or duplication of elements.
>
> ---
>
> Contributions:
> 1、提出了一种高性能的直接卷积算法；
> 2、提出了一种新的数据排布，不会引入额外的内存占用；
> ^SDG3XIAKa7C39LM9Zp2

> [!note] Page 3
>
> The maximum performance on our model architecture is attained when all Nfma units are computing one FMA per cycle. However, because each FMA instruction has a latency
> ^ITDYWR7Ea7C39LM9Zp3

> [!note] Page 4
>
> of Lfma cycles, this means that there must at least be Lfma independent FMA instructions issued to each computational unit.
>
> ---
>
> 硬件上有$N_{fma}$ 个多发单元，每个单元发射后有$L_{fma}$的延迟，如果希望填满整个流水线，则需要 $N_{fma} x L_{fma}$ 个指令，而每个指令可以同时处理 $N_{vec}$ 个数据
> ^4DCPLXSLa7C39LM9Zp4
