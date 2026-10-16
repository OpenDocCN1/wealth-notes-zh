# Cochran's Theorem

+   - **Cochran's Theorem**
+     Let $Z_i \sim_{i.i.d.} N(0, \sigma^2)$ for $i = 1, \ldots, v$ and
+   
+     $$\sum_{i=1}^{v} Z_i^2 = Q_1 + Q_2 + \cdots + Q_s$$
+   
+     where $s \le v$, and $Q_i$ has d.f. of $v_i$. If $v = v_1 + v_2 + \cdots + v_s$, then $Q_i$'s are independent and $Q_i / \sigma^2 \sim \chi^2(v_i)$.

+   - By Cochran's Theorem, we can show that if $H_0$ is true, then SSE and SSTR are independent and
+   
+     $$\frac{SSE}{\sigma^2} = \frac{\sum_{j=1}^{k} \sum_{i=1}^{n_j} (x_{ij} - \bar{x}_j)^2}{\sigma^2} \sim \chi^2(n_T - k)$$ 
+   
+     $$\frac{SSTR}{\sigma^2} = \frac{\sum_{j=1}^{k} n_j (\bar{x}_j - \bar{\bar{x}})^2}{\sigma^2} \sim \chi^2(k - 1)$$