该值处在 $[-1,+1]$ 区间内, 若取 $[-1,0)$, 则说明两资产收益率变化趋势相反; 若取 $(0,1]$, 则说明两资产收益率同向变化; 若取 0 , 则说明两资产收益率在线性关系上不相关。接下来, 我们举个例 子来看不同投资比例、不同相关系数对投资组合收益率的标准差的影响。

比如，市场有资产 $A$ 和资产 $B$ ，收益率分别为 $R_A$ 和 $R_B$ ，收益率的期望值 $\mathbb{E}(R_A)$ 、 $\mathbb{E}(R_B)$ 分别为 $8\%$ 和 $15\%$ ，标准差 $\sigma(R_A)$ 、 $\sigma(R_B)$ 分别为 $12\%$ 和 $25\%$ 。当两资产的相关系数分别为 $-1$ 、 $-0.5$ 、 $0$ 、 $+0.5$ 、 $+1$ 时，不同的投资比例下投资组合的期望收益率及风险分 别是如何变化的？根据公式，资产组合的期望收益率为：

```
$$\begin{aligned}\mathbb{E}\left(R_p\right) & =\omega_A \mathbb{E}\left(R_A\right)+\omega_B \mathbb{E}\left(R_B\right) \\& =0.08 \omega_A+0.15\left(1-\omega_A\right) \\& =0.15-0.07 \omega_A\end{aligned}$$
```

风险（收益率的标准差）为：

```
$$\begin{aligned}\sigma\left(R_p\right) & =\sqrt{\omega_A^2 \sigma^2\left(R_A\right)+\omega_B^2 \sigma^2\left(R_B\right)+2 \omega_A \omega_B \sigma\left(R_A,R_B\right)} \\& =\sqrt{0.0144 \omega_A^2+0.0625\left(1-\omega_A\right)^2+2 \omega_A\left(1-\omega_A\right) \rho_{A,B} \times 0.03}\end{aligned}$$
```

当变动 $\omega_A$ 的值时，可以得到如表 19.1 所示内容。

| A资产投 资比 | B资产投 资比 | R<sub>p</sub>-之期 望值 | 两种资产之相关系数 $\rho_{A,B}$ |   |   |     |    |

从表 19.1 的横向看, 当投资比例维持不变时, 两资产的相关系数越高, 风险越大, 这 个结果很符合直觉: 若两资产收益率同向变动, 当收益大于其期望值的情境发生时, 两资 产的收益均大于各自的期望值，反之均小于各自的期望值，总的收益变动比较剧烈；若两 资产收益率反向变动，两资产的收益一个会高于期望值，一个会低于期望值，一高一低会 使总的收益变动幅度减小，达到降低风险的作用。