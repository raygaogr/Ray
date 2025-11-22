---
tags: 
title: GEMM Macro Kernel
date: 2024-12-10 15:09
type:
---
---
The macro kernel should compute the matrix-matrix product $C \leftarrow \beta C + \alpha \; \tilde{A} \cdot \tilde{B}$  for a packed $m_c \times k_c$  matrix block $\tilde{A}$  and a packed $k_c \times n_c$  matrix block $\tilde{B}$.


