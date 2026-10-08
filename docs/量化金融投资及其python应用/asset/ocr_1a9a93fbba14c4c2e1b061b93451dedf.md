运行上述代码，可以得到如图 11-1 所示的图形。从图 11-1 中可以看出，随着组合规模的增长，计算时间呈线性增长。

![](bbox=[0.2, 0.16, 0.75, 0.44])

# 11.2.2 使用 NumPy 向量计算

NumPy 的内置数学函数可以应用于向量：

```
```
 sample = np.linspace(1.0,100,0,5)
 np.exp(sample)
 array([ 2.71828183e+00, 1.52434373e+11, 8.54813429e+32, 4.79357761e+74,
 2.68811714e+165])
```
```

利用 NumPy 的数学函数，可以重写计算公式 call_option_pricer，使得它接受向量参数。

```
```
 #使用 NumPy 的向量函数重写 Black-Scholes 公式
 def call_option_pricer_numpay(spot, strike, maturity, r, vol):
 d1 = (np.log(spot/strike) + (r + 0.5 * vol *vol) * maturity) / vol * np.sqrt(maturity)
 d2 = d1 - vol * np.sqrt(maturity)
 
 price = spot * norm.cdf(d1) - strike * np.exp(-r * maturity) * norm.cdf(d2)
 return price
 timeSpentNumpy = []
 for size in portfolioSize:
 now = time.time()
 strikes = np.linspace(2.0,3.0, size)
 res = call_option_pricer_numpay(spot, strikes, maturity, r, vol)
 timeSpentNumpy.append(time.time() - now)
 pylab.figure(figsize = (12,8))
 pylab.bar(portfolioSize, timeSpentNumpy, color = 'r', width = 300)
 pylab.grid(True)
```
```