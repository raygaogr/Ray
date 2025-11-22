---
tags: 
title: GEMM
date: 2024-12-10 15:13
type: permanent-note
---
---
## algorithm
The matrices $A, B$ and $C$ are divided into blocks:
- $A$ consists of blocks of maximum $m_c \times k_c$ size,
- $B$ consists of blocks of maximum $k_c \times n_c$ size and
- $C$ consists of blocks of maximum $m_c \times n_c$ size.

The following algorithm can be used to calculate $C$:

- Let $M = \lfloor \frac{m+m_c-1}{m_c} \rfloor$
- Let $K = \lfloor \frac{k+k_c-1}{k_c} \rfloor$
- Let $N = \lfloor \frac{n+n_c-1}{n_c} \rfloor$
- For $i=1, \dots, M$
    - For $l=1, \dots, K$
        - Copy the Block $B_{i,l}$ into the Buffer $\tilde{B}$
        - For $j=1, \dots, N$
            - Copy the block $A_{i,l}$ into the buffer $\tilde{A}$
            - If $l=1$  calculate
                - $C_{i,j} \leftarrow \beta C_{i,j} +\alpha \cdot \tilde{A} \tilde{B}$
                otherwise
                - $C_{i,j} \leftarrow C_{i,j} +\alpha \cdot \tilde{A} \tilde{B}$

### comments

- How big are the blocks?
- Why is the case distinction for $l=1$ necessary?
- You could also arrange the for loops differently, e.g.
    - First iterate over $i$, then over $j$ and then over $k$.
    - First iterate over $k$, then over $i$ and then over $j$.
    What disadvantages would these strategies have? How many variants are there?

### Example

Let us consider the multiplication of such block matrices using a small example:

$$\begin{eqnarray*}\left(\begin{array}{ccc}C_{1,1} & C_{1,2} & C_{1,3} \\C_{2,1} & C_{2,2} & C_{2,3} \\\end{array}\right)&\leftarrow&\beta\left(\begin{array}{ccc}C_{1,1} & C_{1,2} & C_{1,3} \\C_{2,1} & C_{2,2} & C_{2,3} \\\end{array}\right)+ \alpha\left(\begin{array}{cccc}A_{1,1} & A_{1,2} & A_{1,3} & A_{1,4} \\A_{2,1} & A_{2,2} & A_{2,3} & A_{2,4} \\\end{array}\right)\left(\begin{array}{cccc}B_{1,1} & B_{1,2} & B_{1,3}\\B_{2,1} & B_{2,2} & B_{2,3}\\B_{3,1} & B_{3,2} & B_{3,3}\\B_{4,1} & B_{4,2} & B_{4,3}\\\end{array}\right)\end{eqnarray*}$$

Then

$$C_{i,j} \leftarrow \beta\cdot C_{i,j} + \alpha\cdot A_{i,1} B_{1,j} + \alpha\cdot A_{i,2} B_{2,j} + \alpha\cdot A_{i,3} B_{3,j} + \alpha\cdot A_{i,4} B_{4,j}$$

Also

$$\begin{eqnarray*}C_{i,j} &\leftarrow& \beta \cdot C_{i,j} + \alpha \cdot A_{i,1} B_{1,j} \\C_{i,j} &\leftarrow& \quad\, C_{i,j} + \alpha \cdot A_{i,2} B_{2,j} \\C_{i,j} &\leftarrow& \quad\, C_{i,j} + \alpha \cdot A_{i,3} B_{3,j} \\C_{i,j} &\leftarrow& \quad\, C_{i,j} + \alpha \cdot A_{i,4} B_{4,j}\end{eqnarray*}$$

The individual products of these matrix blocks are carried out using the macro kernel. Instead of the blocks $A_{i,l}$ and $B_{l,j}$, their copies $\tilde{A}, \tilde{B}$ with possible padding are used. The product $\alpha \cdot \tilde{A} \tilde{B}$ therefore always has the dimension $m_c \times n_c$. However, if $C_{i,j}$ is an edge block, its size can be smaller. In this case, a buffer $\tilde{C}$ is used here too and $\tilde{C} \leftarrow \alpha \cdot \tilde{A} \tilde{B}$ is first calculated. The same macro kernel can therefore be used here too. Subsequently, one can update $C_{i,j}$ with the relevant part of $\tilde{C}$, ie essentially calculate $C_{i,j} \leftarrow \beta C_{i,j} + \tilde{C}$.

## Special cases in the BLAS specification

For $A \in \mathbb{R}^{m \times n}$ and $B \in \mathbb{R}^{k \times n}$  with $k=0$ , the product $C \leftarrow \beta C + \alpha AB$  should be interpreted as scaling $C \leftarrow \beta C$ .

