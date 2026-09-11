### 1. 目标函数
#### 平均价值
$$\begin{equation}
J(\theta) = \bar{v}_\pi = \sum_{s\in S} d(s)v_\pi(s) \tag{1}
\end{equation}$$


$$J(\theta) =\mathbb{E}[\sum_{t=0}^\infty \gamma ^tR_{t+1}] \tag{2}$$
公式 $(1)$ 和 $(2)$ 是等价的，证明如下：
$$\begin{align}
\mathbb{E}[\sum_{t=0}^\infty \gamma ^tR_{t+1}] &= \sum_{s\in S}d(s)\mathbb{E}[\sum_{t=0}^\infty \gamma ^tR_{t+1} | s] \\
&=\sum_{s\in S}d(s)v_\pi(s)
\end{align}$$
### 2. 目标函数的梯度
#### 通用梯度表达式
$$\nabla_\theta J(\theta) = \sum_{s\in S} \eta(s)\sum_{a \in A} \nabla_\theta\pi(a|s,\theta)q_\pi(s,a) \tag{3}$$
上式等价于：
$$\nabla_\theta J(\theta) = \mathbb{E}_{S\sim \eta,A\sim \pi(S,\theta)} [\nabla_\theta \ln \pi(A|S,\theta)q_\pi(S, A)]$$
#### 价值函数的梯度
$$\begin{align}
\nabla_\theta v_\pi(s) &= \nabla_\theta(\sum_{a\in A}\pi(a|s,\theta)q_\pi(s,a)) \\
&=\sum_{a\in A}(\nabla_\theta \pi(a|s,\theta)q_\pi(s,a) + \pi(a|s, \theta)\nabla_\theta q_\pi(s,a)) \tag{4}
\end{align}$$
其中 $q_\pi(s,a)$ 是action value
$$q_\pi(s,a) = \sum_{r\in R(s,a)}rp(r|s,a) + \gamma\sum_{s'\in S}p(s'|s,a)v_\pi(s')$$
上式的第一项与 $\pi$ 无关，因此梯度为0，则：
$$\nabla_\theta q_\pi(s,a) = \gamma\sum_{s'\in S} p(s'|s,a)\nabla_\theta v_\pi(s') \tag{5}$$
将 $(5)$, 带入 $(4)$ 中：
$$\begin{align}
\nabla_\theta v_\pi(s) 
&=\sum_{a\in A}\nabla_\theta \pi(a|s,\theta)q_\pi(s,a) + \gamma\sum_{a\in A}\pi(a|s, \theta)\sum_{s'\in S} p(s'|s,a)\nabla_\theta v_\pi(s') \\
&= \sum_{a\in A}\nabla_\theta \pi(a|s,\theta)q_\pi(s,a) + \gamma \sum_{s' \in S} p(s'|s)\nabla_\theta v_\pi(s') \\
&= \sum_{a\in A}\nabla_\theta \pi(a|s,\theta)q_\pi(s,a) + \gamma [P_\pi]_{ss'}\nabla_\theta v_\pi(s') \tag{6} 
\end{align}$$

令 $u(s) = \sum_{a\in A}\nabla_\theta \pi(a|s,\theta)q_\pi(s,a)$ ,则 $(6)$ 的矩阵形式为：

$$\begin{align}
\underbrace{ 
\left[ 
\begin{matrix} 
\vdots \\ 
\nabla_{\theta} v_{\pi}(s) \\ 
\vdots 
\end{matrix}
\right]
}_{\nabla_{\theta} v_{\pi} \in \mathbb{R}^{mn}} 
 &= 
 \underbrace{
 \left[ 
 \begin{matrix} 
 \vdots \\
 u(s)  \\
 \vdots 
 \end{matrix}
 \right]
 }_{u \in \mathbb{R}^{mn}} 
  + \gamma (P_{\pi} \otimes I_{m}) +
  \underbrace{
  \left[ 
  \begin{matrix} 
  \vdots \\ 
  \nabla_{\theta} v_{\pi}(s') \\ 
  \vdots \\
  \end{matrix}
  \right]
  }_{\nabla_{\theta} v_{\pi} \in \mathbb{R}^{mn}} \\
\nabla_{\theta} v_{\pi} &= u + \gamma (P_\pi \otimes I_m) \nabla_\theta v_\pi
\end{align}$$
其中，$n=|\mathcal{S}|$ ，$m$ 是参数 $\theta$ 的维度信息
$$\begin{align}
\nabla_{\theta} v_{\pi} &= u + \gamma (P_\pi \otimes I_m) \nabla_\theta v_\pi \\
&=(I_{nm} -\gamma P_\pi \otimes I_m)^{-1}u \\
& = (I_n \otimes I_m -\gamma P_\pi\otimes I_m) ^{-1}u \\
& = [(I_n - \gamma P_\pi) \otimes I_m]^{-1}u
\end{align}$$
则对于任意 $s$ ，上式为：
$$\begin{align}
\nabla_{\theta} v_{\pi}(s) 
& = \sum_{s'\in S}[(I_n - \gamma P_\pi)^{-1}]_{ss'}u(s') \\
& = \sum_{s'\in S}[(I_n - \gamma P_\pi)^{-1}]_{ss'}\sum_{a\in A}\nabla_\theta \pi(a|s',\theta)q_\pi(s',a)
\end{align}$$

其中 $\sum_{s'\in S}[(I_n - \gamma P_\pi)^{-1} ]_{ss'} = [I]_{ss'} + \gamma [P_\pi]_{ss'} + \gamma^2 [P_\pi^2]_{ss'} + \cdots = \sum_{k=0}^\infty\gamma^k [P_\pi^k]_{ss'}$   