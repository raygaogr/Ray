### State Value
考虑一个过程：
$$S_t\stackrel{A_t}\longrightarrow R_{t+1},S_{t+1}\stackrel{A_{t+1}}\longrightarrow R_{t+2},S_{t+2},...$$
它的discounted return为：
$$G_t=\sum_{k=0}^{\infty}\gamma^kR_{t+1+k}$$

用来反映从某个状态 $s$ 出发，采用某个策略 $\pi$，能获取到的 Return 的期望，可以用于评估策略的好坏，它的定义是：
$$v_\pi(s) = \mathbb{E}[G_t|S_t=s]$$
### Bellman 公式

首先 $$G_t = R_{t+1} + \gamma G_{t+1}$$
则 对 $\forall s \in S$
$$\begin{align}
v_\pi(s) &= \mathbb{E}[R_{t+1}+\gamma G_{t+1}|S_t=s] \\
& = \mathbb{E}[R_{t+1}|S_t=s] + \gamma\mathbb{E}[G_{t+1}|S_t=s] \\
& = \sum_{a \in A(s)} \pi(a|s) \mathbb{E} [R_{t+1} | s, a] + \gamma \sum_{s'\in S}p(s'|s)\mathbb{E}[G_{t+1}|S_t=s,S_{t+1} =s'] \\
& = \sum_{a \in A(s)} \pi(a|s) \sum_{r \in R(s,a)} p(r|s,a) r + \gamma \sum_{s'\in S}p(s'|s)\mathbb{E}[G_{t+1}|S_{t+1} =s'] \\
& = \sum_{a \in A(s)} \pi(a|s) \sum_{r \in R(s,a)} p(r|s,a) r + \gamma \sum_{s'\in S}p(s'|s,a)\sum_{a\in A(s)}\pi(a|s)v_\pi(s') \\
& = \sum_{a} \pi(a|s) (\sum_{r} p(r|s,a) r + \gamma \sum_{s'}p(s'|s,a)v_\pi(s')) 
\end{align}$$
上式为贝尔曼公式，描述了不同状态下的 state value的关系，它包含了两项内容，前面一项描述了当前时刻获得reward的期望，后面一项描述了未来状态能获得到的return的期望；

### Bellman 公式求解

#### Bellman公式的矩阵形式
首先将上面的bellman公式进行重写
$$\begin{align}
v_\pi(s) &=\sum_{a \in A(s)} \pi(a|s) \sum_{r \in R(s,a)} p(r|s,a) r + \gamma \sum_{s'\in S}p(s'|s,a)\sum_{a\in A(s)}\pi(a|s)v_\pi(s') \\
&=r_\pi(s) + \gamma\sum_{s'} p_\pi(s'|s) v_\pi(s')
\end{align}$$
当考虑n个状态时，则上式可以写为：
$$v_\pi(s_i) = r_\pi(s_i) + \gamma \sum_{s_j} p_\pi(s_j|s_i) v_\pi(s_j)$$
写成矩阵形式为：
$$v_\pi = r_\pi + \gamma P_\pi v_\pi$$
其中，
$v_\pi = [v_\pi(s1),...,v_\pi(s_n)]^T \in \mathbb{R}^n$ 
$r_\pi = [r_\pi(s1),...,r_\pi(s_n)]^T \in \mathbb{R}^n$ 
$P_\pi \in \mathbb{R}^{n\times n}$ , where $P_\pi|_{ij} = p_\pi(s_j|s_i)$ 是状态转移矩阵

#### 上式的求解
**1、直接求解析解：**
$$v_\pi=(I - \gamma P_\pi)^{-1}r_\pi$$
上式有解，因为$(I= \gamma P_{\pi})$ 可逆

**2、迭代求解：**
$$v_{k+1} = r_\pi + \gamma P_\pi v_k$$
上式的含义：
当 $k \rightarrow \infty$  时，$v_{k} = v_{\pi} = (I - \gamma P_{\pi})^{-1}r_{\pi}$ ，证明如下：
令 $\delta_{k} = v_k - v_{\pi}$ ，则有：
$$\begin{align}
\delta_{k+1} + v_{\pi} &=  r_{\pi} + \gamma P_{\pi}(\delta_k + v_{\pi}) \\
\delta_{k+1} &= r_\pi - v_\pi + \gamma P_\pi \delta_k + \gamma P_\pi v_\pi \\
\delta_{k+1} &= \gamma P_\pi \delta_k
\end{align}$$
因为$\gamma$ 和 $P_\pi$ 都是$(0,1)$ ，因此 $\lim_{k\rightarrow \infty} \delta_{k+1} = 0$  ，即 $\lim_{k \rightarrow \infty} v_{k+1} - v_\pi = 0$ 
### Action Value
$$q_\pi(s,a)=\mathbb{E}[G_{t}|S_t=s,A_t=a]$$
它与State Value的关系：
$$v_\pi(s)=\mathbb{E}[G_t|s]=\sum_a \pi(a|s)\mathbb{E}[G_{t} | s,a]=\sum_a \pi(a|s)q_\pi(s,a)$$
State Value是Action Value的加权平均。
又因为：
$$v_\pi(s)=\sum_a\pi(a|s)(\sum_rrp(r|s,a) + \gamma\sum_{s'}p(s'|s,a)v_\pi(s'))$$
则：
$$\begin{align}
q_\pi(s,a) &= \sum_rrp(r|s,a) + \gamma\sum_{s'}p(s'|s,a)v_\pi(s') \\
&= \sum_rrp(r|s,a) + \gamma\sum_{s'}p(s'|s,a)\sum_a \pi(a'|s')q_\pi(s',a')
\end{align}$$