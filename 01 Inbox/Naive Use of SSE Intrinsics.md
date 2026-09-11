---
tags: 
title: Naive Use of SSE Intrinsics
date: 2024-12-10 19:16
type: permanent-note
---
---
The implementation presented here uses [SSE intrinsics](https://software.intel.com/sites/landingpage/IntrinsicsGuide/). In my naive way of thinking I expected a compiler to produce this implementation on assembly level when optimizing the demo-pure-c micro kernel. However, no matter what attributes, optimization flags and tricks I used, the compiler never could optimize the demo-pure-c micro kernel to the performance level of this micro kernel.

## The Micro Kernel Algorithm

We merely optimize the update step, this outer product algorithm

$$\mathbf{AB} \leftarrow \mathbf{AB} + \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix} \begin{pmatrix} b_{4l}, & b_{4l+1}, & b_{4l+2}, & b_{4l+3}\end{pmatrix}$$

by using SSE intrinsics. Looking at the original C code

```c
for (l=0; l<kc; ++l) {  
    for (j=0; j<NR; ++j) {  
        for (i=0; i<MR; ++i) {  
            AB[i+j*MR] += A[i]*B[j];  
        }  
    }  
    A += MR;  
    B += NR;  
}  
```

we notice that in the most inner loop the value B[j] does not change. The natural idea is to compute this step as

$$\mathbf{AB} \leftarrow \mathbf{AB} + \begin{pmatrix} b_{4l} \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix}, & b_{4l+1} \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix}, & b_{4l+2} \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix}, & b_{4l+3} \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix} \end{pmatrix}$$

Let $\mathbb{b}_{00}, \mathbb{b}_{11}, \mathbb{b}_{22}, \mathbb{b}_{33}$  and $\mathbb{a}_{01}, \mathbb{a}_{23}$  denote SSE registers. We use this 6 registers to store the operands:

$$\begin{array}{llllll}\mathbb{b}_{00} \leftarrow \begin{pmatrix} b_{4l } \\ b_{4l } \end{pmatrix}, &\mathbb{b}_{11} \leftarrow \begin{pmatrix} b_{4l+1} \\ b_{4l+1} \end{pmatrix}, &\mathbb{b}_{22} \leftarrow \begin{pmatrix} b_{4l+2} \\ b_{4l+2} \end{pmatrix}, &\mathbb{b}_{33} \leftarrow \begin{pmatrix} b_{4l+3} \\ b_{4l+3} \end{pmatrix}, &\mathbb{a}_{01} \leftarrow \begin{pmatrix} a_{4l } \\ a_{4l+1} \end{pmatrix}, &\mathbb{a}_{23} \leftarrow \begin{pmatrix} a_{4l+2} \\ a_{4l+3} \end{pmatrix}\end{array}$$

Another 8 SSE registers denoted as $\mathbb{ab}_{\cdot,\cdot}$  are used to represent $\mathbf{AB}$ :

$$\begin{array}{llll}\mathbb{ab}_{00,10} \leftarrow \begin{pmatrix} ab_{0,0} \\ ab_{1,0} \end{pmatrix}, &\mathbb{ab}_{01,11} \leftarrow \begin{pmatrix} ab_{0,1} \\ ab_{1,1} \end{pmatrix}, &\mathbb{ab}_{02,12} \leftarrow \begin{pmatrix} ab_{0,2} \\ ab_{1,2} \end{pmatrix}, &\mathbb{ab}_{03,13} \leftarrow \begin{pmatrix} ab_{0,3} \\ ab_{1,3} \end{pmatrix} \\[0.5cm]\mathbb{ab}_{20,30} \leftarrow \begin{pmatrix} ab_{2,0} \\ ab_{3,0} \end{pmatrix}, &\mathbb{ab}_{21,31} \leftarrow \begin{pmatrix} ab_{2,1} \\ ab_{3,1} \end{pmatrix}, &\mathbb{ab}_{22,32} \leftarrow \begin{pmatrix} ab_{2,2} \\ ab_{3,2} \end{pmatrix}, &\mathbb{ab}_{23,33} \leftarrow \begin{pmatrix} ab_{2,3} \\ ab_{3,3} \end{pmatrix}\end{array}$$

As our architecture has a total of 16 SSE registers we have two registers left. We use them for temporary results and denote them as $\mathbb{tmp}_1$  and $\mathbb{tmp}_2$ .

A single update can now be computed as
- Update the first column:
    - $\mathbb{tmp}_1 \leftarrow \mathbb{a}_{01}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{a}_{23}$
    - $\mathbb{tmp}_1 \leftarrow \mathbb{tmp}_1 \odot \mathbb{b}_{00}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{tmp}_2 \odot \mathbb{b}_{00}$ 
    - $\mathbb{ab}_{00,10} \leftarrow \mathbb{ab}_{00,10} + \mathbb{tmp}_1$
    - $\mathbb{ab}_{20,30} \leftarrow \mathbb{ab}_{20,30} + \mathbb{tmp}_2$
- Update the second column:
    - $\mathbb{tmp}_1 \leftarrow \mathbb{a}_{01}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{a}_{23}$
    - $\mathbb{tmp}_1 \leftarrow \mathbb{tmp}_1 \odot \mathbb{b}_{11}$ 
    - $\mathbb{tmp}_2 \leftarrow \mathbb{tmp}_2 \odot \mathbb{b}_{11}$
    - $\mathbb{ab}_{01,11} \leftarrow \mathbb{ab}_{01,11} + \mathbb{tmp}_1$
    - $\mathbb{ab}_{21,31} \leftarrow \mathbb{ab}_{21,31} + \mathbb{tmp}_2$
- Update the third column:
    - $\mathbb{tmp}_1 \leftarrow \mathbb{a}_{01}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{ab}_{23}$
    - $\mathbb{tmp}_1 \leftarrow \mathbb{tmp}_1 \odot \mathbb{b}_{22}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{tmp}_2 \odot \mathbb{b}_{22}$
    - $\mathbb{ab}_{02,12} \leftarrow \mathbb{ab}_{02,12} + \mathbb{tmp}_1$
    - $\mathbb{ab}_{22,32} \leftarrow \mathbb{ab}_{22,32} + \mathbb{tmp}_2$
- Update the forth column:
    - $\mathbb{tmp}_1 \leftarrow \mathbb{a}_{01}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{a}_{23}$
    - $\mathbb{tmp}_1 \leftarrow \mathbb{tmp}_1 \odot \mathbb{b}_{33}$
    - $\mathbb{tmp}_2 \leftarrow \mathbb{tmp}_2 \odot \mathbb{b}_{33}$
    - $\mathbb{ab}_{03,13} \leftarrow \mathbb{ab}_{03,13} + \mathbb{tmp}_1$
    - $\mathbb{ab}_{23,33} \leftarrow \mathbb{ab}_{23,33} + \mathbb{tmp}_2$

Hereby $\odot$ denotes the usual component wise multiplication of SSE registers. We also assume that previous to the first update step all the $\mathbb{ab}$  registers are zero initialized.

Once we have completed the total of $k_c$  updates we write the result back to memory into $\mathbf{AB}$.