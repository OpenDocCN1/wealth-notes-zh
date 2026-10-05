# 3.5.3 分布图

Seaborn 不仅在图形的外观上提供了有效的帮助，还提供了辅助图形，使我们更快捷地获得可视化结果。先从最简单的正态分布直方图入手：

```
In [155]:import numpy.random as npr
In [156]:size = 1000
    rn1 = npr.standard_normal(size)
```

Seaborn 中的 distplot 函数起到了类似于 Matplotlib 中的 hist 函数的功能，但更强大一些。我们通过 kde 及 rug 参数就可以选择是否显示核密度估计及边际毛毯图，效果如图 3-17 所示：

```
In [157]:sns.set_palette("muted")
    fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(10, 10))
    ax1.hist(rn1, bins=25)
    ax1.set_title('fig1')
    sns.distplot(rn1, bins=25, kde=False, ax=ax2)
    ax2.set_title('fig2')
    sns.distplot(rn1, bins=25, kde=True, ax=ax3)
    ax3.set_title('fig3')
    sns.distplot(rn1, bins=25, kde=True, rug=True, ax=ax4)
    ax4.set_title('fig4')
```

![](bbox=[0.13, 0.546, 0.848, 0.815])

图 3-17