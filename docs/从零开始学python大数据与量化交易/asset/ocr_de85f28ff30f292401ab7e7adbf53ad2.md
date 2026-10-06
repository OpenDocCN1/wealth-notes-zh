# 从零开始学 Python 大数据与量化交易

```
X,Y = np.meshgrid(phi_p, phi_m)        #numpy 的 meshgrid 函数，实现从坐标向量返回坐标矩阵
Z = flux_qubit_potential(X, Y).T        #调用自定义函数
fig = plt.figure(figsize=(12,6))
ax = fig.add_subplot(1, 1, 1, projection='3d')
p = ax.plot_wireframe(X, Y, Z, rstride=4, cstride=4)
```

单击工具栏中的 运行 按钮，运行结果如图 9.16 所示。

![](bbox=[0.13, 0.225, 0.91, 0.48])

图 9.16 绘制三维线框图示例的运行结果