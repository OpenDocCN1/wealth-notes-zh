# Logit模型估计值的解释

概率变化率的计算

$L_i = \ln\left(\frac{p_i}{1-p_i}\right) = z_i = \beta_0 + \beta_1 x_i$

$\frac{dL_i}{dx_i} = \beta_1 = \frac{dL_i}{dp_i}\frac{dp_i}{dx_i}$

$= \frac{1-p_i}{p_i}\frac{1-p_i - p_i(-1)}{(1-p_i)^2}\frac{dp_i}{dx_i} = \frac{1}{p_i(1-p_i)}\frac{dp_i}{dx_i}$

$\frac{dp_i}{dx_i} = \beta_1 p_i (1-p_i)$