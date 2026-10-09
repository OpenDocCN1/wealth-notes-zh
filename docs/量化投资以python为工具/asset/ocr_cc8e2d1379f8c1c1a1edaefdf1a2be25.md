+   （b）选取模型 $Y = b_0 + b_1X + b_2X^2 + \varepsilon$，$\varepsilon \sim N(0, \sigma^2)$，求出 $b_0, b_1, b_2$ 的估计值；

+   （c）取显著性水平 $\alpha = 0.05$，检验假设：$H_0: b_1 = b_2 = 0$，$H_1: b_1、b_2$ 不全为 0；

+   （d）在 $x = 95$ 时，$Y$ 的预测值.

6. 导入 CPS1988.csv，该数据集是 1988 年 3 月美国人口普查局（Census Bureau）调查的当前人口调查数据：

+   （a）查看该数据集的数据结构，解释每一个变量的经济意义；

+   （b）构建形式如下的多元回归模型：

$$\log(wage) = \beta_1 + \beta_2 experience + \beta_3 education + \beta_4 ethnicity + \varepsilon$$

+   （c）使用 summary() 函数展示模型的详细结果，并解释输出结果；

+   （d）列出模型的拟合值。

7. 构建形式如下的多元回归模型：

$$\log(wage) = \beta_1 + \beta_2 experience + \beta_3 experience^2 + \beta_4 education + \beta_5 ethnicity + \varepsilon$$

+   （a）使用 summary() 函数展示模型的详细结果，并解释输出结果；

+   （b）比较与上题的回归模型回归结果，比较两个模型的差异，说明哪一个模型更有效。

8. 对第 7 题的模型进行回归模型的显著性检验，即原假设 $H_0: \beta_1 = \beta_2 = \dots = \beta_5 = 0$。

9. 对第 7 题的模型参数 $\beta_3$ 进行显著性检验，即原假设 $H_0: \beta_3 = 0$。