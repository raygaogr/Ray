---
tags:
  - Convolition
  - HPC
  - Direct-Conv
aliases:
  - Direct-Conv
date: 2023-12-21T15:25:00
---
---
>The maximum performance on our model architecture is attained when all Nfma units are computing one FMA per cycle. However, because each FMA instruction has a latencyof Lfma cycles, this means that there must at least be Lfma independent FMA instructions issued to each computational unit.” ([Zhang 等, 2018, p. 4](zotero://select/library/items/2V3AP7U6)) ([pdf](zotero://open-pdf/library/items/7C39LM9Z?page=4&annotation=4DCPLXSL)) 

硬件上有$N_{fma}$个多发单元，每个单元发射后有$L_{fma}$的延迟，如果希望填满整个流水线，则需要 $N_{fma} \times L_{fma}$ 个指令，而每个指令可以同时处理 $N_{vec}$ 个数据，因此最少需要 $N_{fma} \times L_{fma}\times N_{vec}$ 个数据进行处理。

[@zhangHighPerformanceZeroMemory2018]