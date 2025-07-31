### 最优策略的定义（optimal policy）

策略的好坏被定义为，如果：
$$v_{\pi_1}(s) >  v_{\pi_2}(s)  \qquad  \forall s \in \mathcal{S}$$
则我们说 $\pi_1$ 由于 $\pi_2$ ，于是最优策略 $\pi^*$可以表示成：
$$v_{\pi^*}(s) \geq v_{\pi_i}(s) \qquad \forall s \in S$$
### Bellman 最优公式（标量形式）
对于每一个 $s \in S$ ，Bellman最优公式的标量形式定义如下：
$$\begin{align}
v(s) &= \max_{\pi(s) \in \Pi(s)} \sum_a\pi(a|s)(\sum_r rp(r|s,a) + \sum_{s'}p(s'|s,a)v(s')) \\
&= \max_{\pi(s) \in \Pi(s)}\sum_a \pi(a|s) q(s,a)
\end{align}$$

### Bellman 最优公式的求解
首先考虑当为某个固定的策略 $\pi$ 时，state value的最大值为：
$$\sum_a \pi(a|s)q(s,a) \leq \sum_a\pi(a|s)\max_{a} q(s,a) = \max_a q(s,a)$$
其中：
$$\begin{equation}
\pi(a|s) = 
\begin{cases}
1, \qquad a = a^*; \\
0, \qquad a \neq a^*;
\end{cases}
\end{equation}$$
其中，$a^* = \arg\max_a q(s,a)$ 

### Bellman最优公式（矩阵形式）
$$v = \max_{\pi \in \Pi}(r_\pi + \gamma P_\pi v)$$
则有：
$$f(v) = v$$
### Bellman 矩阵形式公式求解
使用 Contraction Mapping Theorem 求解
需要证明 BOE 函数 $f(v)$ 是一个 Contraction Mapping，对任意的 $v_1, v_2 \in \mathbb{R}^{|S|}$ :
$$||f(v_1) - f(v_2)||_{\infty} = \gamma ||v_1 - v_2||_{\infty}$$
其中，$\gamma$ 是折扣因子，$\gamma \in (0,1)$ , $||\cdot||_{\infty}$ 是max norm。
证明：
	 假设 $\pi_1^* = \arg\max_\pi(r_\pi + \gamma P_\pi v_1)$ , $\pi_2^* = \arg\max_\pi(r_\pi + \gamma P_\pi v_2)$
	 $f(v_1) = r_{\pi_1^*} + \gamma P_{\pi_1^*} v_1 \geq r_{\pi_2^*} + \gamma P_{\pi_2^*} v_1$ 
	 $f(v_2) = r_{\pi_2^*} + \gamma P_{\pi_2^*} v_2 \geq r_{\pi_1^*} + \gamma P_{\pi_1^*} v_2$ 
	 则：
	 $$\begin{align}
	 f(v_1) - f(v_2) &= r_{\pi_1^*} + \gamma P_{\pi_1^*} v_1 - r_{\pi_2^*} - \gamma P_{\pi_2^*} v_2 \\
	 & \geq r_{\pi_2^*} + \gamma P_{\pi_2^*} v_1 - r_{\pi_2^*} - \gamma P_{\pi_2^*} v_2 \\
	 & = \gamma P_{\pi_2^*} (v_1 - v_2)
	 \end{align}$$
	 同理可证：
	  $$\begin{align}
	 f(v_1) - f(v_2) &= r_{\pi_1^*} + \gamma P_{\pi_1^*} v_1 - r_{\pi_2^*} - \gamma P_{\pi_2^*} v_2 \\
	 & \leqq r_{\pi_1^*} + \gamma P_{\pi_1^*} v_1 - r_{\pi_1^*} - \gamma P_{\pi_1^*} v_2 \\
	 & = \gamma P_{\pi_1^*} (v_1 - v_2)
	 \end{align}$$
	 因此有：
	 $$\gamma P_{\pi_2^*} (v_1 - v_2) \leq f(v_1) - f(v_2) \leq \gamma P_{\pi_1^*} (v_1 - v_2)$$
	 令 $z = \max \{\gamma P_{\pi_1^*} |v_1 - v_2|, \gamma P_{\pi_2^*} |v_1 - v_2|\}$ ，则有
	 $$ -z \leq f(v_1) - f(v_2) \leq z$$
	 即：
	 $$|f(v_1) - f(v_2)| \leq z$$
	 然后有:
	 $$||f(v_1) - f(v_2)||_{\infty} \leq ||z||_{\infty}$$
	 又因为：
	 $$|P_{\pi_1^*}(v_1-v_2)| = P_{\pi_1^*}|v_1 - v2| \leq |v_1 - v_2|$$
	 同理 
	 $$|P_{\pi_2^*}(v_1-v_2)| = P_{\pi_2^*}|v_1 - v2| \leq |v_1 - v_2|$$
	 则有：
	 $$||f(v_1) - f(v_2)||_{\infty} \leq ||z||_{\infty} \leq ||v_1 - v_2||_{\infty}$$
	 所以 $f(v)$ 是一个 Contraction Mapping

