---
tags: 
title: GEMM Micro Kernel
date: 2024-12-10 14:58
type: permanent-note
---
---
The micro kernel should multiply a $m_r \times k_c$ panel $\hat{A}$ by a $k_c \times n_r$ panel $\hat{B}$. More precisely, the operation $C \leftarrow \beta C + \alpha \; \hat{A} ​​\cdot \hat{B}$ should be performed. The values ​​ $m_r$ and $n_r$ are already known at compile time, the value $k_c$ only at runtime.
## algorithm
A possible algorithm for calculating

$$\begin{eqnarray*} C &\leftarrow& \beta C + \alpha \; \hat{A} \cdot \hat{B} & = & \beta C + \alpha \left( \begin{array}{ccc} \vec{a}_1 & \dots & \vec{a}_{k_c} \end{array} \right) \cdot \left( \begin{array}{c} \vec{b}_{ 1}^T \\ \vdots \\ \vec{b}_{k_c}^T \\ \end{array} \right) & = & \beta C + \alpha \left( \vec{a}_1 \cdot \vec{b}_{ 1}^T + \dots + \vec{a}_{k_c} \cdot \vec{b}_{k_c}^T \right)\end{eqnarray*}$$

Could be based on the following idea:

- Create a local buffer $AB$ with $m_r \cdot n_r$ elements and initialize it to zero.
    
- Overwrite the buffer $AB$ with $$\vec{a}_1 \cdot \vec{b}_{ 1}^T+\dots + \vec{a}_{k_c} \cdot \vec{b}_{k_c}^T$$ (so the calculations are done correctly in this step!)
    
- If $\beta=0$ initialize $C$ with zeros and otherwise with $\beta C$. In the latter case you have $m_r \cdot n_r$ additional multiplications.
    
- If $alpha=1$ overwrite $C$ with $C+AB$ otherwise with $C+\alpha \cdot AB$. In the latter case you have $m_r \cdot n_r$ additional multiplications.
## Further remark on implementation
The crucial micro-micro operation is the calculation of

$$\begin{eqnarray*}\vec{a}_l \cdot \vec{b}_l^T &=&\left( \begin{array}{c} a_1^{(l)} \\ \vdots \\ a_{m_r}^{(l)} \end{array}\right)\left( \begin{array}{ccc} b_1^{(l)} & \dots & b_{n_r}^{(l)} \end{array}\right)\\[0.5cm]&=&\left( \begin{array}{c} a_1^{(l)} \\ \vdots \\ a_{m_r}^{(l)} \end{array}\right)b_1^{(l)}+ \dots +\left( \begin{array}{c} a_1^{(l)} \\ \vdots \\ a_{m_r}^{(l)} \end{array}\right)b_{n_r}^{(l)} \\[0.5cm]&=& \vec{a}_l \cdot b_1^{(l)} + \dots + \vec{a}_l \cdot b_{n_r}^{(l)}\end{eqnarray*}$$

If you choose $m_r$ correctly, then the elements of $\vec{a}_l$ can remain in the registers during the entire micro-micro operation!