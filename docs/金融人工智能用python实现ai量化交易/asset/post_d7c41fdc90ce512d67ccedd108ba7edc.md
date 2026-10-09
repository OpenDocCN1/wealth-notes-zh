
## C.3 测试模型

最后，接下来的 Python 代码会将训练好的模型应用于测试数据集合。CNN 模型明显优于被动基准投资。然而，如果考虑以典型（零售）买卖价差形式出现的交易成本，大部分出色表现会被交易成本吞噬。图 C-2 显示了随时间变化的业绩。

```python
In [24]: model.evaluate(np.atleast_3d(test[col_s], test['d']))
              499/499 [=============================] - 0s 25us/step
Out[24]: [0.7364884222655653, 0.5210421085357666]
```

```python
In [25]: test['p'] = np.where(model.predict(np.atleast_3d(test[cols])) > 0.5, 1, 0)
```

```python
In [26]: test['p'] = np.where(test['p'] > 0, 1, -1) ②
```

```python
In [27]: test['p'].value_counts() ③
Out[27]: -1        478
         1        21
         Name: p, dtype: int64
```

```python
In [28]: (test['p'].diff() != 0).sum() ④
Out[28]: 41
```

```python
In [29]: test['s'] = test['p'] * test['r'] ⑤
```

```python
In [30]: ptc = 0.00012 / test[ symbol ] ⑥
```

```python
In [31]: test['s_'] = np.where(test['p'] != 0, test['s'] - ptc, test['s']) ⑥
```

```python
In [32]: test['r', 's', 's_'].sum().apply(np.exp)
Out[32]: r      0.931992
         s      1.086525
         s_     1.031307
         dtype: float64
```

```python
In [33]: test['r', 's', 's_'].cumsum().apply(np.exp).plot(figsize=(10, 6));
```

① 样本外的准确率。

② 基于预测的头寸（多头 / 空头）。

③ 头寸产生的交易数量。

④ 给定买卖差价的成比例交易成本。

⑤ 考虑交易成本之前的策略投资收益。

⑥ 考虑交易成本之后的策略投资收益。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_387_0.png)

图 C-2: 考虑交易成本之前/之后的被动基准投资和 CNN 策略投资的总体表现

## 参考文献

- [1] Chollet, François. *Deep Learning with Python*. Shelter Island: Manning, 2017.

- [2] Hilpisch, Yves. *Python for Finance: Mastering Data-Driven Finance*. 2nd ed. Sebastopol: O’Reilly, 2018.

- [3] Hilpisch, Yves. *Python for Algorithmic Trading: From Idea to Cloud Deployment*. Sebastopol: O’Reilly, 2020.

- [4] Lee, Justina, Melissa Karsh. "Machine-Learning Hedge Fund Voleon Group Returns 7% in 2019." 2020. *Bloomberg*, January 21, 2020.

- [5] López de Prado, Marcos. *Advances in Financial Machine Learning*. Hoboken, NJ: John Wiley & Sons, 2018.

- [6] Mnih, Volodymyr, et al. "Playing Atari with Deep Reinforcement Learning." arXiv. December 19, 2013.

- [7] Shiller, Robert. "The Mirage of the Financial Singularity." *Yale Insights*. July 16, 2015.

- [8] Silver, David, et al. "Mastering the Game of Go with Deep Neural Networks and Tree Search." 2016, *Nature* 529 (January): 484-489.

- [9] Alpaydin, Ethem. *Machine Learning*. MIT Press, Cambridge, 2016.

- [10] Chollet, François. *Deep Learning with Python*. Shelter Island: Manning, 2017.

- [11] Goodfellow, Ian, Yoshua Bengio, Aaron Courville. *Deep Learning*. Cambridge: MIT Press, 2016.

- [12] Kratsios, Anastasis. "Universal Approximation Theorems." 2019.

- [13] Silver, David, et al. "Mastering the Game of Go with Deep Neural Networks and Tree Search." 2016, *Nature* 529 (January): 484-489.

- [14] Shanahan, Murray. *The Technological Singularity*. Cambridge: MIT Press, 2015.

- [15] Tegmark, Max. *Life 3.0: Being Human in the Age of Artificial Intelligence*. United Kingdom: Penguin Random House, 2017.

- [16] VanderPlas, Jake. *Python Data Science Handbook*. Sebastopol: O’Reilly, 2017.

- [17] Barrat, James. *Our Final Invention: Artificial Intelligence and The End of the Human Era*. New York: St. Martin’s Press, 2013.

- [18] Bostrom, Nick. *Superintelligence: Paths, Dangers, Strategies*. Oxford: Oxford University Press, 2014.

- [19] Chollet, François. *Deep Learning with Python*. Shelter Island: Manning, 2017.

- [20] Domingos, Pedro. *The Master Algorithm: How the Quest for the Ultimate Learning Machine will Remake our World*. United Kingdom: Penguin Random House, 2015.

- [21] Doudna, Jennifer, Samuel H. Sternberg. *A Crack in Creation: The New Power to Control Evolution*. London: The Bodley Head, 2017.

- [22] Gersh, Sean. *How Smart Machines Think*. Cambridge: MIT Press, 2018.

- [23] Harari, Yuval Noah. *Homo Deus: A Brief History of Tomorrow*. London: Harvill Secker, 2015.

- [24] Kasparov, Garry. *Deep Thinking: Where Machine Intelligence Ends*. London: John Murray, 2017.

- [25] Kurzweil, Ray. *The Singularity Is Near: When Humans Transcend Biology*. New York: Penguin Group, 2005.

