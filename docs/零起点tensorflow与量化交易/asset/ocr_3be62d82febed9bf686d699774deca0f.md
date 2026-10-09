```
```text
pd.set_option('display.width', 450)
pd.set_option('display.float_format', zt.xfloat3)
rlog='~/ailib/log tmp'
if os.path.exists(rlog):tf.gfile.DeleteRecursively(rlog)
```
```

设置运行环境，清理 TensorBoard 的 log 数据目录。因为神经网络的 log 数据有些很大，会占用大量的硬盘空间，所以需要及时清理。

本书将有关机器学习的模块库数据包统一保存在 ailib/目录下，ailib 默认为根目录，而且必须与课件案例程序位于同一块硬盘上，否则可能会引起文件数据错误。

第 2 组代码如下：

```
```text
#2
print('\n#2，读取数据')
fss='data/lin_reg2.csv'
df=pd.read_csv(fss)
print(f, f, fss)
print(df.tail())
```
```

读取数据，用于神经网络模型训练和测试。

通常，基于速度、性能方面的考虑，神经网络模型使用的训练数据，内部都是采用 NumPy 的 ndarray 多维数组格式的。

但是 ndarray 多维数组非常复杂，不便于人工分析及模型设计，所以 TopQuant 极宽量化系统，以及本书相关案例采用的都是 Pandas 的 DataFrame 数据格式，在导入神经网络模型时，通过函数自动把数据转换为 ndarray 多维数组格式。

第 2 组代码对应的输出信息如下：

#2, 读取数据
f, data/lin_reg2.csv
	x	y
195 -0.552 -52.960
196 1.852 183.505
197 -0.909 -88.254
198 -0.873 -85.407
199 -1.513 -147.368

在上面输出信息中，字段 x 是输入数据，字段 y 是对应的结果数据。

数据文件 data/lin_reg2.csv 中共收录数据 200 条，源自二元一次回归方程，增加