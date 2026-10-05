```
by_time='M')
mean_monthly_ic.head()
```

输出结果如图14-13所示。

绘制每个月的信息系数表现图，代码如下：

```
alphalens.plotting.plot_monthly_ic_heatmap(mean_monthly_ic);
```

信息系数表现图如图14-14所示。

![](bbox=[0.23, 0.42, 0.77, 0.78])

图 14-13

从图14-14中，我们可以看到每个月的信息系数的表现。