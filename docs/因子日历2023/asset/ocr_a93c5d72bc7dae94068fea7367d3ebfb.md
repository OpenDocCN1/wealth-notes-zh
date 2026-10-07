# 盈余持续性

财务质量因子-盈利质量

```
$X_{j,t} = \phi_{0,j} + \phi_{1,j}X_{j,t-1} + v_{j,t}$
```

# 其中

+   ① 对年度收益进行如上的一阶自回归AR(1)，$\phi_{1,j}$即为盈余持续性衡量指标；

+   ② 年度收益 $X_{j,t}$ 可以取每股收益、净利润等指标；

+   ③ 自回归滚动窗口可以取10个财年。

# 说明

$\phi_{1,j}$ 取值越高，盈余持续性越强，如果 $\phi_{1,j}$ 接近1，意味着企业有高度持续的益；如果 $\phi_{1,j}$ 接近0，意味着企业的收益短暂而不可持续。

# 【参考文献】

Francis, Jennifer, Ryan LaFond, Per M. Olsson, Katherine Schipper, 2004, Cost of equity and earnings attributes, Accounting Review 79, 967-1010.