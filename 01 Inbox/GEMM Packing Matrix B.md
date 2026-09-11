---
tags: 
title: GEMM Packing Matrix B
date: 2024-12-10 14:51
type:
---
---
In principle, packing of the matrix $B$ is done in the same way as for the matrix $A$. However, here the vertical panels are copied from a $k_c \times n_c$. In theory, each vertical panel consists of **row vectors** of length $n_r$. In practice, however, the buffer can be interpreted in two ways:

- A $k_c \times n_c$ matrix with vertical panels composed of row vectors.
- A $n_c \times k_c$ matrix with horizontal panels composed of column vectors.

Obviously, the matrix in the second case is just the transpose of the matrix from the first case.

## example
The partitioning of the matrix $B$ into $k_c \times n_c$ blocks is analogous to the partitioning of $A$. We therefore only consider the packing of a single block.

Let $k_c = 11, n_c = 12$ and $n_r = 6$. Then

$$\begin{eqnarray*}B &=& \left( \begin{array}{cccccc:cccccc} b_{ 1,1} & b_{ 1,2} & b_{ 1,3} & b_{ 1,4} & b_{ 1,5} & b_{ 1,6} & b_{ 1,7} & b_{ 1,8} & b_{ 1,9} & b_{ 1,10} & b_{ 1,11} & b_{ 1,12} \\ b_{ 2,1} & b_{ 2,2} & b_{ 2,3} & b_{ 2,4} & b_{ 2,5} & b_{ 2,6} & b_{ 2,7} & b_{ 2,8} & b_{ 2,9} & b_{ 2,10} & b_{ 2,11} & b_{ 2,12} \\ b_{ 3,1} & b_{ 3,2} & b_{ 3,3} & b_{ 3,4} & b_{ 3,5} & b_{ 3,6} & b_{ 3,7} & b_{ 3,8} & b_{ 3,9} & b_{ 3,10} & b_{ 3,11} & b_{ 3,12} \\ b_{ 4,1} & b_{ 4,2} & b_{ 4,3} & b_{ 4,4} & b_{ 4,5} & b_{ 4,6} & b_{ 4,7} & b_{ 4,8} & b_{ 4,9} & b_{ 4,10} & b_{ 4,11} & b_{ 4,12} \\ b_{ 5,1} & b_{ 5,2} & b_{ 5,3} & b_{ 5,4} & b_{ 5,5} & b_{ 5,6} & b_{ 5,7} & b_{ 5,8} & b_{ 5,9} & b_{ 5,10} & b_{ 5,11} & b_{ 5,12} \\ b_{ 6,1} & b_{ 6,2} & b_{ 6,3} & b_{ 6,4} & b_{ 6,5} & b_{ 6,6} & b_{ 6,7} & b_{ 6,8} & b_{ 6,9} & b_{ 6,10} & b_{ 6,11} & b_{ 6,12} \\ b_{ 7,1} & b_{ 7,2} & b_{ 7,3} & b_{ 7,4} & b_{ 7,5} & b_{ 7,6} & b_{ 7,7} & b_{ 7,8} & b_{ 7,9} & b_{ 7,10} & b_{ 7,11} & b_{ 7,12} \\ b_{ 8,1} & b_{ 8,2} & b_{ 8,3} & b_{ 8,4} & b_{ 8,5} & b_{ 8,6} & b_{ 8,7} & b_{ 8,8} & b_{ 8,9} & b_{ 8,10} & b_{ 8,11} & b_{ 8,12} \\ b_{ 9,1} & b_{ 9,2} & b_{ 9,3} & b_{ 9,4} & b_{ 9,5} & b_{ 9,6} & b_{ 9,7} & b_{ 9,8} & b_{ 9,9} & b_{ 9,10} & b_{ 9,11} & b_{ 9,12} \\ b_{10,1} & b_{10,2} & b_{10,3} & b_{10,4} & b_{10,5} & b_{10,6} & b_{10,7} & b_{10,8} & b_{10,9} & b_{10,10} & b_{10,11} & b_{10,12} \\ b_{11,1} & b_{11,2} & b_{11,3} & b_{11,4} & b_{11,5} & b_{11,6} & b_{11,7} & b_{11,8} & b_{11,9} & b_{11,10} & b_{11,11} & b_{11,12} \\ \end{array} \right)\\[0.5cm]&=& \left( \begin{array}{c:c} \vec{b}_{ 1}^T & \vec{b}_{12}^T\\ \vec{b}_{ 2}^T & \vec{b}_{13}^T\\ \vec{b}_{ 3}^T & \vec{b}_{14}^T\\ \vec{b}_{ 4}^T & \vec{b}_{15}^T\\ \vec{b}_{ 5}^T & \vec{b}_{16}^T\\ \vec{b}_{ 6}^T & \vec{b}_{17}^T\\ \vec{b}_{ 7}^T & \vec{b}_{18}^T\\ \vec{b}_{ 8}^T & \vec{b}_{19}^T\\ \vec{b}_{ 9}^T & \vec{b}_{20}^T\\ \vec{b}_{10}^T & \vec{b}_{21}^T\\ \vec{b}_{11}^T & \vec{b}_{22}^T\\ \end{array} \right)\end{eqnarray*}$$

copied to the **row-by-row stored matrix**

$$\tilde{B} = \left( \begin{array}{c} \vec{b}_{ 1}^T \\ \vec{b}_{ 2}^T \\ \vec{b}_{ 3}^T \\ \vec{b}_{ 4}^T \\ \vec{b}_{ 5}^T \\ \vec{b}_{ 6}^T \\ \vec{b}_{ 7}^T \\ \vec{b}_{ 8}^T \\ \vec{b}_{ 9}^T \\ \vec{b}_{10}^T \\ \vec{b}_{11}^T \\ \vec{b}_{12}^T \\ \vec{b}_{13}^T \\ \vec{b}_{14}^T \\ \vec{b}_{15}^T \\ \vec{b}_{16}^T \\ \vec{b}_{17}^T \\ \vec{b}_{18}^T \\ \vec{b}_{19}^T \\ \vec{b}_{20}^T \\ \vec{b}_{21}^T \\ \vec{b}_{22}^T \\ \end{array} \right)$$

This is identical _in terms of memory layout_ to the **column-wise stored matrix** :
$$\tilde{B} = \left( \begin{array}{cccccccccccccccccccccc} \vec{b}_{ 1} & \vec{b}_{ 2} & \vec{b}_{ 3} & \vec{b}_{ 4} & \vec{b}_{ 5} & \vec{b}_{ 6} & \vec{b}_{ 7} & \vec{b}_{ 8} & \vec{b}_{ 9} & \vec{b}_{10} & \vec{b}_{11} & \vec{b}_{12} & \vec{b}_{13} & \vec{b}_{14} & \vec{b}_{15} & \vec{b}_{16} & \vec{b}_{17} & \vec{b}_{18} & \vec{b}_{19} & \vec{b}_{20} & \vec{b}_{21} & \vec{b}_{22} \end{array} \right)$$
