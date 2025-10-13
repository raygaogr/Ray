---
tags:
  - MIT
  - BayesRules
title: Conditional Probability
date: 2024-01-08 19:10
---

---

## 条件概率

```ad-note
title: 条件概率的定义
$P(A | B) = \frac{P(A \cap B)}{P(B)}$

当$P(B) = 0$ 时，条件概率$P(A|B)$无定义

```

```ad-note
title: 全概率公式
![[Drawing 2024-01-08 19.56.00.excalidraw|200]]
$P(B) = P(A_1)P(B|A_1) + P(A_2)P(B|A_2) + P(A_3)P(B|A_3)$

```

```ad-note
title: 贝叶斯公式
$$
\begin{align}
P(A_i|B) &= \frac{P(A_i \cap B)}{P(B)} \\
           &= \frac{P(B | A_i)P(A_i)}{P(B)} \\
           &= \frac{P(B|A_i)P(A_i)}{\sum\limits_{j}P(A_j)P(B|A_j)}
           \end{align}
$$
```
 