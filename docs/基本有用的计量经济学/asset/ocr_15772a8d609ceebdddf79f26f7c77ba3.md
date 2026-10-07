在断点局部近似于完全随机化实验，那么，我们可以识别出在断点处的平均因果效应。如果是精确断点，断点完全决定干预分配状态，则 $p^+ = 1, p^- = 0$，从而在断点处的平均因果效应为 $E[\tau_i \mid X_i = x_0] = \mu^+ - \mu^-$，即在断点处结果平均值的跳跃可以解释为干预的影响。下面我们给出该定理的证明：

证明 考虑在断点左右观测结果的期望值变化，对于 $0 < \varepsilon < \delta$,

$E[Y_i \mid X_i = x_0 + \varepsilon] - E[Y_i \mid X_i = x_0 - \varepsilon]$ 
$= E[Y_{0i} + \tau_i D_i \mid X_i = x_0 + \varepsilon] - E[Y_{0i} + \tau_i D_i \mid X_i = x_0 - \varepsilon]$
$= \{E[Y_{0i} \mid X_i = x_0 + \varepsilon] - E[Y_{0i} \mid X_i = x_0 - \varepsilon]\} + \{E[\tau_i D_i \mid X_i = x_0 + \varepsilon] - E[\tau_i D_i \mid X_i = x_0 - \varepsilon]\}$  (9.3)

其中第一行等号利用观测结果与潜在结果之间的关系

$Y_i = D_i Y_{1i} + (1 - D_i) Y_{0i} = Y_{0i} + (Y_{1i} - Y_{0i}) D_i$

当 $\varepsilon \to 0$，根据连续性假设 9.2，(9.3) 第二行第一项将趋近于 0，即

$\lim_{\varepsilon \to 0} \{E[Y_{0i} \mid X_i = x_0 + \varepsilon] - E[Y_{0i} \mid X_i = x_0 - \varepsilon]\} = 0$  (9.4)

考虑(9.3) 第二项，利用局部随机化假设 9.3，$X_i \in \delta(x_0)$，有 $\tau_i \perp D_i \mid X_i$，则

$E[\tau_i D_i \mid X_i = x_0 + \varepsilon] - E[\tau_i D_i \mid X_i = x_0 - \varepsilon]$
$= E[\tau_i \mid X_i = x_0 + \varepsilon]E[D_i \mid X_i = x_0 + \varepsilon] - E[\tau_i \mid X_i = x_0 - \varepsilon]E[D_i \mid X_i = x_0 - \varepsilon]$

两边取极限，由连续性假设 9.2 得

$\lim_{\varepsilon \to 0} \{E[\tau_i D_i \mid X_i = x_0 + \varepsilon] - E[\tau_i D_i \mid X_i = x_0 - \varepsilon]\}$
$= \lim_{\varepsilon \to 0} \{E[\tau_i \mid X_i = x_0 + \varepsilon]E[D_i \mid X_i = x_0 + \varepsilon]\} - \lim_{\varepsilon \to 0} \{E[\tau_i \mid X_i = x_0 - \varepsilon]E[D_i \mid X_i = x_0 - \varepsilon]\}$
$= E[\tau_i \mid X_i = x_0]p^+ - E[\tau_i \mid X_i = x_0]p^-$
$= E[\tau_i \mid X_i = x_0](p^+ - p^-)$  (9.5)

对(9.3) 两边求极限，并利用(9.4)和(9.5)得

$\mu^+ - \mu^- = \lim_{\varepsilon \to 0} \mu(x_0 + \varepsilon) - \lim_{\varepsilon \to 0} \mu(x_0 - \varepsilon)$
$= E[\tau_i \mid X_i = x_0](p^+ - p^-)$

从而，

$E[\tau_i \mid X_i = x_0] = \frac{\mu^+ - \mu^-}{p^+ - p^-}$

定理 9.1 依赖于局部随机化假设，即假设在断点附近干预变量的分配 $D_i$ 近似于完全随机化实验，从而排除了个人根据预期收益进行的自选择问题。这一假