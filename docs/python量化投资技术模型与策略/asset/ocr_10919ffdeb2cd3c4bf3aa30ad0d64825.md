dist.pdf是指对一组数据估计出来的概率密度函数，一般调用方法为dist.pdf(x,*args,loc=0,scale=1)。其中，x是用来进行估计的数组。示例代码如下：

```
stats.norm(0,1).pdf(0)
Out[49]: 0.3989422804014327
```

# 3.dist.logpdf

dist.logpdf是指对一组数据估计出来的对数概率密度函数，一般调用方法为dist.logpdf(x,*args,loc=0,scale=1)，其中，x是用来进行估计的数组。示例代码如下：

```
stats.norm(0,1).logpdf(0)
Out[50]: -0.9189385332046727
```

# 4.dist.cdf

dist.cdf是指对一组数据估计出来的累积分布函数，一般调用方法为dist.cdf(x,*args,loc=0,scale=1)，其中，x是用来进行估计的数组。示例代码如下：

```
stats.norm(0,1).cdf(2)
Out[54]: 0.9772498680518208
```

# 5.dist.ppf