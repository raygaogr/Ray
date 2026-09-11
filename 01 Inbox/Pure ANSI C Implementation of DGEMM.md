---
tags: 
title: DGEMM
date: 2024-12-10 10:36
type: fleeting-note
---
---
In the following we present the cache optimized implementation of the matrix-matrix product. The function dgemm_nn can compute operations of the form $C \leftarrow \beta C + \alpha A B$. All matrices can have **arbitrary row and column stride**. That in particular means each matrix can be row or column wise stored. It further means that we also can use the function for the computation of $C \leftarrow \beta C + \alpha A^T B$, $C \leftarrow \beta C + \alpha A B^T$ and  $C \leftarrow \beta C + \alpha A^T B^T$ 

## Building Blocks of dgemm_nn

- **pack_MRxk** and **pack_A** are copying row panels from matrix blocks of A into the buffer \_A. Details are described in [[GEMM Packing Matrix A]]
- **pack_kxNR** and **pack_B** are copying col panels from matrix blocks of A into the buffer \_B. Details are described in  [[GEMM Packing Matrix B]]
- **dgemm_micro_kernel** multiplies a row panel with a col panel. Details are given in [[GEMM Micro Kernel]]
- **dgemm_macro_kernel** multiplies a row panel with a col panel. Details are given in [[GEMM Macro Kernel]]
- **dgemm_nn** computes $C \leftarrow \beta C + \alpha A B$ as described in [[GEMM]]

## The Micro Kernel Algorithm
For the sake of simplicity we assume $m_r = n_r = 4$ in the description of the algorithm. Note that the pure C implementation works as long as $m_r$ is a divisor or $m_c$ and $n_r$ a divisor of $n_c$.

Recall that **A** points to a packed (maybe zero padded) row panel of height four and width $k_c$. That means we have the column wise stored panel

$$A=\begin{pmatrix}a_0 & a_4 & \dots & a_{4 k_c -4} \\a_1 & a_5 & \dots & a_{4 k_c -3} \\a_2 & a_6 & \dots & a_{4 k_c -2} \\a_3 & a_7 & \dots & a_{4 k_c -1} \\\end{pmatrix}$$

Further B points to a column panel of height $k_c$ and width four. That can be illustrated as a row wise stored panel

$$B=\begin{pmatrix}b_0 & b_1 & b_2 & b_3 \\b_4 & b_5 & b_6 & b_7 \\\vdots & \vdots & \vdots & \vdots \\b_{4 k_c-4} & b_{4 k_c-3} & b_{4 k_c-2} & b_{4 k_c-1} \\\end{pmatrix}$$
For the product $A \cdot B$ this means

$$\begin{eqnarray*}A \cdot B&=&\begin{pmatrix}a_0 \\a_1 \\a_2 \\a_3 \\\end{pmatrix}\begin{pmatrix}b_0 & b_1 & b_2 & b_3\end{pmatrix}+\begin{pmatrix}a_4 \\a_5 \\a_6 \\a_7 \\\end{pmatrix}\begin{pmatrix}b_4 & b_5 & b_6 & b_7\end{pmatrix}+\dots+\begin{pmatrix}a_{4 k_c-4} \\a_{4 k_c-3} \\a_{4 k_c-2} \\a_{4 k_c-1} \\\end{pmatrix}\begin{pmatrix}b_{4 k_c-4} & b_{4 k_c-3} & b_{4 k_c-2} & b_{4 k_c-1}\end{pmatrix}\\[1cm]&=&\begin{pmatrix}a_0 b_0 & a_0 b_1 & a_0 b_2 & a_0 b_3 \\a_1 b_0 & a_1 b_1 & a_1 b_2 & a_1 b_3 \\a_2 b_0 & a_2 b_1 & a_2 b_2 & a_2 b_3 \\a_3 b_0 & a_3 b_1 & a_3 b_2 & a_3 b_3 \\\end{pmatrix}+\begin{pmatrix}a_4 b_4 & a_4 b_5 & a_4 b_6 & a_4 b_7 \\a_5 b_4 & a_5 b_5 & a_5 b_6 & a_5 b_7 \\a_6 b_4 & a_6 b_5 & a_6 b_6 & a_6 b_7 \\a_7 b_4 & a_7 b_5 & a_7 b_6 & a_7 b_7 \\\end{pmatrix}+\dots+\begin{pmatrix}a_{4 k_c-4} b_{4 k_c-4} & a_{4 k_c-4} b_{4 k_c-3} & a_{4 k_c-4} b_{4 k_c-2} & a_{4 k_c-4} b_{4 k_c-1} \\a_{4 k_c-3} b_{4 k_c-4} & a_{4 k_c-3} b_{4 k_c-3} & a_{4 k_c-3} b_{4 k_c-2} & a_{4 k_c-3} b_{4 k_c-1} \\a_{4 k_c-2} b_{4 k_c-4} & a_{4 k_c-2} b_{4 k_c-3} & a_{4 k_c-2} b_{4 k_c-2} & a_{4 k_c-2} b_{4 k_c-1} \\a_{4 k_c-1} b_{4 k_c-4} & a_{4 k_c-1} b_{4 k_c-3} & a_{4 k_c-1} b_{4 k_c-2} & a_{4 k_c-1} b_{4 k_c-1} \\\end{pmatrix}\end{eqnarray*}$$
We compute $\mathbf{AB} = A \cdot B$ sequentially:

- Initialize: $\mathbf{AB} \leftarrow \mathbf{0}_{4 \times 4}$
    
- For $l = 0, \dots, k_c-1$ update:
    - $\mathbf{AB} \leftarrow \mathbf{AB} + \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix} \begin{pmatrix} b_{4l} & b_{4l+1} & b_{4l+2} & b_{4l+3}\end{pmatrix}$
        **(Note that this loop is the computational hotspot. In principal the overall performance only depends on the efficient update of $\mathbf{AB}$)**
Afterwards we merely update the left hand side micro block $\tilde{C}$:
- $\tilde{C} \leftarrow \beta \tilde{C}$
- $\tilde{C} \leftarrow \tilde{C} + \alpha \mathbf{AB}$