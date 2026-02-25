---
tags:
  - CUDA
  - BankConflict
title: Swizzle
date: 2026-02-08 09:40
type: permanent-note
---
---
$$
\begin{align}
x_{chunk\_swz} &= y_{chunk} \oplus x_{chunk} \\
&=y \oplus (x \times sizeof(T) / sizeof(TC)) \\
&=y/(NX \times sizeof(T) / sizeof(TC)) \times NX \times sizeof(T) / sizeof(TC) + (y \% (NX * sizeof(T)/sizeof(TC))) \oplus (x \times sizeof(T)/sizeof(TC)) \\
&=y/(NX \times sizeof(T) / sizeof(TC)) \times NX \times sizeof(T) / sizeof(TC) + ((y \% NX) * sizeof(T)/sizeof(TC)) \oplus (x \times sizeof(T)/sizeof(TC))
\end{align}
$$


### Shared Memory 2D Layout and Shared Memory Bank Conflicts

On devices of compute capability 5.x or newer, each bank has a bandwidth of 32 bits every clock cycle, and successive 32-bit words are assigned to successive banks. So element size matters for shared memory bank conflicts.

In many use cases, the shared memory layout is a 2D row-major matrix whose row size is a multiple of 32 and element size is 32-bit. When strided access from a warp of threads is performed on the column of the matrix, severe 32-way shared memory bank conflicts will occur. If the element in each column are mapped to different shared memory banks, the shared memory bank conflicts can be mitigated.

Assuming `MBase` is zero, element size is 32-bit, and the matrix row size is , we could design the swizzle operation such that there is free of shared memory bank conflicts when accessing each column of the matrix, based on the offset bijection property we just proved above. To configure `BBits` and `SShift` such that `offset % 32` is distinct when a warp of threads accesses the column of the matrix, we have to set `SShift` to be $\log_2n$ and `BBits` to be $\log_232=5$, so that the $c$ used in $f(x)=x \oplus c$ are different for each row, resulting in distinct `offset % 32` when a warp of threads accesses the column of the matrix. To further elaborate on this, suppose the shared memory row size $n=128$, and the strided access of a column results in a 32-way shared memory bank conflict. The indices of the first column are $[0,128,256,...,128 * 31]$, and the last 7 bits of those indices are all 0, i.e., $c=0$. We want the last `BBits` of those indices to be swizzled so that the last 5 bits of the swizzled indices are $[0,1,2,...,31]$. Intuitively, by setting `SShift` to be $\log_2128 = 7$ and `BBits` to be 5, the $x$ used for swizzle for each row are $[0,1,2,...,31]$. The last 5 bits after swizzle $f(x)=x \oplus c$ are $[0 \oplus 0,1 \oplus 0,2\oplus 0,...,31\oplus 0]=[0,1,2,...,31]$, which guarantees the free of shared memory bank conflicts when accessing the column of the matrix. Similarly, this is also the case for the second, third, and other columns, where $c$ is different for each column.


