# 3. 刻度、标签和图例

我们仍然通过下面的例子来说明操作方法，如图 12-5 所示。

```
```
In [47]: fig=plt.figure()
          ax=fig.add_subplot(1,1,1)
          ax.plot(np.random.randn(100).cumsum)
          ax.set_xticks([0, 25, 50, 75, 100])
          ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'])
          ax.set_yticks([-20,-10,0,10,20])
          ax.set_xlabel('Sample')
          ax.set_title('plot random walk')
Out[47]: <matplotlib.text.Text at 0x20434199eb8>
```
```

![](bbox=[0.24, 0.27, 0.77, 0.47])

图 12-5

# 4. 一个图片窗口中画多幅图并添加标签

如果我们需就 多组数据进行绘图，并在同一个图片窗口中进行比较，我们就需要做个标签，如图 12-6 所示。

```
```
In [53]: fig=plt.figure()
          ax=fig.add_subplot(1,1,1)
          ax.plot(np.random.randn(100).cumsum, color='r', label='one')
          ax.plot(np.random.randn(100).cumsum, color='g', label='two')
          ax.plot(np.random.randn(100).cumsum, color='b', label='three')
          ax.legend(loc='best')
Out[53]: <matplotlib.legend.Legend at 0x204353fe8d0>
```
```

![](bbox=[0.24, 0.71, 0.77, 0.86])

图 12-6