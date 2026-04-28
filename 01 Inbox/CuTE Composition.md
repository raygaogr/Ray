---
tags:
  - CuTe
title: CuTE Composition
date: 2026-04-22 10:37
type:
---
---
## Composition
给定 layouts A 和 B，这两个 layouts 的组合定义为 $\text{R} = \text{A}\circ\text{B}$ ，其中
$$
\text{Domain compatibility:} \quad\quad \text{B} \preceq \text{R}
$$
$$
\text{Functinal composition:} \quad \quad \forall c\in \mathbb{Z}(B), \text{R}(c) = \text{A}(\text{B}(c))
$$
其中 $\mathbb{Z}(B)$ 是由 Shape B 生成的[[CuTE 常见概念#5、Compatible coordinate sets|相容坐标集]] 。
## Composition满足结合率
给定 layouts A、B 和 C，如果它们满足：
$$
\text{image}(C) \subseteq \mathbb{Z}(B) \quad \text{and} \quad \text{image}(B) \subseteq \mathbb{Z}(A)
$$
则有：
$$
\text{A}\circ(\text{B}\circ\text{C}) = (\text{A}\circ\text{B})\circ\text{C}
$$
## Composition 的计算方法和约束条件
### Base Case 
可以参考 [[Cute Layouts#Composition 运算]]
令 Layout $\text{B} = s:d$  ，其中 $s\in \mathbb{Z}^+$ 并且 $d\in\mathbb{N}$ ，令 Layout $\text{B} = S : D = (S_0,S_1,\cdots,S_R):(D_0,D_1,\cdots,D_R)$ 其中，$S_r\in\mathbb{Z}^+$ 并且 $D_r\in \mathcal{D}$ 。对于 Shape $S = (S_0,S_1,\cdots,S_R)$ 定义它的前序积为：
$$
\overline{S}_r = \prod_{k=0}^{r-1}S_k
$$
则，复合函数 $R=A\circ B$ 的计算如下：
$$\begin{eqnarray*}
R(i) = (A\circ B)(i) &=& (D\circ S\circ d\circ s)(i) \\
  &=& D(S(d(s(i)))) \\
  &=& \text{inner\_product}(\text{idx2crd}_S(\text{inner\_product}(\text{idx2crd}_s(i), d)), D) \\
  &=& \text{inner\_product}(\text{idx2crd}_S(\text{inner\_product}(i, d)), D) \\
  &=& \text{inner\_product}(\text{idx2crd}_S(i\cdot d), D) \\
  &=& \sum_{r=0}^{R-1}\left(\left\lfloor 
  \frac{i\cdot d}{\overline{S}_r}\right\rfloor \mod S_r
  \right) \cdot D_r + \left\lfloor \frac{i\cdot d}{\overline{S}_R} \right\rfloor\cdot D_R  \\
  &=& \sum_{r=0}^{R-1}
  \left(
  \left\lfloor 
  \frac{i}{\overline{S}_r'}
  \right\rfloor \mod S_r'
  \right) \cdot D_r' +
  \left\lfloor 
  \frac{i}{\overline{S}_R'} \right\rfloor\cdot D_R'
  
\end{eqnarray*}$$
如果 R 存在，则它被定义为 Layout $R = S' : D' = (S_0',S_1',\cdots,S_R'):(D_0,D-1,\cdots,D_R')$ 

假设有 **stride divisibility condition**:
$$
\overline{S}_r \:|\: d \quad \text{or} \quad d\:|\:\overline{S}_r \quad \forall r \in R
$$
定义：
$$
\delta_r = \left\lceil
\frac{d}{\overline{S}_r}
\right\rceil, \quad \rho_r = 
\left\lceil
\frac{\overline{S}_r}{d}
\right\rceil
$$

则有：

$$
\sum_{r=0}^{R-1}\left(
  \left\lfloor 
  \frac{i\cdot d}{\overline{S}_r}
  \right\rfloor \mod S_r
  \right) \cdot D_r + 
  \left\lfloor 
  \frac{i\cdot d}{\overline{S}_R}\right\rfloor \cdot D_R 
   = 
  \sum_{r=0}^{R-1}\left(
  \left\lfloor 
  i\cdot \frac{\delta_r}{\rho_r}
  \right\rfloor \mod S_r
  \right) \cdot D_r + 
  \left\lfloor 
  i\cdot \frac{\delta_R}{\rho_R} \right\rfloor\cdot D_R 
$$
由整除条件可知 $\delta_r = 1$ 或者 $\delta_r > 1$ 
当 $\delta_r = 1$  则 $\delta_r \:| \: S_r$
当 $\delta_r > 1$ ，由前面的整除条件可知：
$$
\overline{S}_r \cdot \delta_r = d \: | \: \overline{S}_{r+1} \implies \prod_{k=0}^{r-1} S_k \cdot \delta_r \: | \: \prod_{k=0}^{r} S_{k} \implies \delta_r \: | \: S_r
$$
因此 $\delta_r \: | \: S_r$
则：
$$
  = 
  \sum_{r=0}^{R-1}\left(
  \left\lfloor 
  i\cdot \frac{1}{\rho_r}
  \right\rfloor \mod \frac{S_r}{\delta_r}
  \right) \cdot (D_r\cdot\delta_r) + 
  \left\lfloor 
  i\cdot \frac{1}{\rho_R} \right\rfloor\cdot (D_R \cdot \delta_r)
$$

可以得到：
$$
\rho_r = \left\lceil
\frac{\overline{S}_r}{d}
\right\rceil = 
\left\lceil
\frac{\prod_{k=0}^{r-1}S_k}{d}
\right\rceil =
\left\lceil
\frac{S_0}{d}
\right\rceil
\left\lceil
\frac{\prod_{k=1}^{r-1}S_k}{\lceil d/S_0\rceil}
\right\rceil =
\cdots = \prod_{k=0}^{r-1}
\left\lceil 
\frac{S_k}{\lceil d/\overline{S_k}\rceil}
\right\rceil = 
\prod_{k=0}^{r-1}
\left\lceil 
\frac{S_k}{\delta_k}
\right\rceil = \overline{S}_r'
$$
则最后的结果 Shape 和 Stride 为：
$$
S_r' = \frac{S_r}{\delta_r} \quad
D_r' = D_r \cdot \delta_r
$$

为了满足 [[CuTE Composition#Composition|Composition的相容性条件]]，Shape 必须满足 $|S'|=\overline{S}_{R+1}'=s$ ，为了满足这个条件，则必须加上 **shape divisibility condition**:
$$
\left\lceil
\frac{\overline{S}_r}{d}
\right\rceil \: | \: s \quad \forall r \in R
$$
$$
S_r' = \frac{S_r}{\delta_r} \quad S_R'= \frac{s}{\rho_R} \quad
D_r' = D_r \cdot \delta_r
$$