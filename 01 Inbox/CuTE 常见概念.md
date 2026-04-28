---
tags:
  - CuTe
title: CuTE 同余
date: 2026-04-22 08:52
type:
---
---
## 1、Congruence
**同余**用符号 $\sim$ 表示，在 CuTE 中它用来表示两个 Tuple 是等价关系，两个 Tuple P 和 S 它们同余则说明：
$$
P \sim S \quad \text{iff} \quad P\in\mathcal{P} \: \text{and} \: S\in\mathcal{S} \: \text{or} \: P,S\in \text{Tuple} \: \text{and} \: \text{rank}(P) = \text{rank}(S) \: \text{and} \: \forall_i \: P_i \sim S_i
$$
即，这两个 Tuple 要么都是标量，要么它们的 rank 相同，并且每个子 tuple 都同余。

## 2、Weak Congruence
**弱同余**用符号 $\lesssim$ 表示，在 CuTE 中它用来表示两个 Tuple 之间的一种偏序关系，对于两个 Tuple P 和 S，它们弱同余则说明：
$$
P \lesssim S \quad \text{iff} \quad P\in\mathcal{P} \:  \text{or} \: P,S\in \text{Tuple} \: \text{and} \: \text{rank}(P) = \text{rank}(S) \: \text{and} \: \forall_i \: P_i \lesssim S_i
$$
和同余的区别在于，只需要 P 是标量，则 P 和 S 就弱同余

## 3、Coordinate set
**坐标集**是一个整数集 $\mathbb{Z}_N = \{0,1,2,\cdots,N-1\}$ 或者整数集的笛卡尔集，$\mathbb{Z}_N \times \mathbb{Z}_M = \mathbb{Z}_{(N,M)}$  

## 4、Compatibility
**相容**用符号 $\preceq$ 表示，在 CuTE 中它用来表示一组 Shape 的偏序关系，对于两组 Shapes P 和 S，P 相容于 S 则说明：
$$
P \preceq S \quad \text{iff} \quad P\in\mathbb{Z}^+ \: \text{and}\: P = |S| \: \text{or} \: P,S\in \text{Tuple} \: \text{and} \: \text{rank}(P) = \text{rank}(S) \: \text{and} \: \forall_i \: P_i \preceq S_i
$$
即，P 如果是一个正整数，它的值要等于 Shape S 的 size。P 和 S 如果都是 Tuple，则 P 和 S 的 rank 要相等，且每个子 Tuple 要相容。

## 5、Compatible coordinate sets
由 Shape S 定义的一组相容坐标集 $\mathbb{Z}(S)$ ：
$$
\mathbb{Z}(S) = \{\mathbb{Z}_{S'} \:|\: S' \preceq S\}
$$
## 6、In-bounds coordinate
由 Shape S 规定的**界内坐标**定义为：
$$
c \in \mathbb{Z}_{S'} \in \mathbb{Z}(S)
$$
## 7、Integral coordinate
由 Shape S 规定的**整型坐标**定义为：
$$
\overline{c} \in \mathbb{Z}_{|S|} \in \mathbb{Z}(S)
$$
## 8、Natural coordinate
由 Shape S 规定的**自然坐标**定义为：
$$
\tilde{c} \in \mathbb{Z}_S \in \mathbb{Z}(S)
$$
## 9、Admissible coordinate
由 Shape S 规定的**合法坐标**定义为：
$$
c \in \text{HTuple}(\mathbb{Z}) \quad \text{and} \quad c \lesssim S
$$

## 10、Out-of-bounds coordinate
由 Shape S 规定的**界外坐标**定义为：
$$
c \:\text{is a admissible coordianate and c} \notin \mathbb{Z}(S)
$$
## 11、Congruence coordinate
由 Shape S 约束成的**同余坐标**定义为：
$$

\mathbb{Z}^S = \{c \in \text{HTuple}(\mathbb{Z}) \quad | \quad c \sim S \}
$$
