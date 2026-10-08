```
info = stats.describe(dat)
print "Data size is: " + str(info[0])
print "Minimum value is: " + str(info[1][0])
print "Maximum value is: " + str(info[1][1])
print "Arithmetic mean is: " + str(info[2])
print "Unbiased variance is: " + str(info[3])
print "Biased skewness is: " + str(info[4])
print "Biased kurtosis is: " + str(info[5])
Data size is: 100
Minimum value is: -4.41884319577
Maximum value is: 5.71520945675
Arithmetic mean is: 0.165282446834
Unbiased variance is: 3.63090718776
Biased skewness is: 0.278066378117
Biased kurtosis is: 0.408791537079
```

当已知一组数据服从某些分布的时候,可以调用 fit 函数来得到对应分布参数的极大似然估计 (Maximum-Likelihood Estimation, MLE)。假设数据服从正态分布,以下代码可以得到分布参数的极大似然估计:

```
norm_dist = stats.norm(loc=0, scale=1.8)
dat = norm_dist.rvs(size=100)
mu, sigma = stats.norm.fit(dat)
print "MLE of data mean:" + str(mu)
print "MLE of data standard deviation:" + str(sigma)
MLE of data mean: -0.126592501904
MLE of data standard deviation:1.74446062629
```

pearsonr 和 spearmanr 函数可以计算 Pearson 和 Spearman 相关系数,这两个相关系数度量了两组数据的线性关联程度:

```
norm_dist = stats.norm()
dat1 = norm_dist.rvs(size=100)

exp_dist = stats.expon()
dat2 = exp_dist.rvs(size=100)
cor, pval = stats.pearsonr(dat1, dat2)
print "Pearson correlation coefficient: " + str(cor)
cor, pval = stats.spearmanr(dat1, dat2)
print "Spearman's rank correlation coefficient: " + str(cor)

Pearson correlation coefficient: -0.078269702955
Spearman's rank correlation coefficient: -0.0667146714671
```

其中的 pval 表示原假设(两组数据不相关)下相关系数的显著性。

在 SciPy 中还提供了金融数据分析中使用频繁的线性回归,下面是一个例子:

```
x = stats.chi2.rvs(3, size=50)
y = 2.5 + 1.2 * x + stats.norm.rvs(size=50, loc=0, scale=1.5)
slope, intercept, r_value, p_value, std_err = stats.linregress(x, y)
print "Slope of fitted model is:", slope
print "Intercept of fitted model is:", intercept
```