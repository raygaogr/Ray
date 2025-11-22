---
tags: 
title: Another SSE Intrinsics Approach
date: 2024-12-10 21:57
type: permanent-note
---
---
In this section we follow the idea of BLIS micro kernel for i86-64 architectures with SSE. Compared to the original BLIS micro kernel we simplify quite a few things:

- We are using [SSE intrinsics](https://software.intel.com/sites/landingpage/IntrinsicsGuide/) instead of inline assembler.
    
- We do not take the latency of SSE instructions into account.
    
- We do not unroll the update loop
    
- We do not prefetch panels and other data. At least not explicitly.
## The Micro Kernel Algorithm
Again we consider the update step

$$\mathbf{AB} \leftarrow \mathbf{AB} + \begin{pmatrix} a_{4l} \\ a_{4l+1} \\ a_{4l+2} \\ a_{4l+3}\end{pmatrix} \begin{pmatrix} b_{4l}, & b_{4l+1}, & b_{4l+2}, & b_{4l+3}\end{pmatrix}$$

And again we will keep the complete matrix $\mathbf{AB}$ in eight SSE registers. However, we will do this with a slight modification that we illustrate later.

Remember that in our so called _naive_ approach the 64-bit operand $b_{4l}$  was duplicated and stored in a 128-bit SSE register. This time we will store the two 64-bit operands $b_{4l}$ and $b_{4l+1}$ in a common 128-bit SSE register. That is basically the main difference. More precise we store the operands in SSE registers $\mathbb{tmp}_0$  to $\mathbb{tmp}_3$ 

$$\begin{array}{llll}\mathbb{tmp}_{0} \leftarrow \begin{pmatrix} a_{4l } \\ a_{4l+1} \end{pmatrix}, &\mathbb{tmp}_{1} \leftarrow \begin{pmatrix} a_{4l+2} \\ a_{4l+3} \end{pmatrix}, &\mathbb{tmp}_{2} \leftarrow \begin{pmatrix} b_{4l } \\ b_{4l+1} \end{pmatrix}, &\mathbb{tmp}_{3} \leftarrow \begin{pmatrix} b_{4l+2} \\ b_{4l+3} \end{pmatrix}, &\end{array}$$

Now we notice that a component wise SSE multiplication like $\mathbb{tmp}_{0} \odot \mathbb{tmp}_{2}$  computes $\begin{pmatrix} a_{4l} b_{4l} \\ a_{4l+1} b_{4l+1} \end{pmatrix}$  which contributes to $\mathbf{AB}_{0,0}$  and $\mathbf{AB}_{1,1}$ . With this registers further contributions can be computed for $(\mathbf{AB}_{2,0}, \mathbf{AB}_{3,1})$ , $(\mathbf{AB}_{0,2}, \mathbf{AB}_{1,3})$  and $(\mathbf{AB}_{2,2}, \mathbf{AB}_{3,3})$ .

For the remaining entries of $\mathbf{AB}$  contributions can be computed after swapping $\mathbb{tmp}_{2}$ and $\mathbb{tmp}_{3}$:

$$\begin{array}{llll}\mathbb{tmp}_{4} \leftarrow \begin{pmatrix} b_{4l+1} \\ b_{4l } \end{pmatrix}, &\mathbb{tmp}_{5} \leftarrow \begin{pmatrix} b_{4l+3} \\ b_{4l+2} \end{pmatrix}, &\end{array}$$ 

Using eight SSE registers for $\mathbf{AB}$  we have two SSE registers $\mathbb{tmp}_6$  and $\mathbb{tmp}_7$  left for intermediate results.

Denoting elements of $\mathbf{AB}$ with $\mathbb{ab}_{\cdot,\cdot}$  we update diags and anti-diags in the first two columns with

$$\begin{array}{lll}\mathbb{tmp}_6 &\leftarrow& \mathbb{tmp}_2 \\\mathbb{tmp}_2 &\leftarrow& \mathbb{tmp}_2 \odot \mathbb{tmp}_0 \\\mathbb{tmp}_6 &\leftarrow& \mathbb{tmp}_6 \odot \mathbb{tmp}_0 \\\mathbb{ab}_{00,11} &\leftarrow& \mathbb{ab}_{00,11} + \mathbb{tmp}_2 \\\mathbb{ab}_{20,31} &\leftarrow& \mathbb{ab}_{20,31} + \mathbb{tmp}_6 \\& & \\\mathbb{tmp}_7 &\leftarrow& \mathbb{tmp}_4 \\\mathbb{tmp}_4 &\leftarrow& \mathbb{tmp}_4 \odot \mathbb{tmp}_0 \\\mathbb{tmp}_7 &\leftarrow& \mathbb{tmp}_7 \odot \mathbb{tmp}_0 \\\mathbb{ab}_{01,10} &\leftarrow& \mathbb{ab}_{01,10} + \mathbb{tmp}_4 \\\mathbb{ab}_{21,30} &\leftarrow& \mathbb{ab}_{21,30} + \mathbb{tmp}_7 \\\end{array}$$

Analogously we compute updates for the last two columns of $\mathbf{AB}$ 

$$\begin{array}{lll}\mathbb{tmp}_6 &\leftarrow& \mathbb{tmp}_3 \\\mathbb{tmp}_3 &\leftarrow& \mathbb{tmp}_3 \odot \mathbb{tmp}_0 \\\mathbb{tmp}_6 &\leftarrow& \mathbb{tmp}_6 \odot \mathbb{tmp}_0 \\\mathbb{ab}_{00,11} &\leftarrow& \mathbb{ab}_{00,11} + \mathbb{tmp}_2 \\\mathbb{ab}_{20,31} &\leftarrow& \mathbb{ab}_{20,31} + \mathbb{tmp}_6 \\& & \\\mathbb{tmp}_7 &\leftarrow& \mathbb{tmp}_5 \\\mathbb{tmp}_5 &\leftarrow& \mathbb{tmp}_5 \odot \mathbb{tmp}_0 \\\mathbb{tmp}_7 &\leftarrow& \mathbb{tmp}_7 \odot \mathbb{tmp}_0 \\\mathbb{ab}_{01,10} &\leftarrow& \mathbb{ab}_{01,10} + \mathbb{tmp}_4 \\\mathbb{ab}_{21,30} &\leftarrow& \mathbb{ab}_{21,30} + \mathbb{tmp}_7 \\\end{array}$$

When copying $\mathbb{ab}_{\cdot,\cdot}$  back two memory we have to move lower and hight double separately. For example the lower double of $\mathbb{ab}_{00,11}$  gets moved to $\mathbf{AB}_{0,0}$  and the higher double to $\mathbf{AB}_{1,1}$ .