- [26] Kurzweil, Ray. *How to Create a Mind: The Secret of Human Thought Revealed*. New York: Penguin Group, 2012.

- [27] Mnih, Volodymyr, et al. “Playing Atari with Deep Reinforcement Learning.” 2013, arXiv. December 19, 2013.

- [28] Murgia, Madhumita, Siddharth Shrikanth. “How Governments Are Beginning to Regulate AI.” *2019, Financial Times*, May 30, 2019.

- [29] Silver, David, et al. “Mastering the Game of Go with Deep Neural Networks and Tree Search.” 2016, *Nature* 529 (January): 484-489.

- [30] Silver, David, et al. “Mastering Chess and Shogi by Self-Play with a General Reinforcement Learning Algorithm.” 2017a, arXiv. December 5, 2017.

- [31] Silver, David, et al. “Mastering the Game of Go without Human Knowledge.” 2017b, *Nature*, 550 (October): 354-359.

- [32] Shanahan, Murray. *The Technological Singularity*. Cambridge: MIT Press, 2015.

- [33] Tegmark, Max. *Life 3.0: Being Human in the Age of Artificial Intelligence*. United Kingdom: Penguin Random House, 2017.

- [34] Vinge, Vernor. “Vernor Vinge on the Singularity.” 1993.

- [35] Bender, Jennifer, et al. “Foundations of Factor Investing.” MSCI Research Insight, 2013.

- [36] Calvello, Angelo. “Fund Managers Must Embrace AI Disruption.” 2020, Financial Times, January 15, 2020.

- [37] Eichberger, Jürgen, Ian R. Harper. *Financial Economics*. New York: Oxford University Press, 1997.

- [38] Fishburn, Peter. “Utility Theory.” 1968, *Management Science* 14 (5): 335-378.

- [39] Fama, Eugene F., Kenneth R. French. “The Capital Asset Pricing Model: Theory and Evidence.” 2004, *Journal of Economic Perspectives* 18 (3): 25-46.

- [40] Halevy, Alon, Peter Norvig, Fernando Pereira. “The Unreasonable Effectiveness of Data.” 2009, *IEEE Intelligent Systems*, Expert Opinion.

- [41] Hilpisch, Yves. Derivatives Analytics with Python: Data Analysis, Models, Simulation, Calibration, and Hedging. Wiley Finance, 2015.

- [42] Jacod, Jean, Phillip Protter. Probability Essentials. 2nd ed. Berlin: Springer, 2004.

- [43] Johnstone, David, Denniss Lindley. “Mean-Variance and Expected Utility: The Borch Paradox.” 2013, Statistical Science 28 (2): 223-237.

- [44] Jones, Charles P. Investments: Analysis and Management. 12th ed. Hoboken: John Wiley & Sons, 2012.

- [45] Karni, Edi. “Axiomatic Foundations of Expected Utility and Subjective Probability.” 2014, In Handbook of the Economics of Risk and Uncertainty, edited by Mark J. Machina and W. Kip Viscusi, 1-39. Oxford: North Holland.

- [46] Lintner, John. “The Valuation of Risk Assets and the Selection of Risky Investments in Stock Portfolios and Capital Budgets.” 1965, *Review of Economics and Statistics* 47 (1): 13-37.

- [47] Markowitz, Harry. “Portfolio Selection.” 1952, *Journal of Finance* 7 (1): 77-91.

- [48] Pratt, John W. “Risk Aversion in the Small and in the Large.” 1964, *Econometrica* 32 (1/2): 122-136.

- [49] Ross, Stephen A. “Portfolio and Capital Market Theory with Arbitrary Preferences and Distributions: The General Validity of the Mean-Variance Approach in Large Markets.” 1971, Working Paper No. 12-72, Rodney L. White Center for Financial Research.

- [50] Ross, Stephen A. “The Arbitrage Theory of Capital Asset Pricing.” 1976, *Journal of Economic Theory* 13: 341-360.

- [51] Rubinstein, Mark. A History of the Theory of Investments—My Annotated Bibliography. Hoboken: Wiley Finance, 2006.

- [52] Sharpe, William F. “Capital Asset Prices: A Theory of Market Equilibrium under Conditions of Risk.” 1964, *Journal of Finance* 19 (3): 425-442.

- [53] Sharpe, William F. “Mutual Fund Performance.” 1966, *Journal of Business* 39 (1): 119-138.

- [54] Varian, Hal R. Intermediate Microeconomics: A Modern Approach. 8th ed. New York & London: W.W. Norton & Company, 2010.

- [55] von Neumann, John, Oskar Morgenstern. Theory of Games and Economic Behavior. Princeton: Princeton University Press, 1944.

- [56] Aliais, M. “Le Comportement de l’Homme Rational devant le Risque: Critique des Postulats et Axiomes de l’Ecole Americaine.” 1953, *Econometrica* 21 (4): 503-546.

- [57] Alexander, Carol. Quantitative Methods in Finance. Market Risk Analysis I, West Sussex: John Wiley & Sons, 2008a.

- [58] Alexander, Carol. Practical Financial Econometrics. Market Risk Analysis II, West Sussex: John Wiley & Sons, 2008b.

- [59] Bender, Jennifer, et al. “Foundations of Factor Investing.” 2013, MSCI Research Insight.

- [60] Campbell, John Y. Financial Decisions and Markets: A Course in Asset Pricing. Princeton and Oxford: Princeton University Press, 2018.

- [61] Ellsberg, Daniel. “Risk, Ambiguity, and the Savage Axioms.” 1961, *Quarterly Journal of Economics* 75 (4): 643-669.

368 | 参考文献
