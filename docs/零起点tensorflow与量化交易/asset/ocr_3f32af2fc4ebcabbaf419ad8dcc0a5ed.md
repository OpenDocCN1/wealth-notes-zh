# ## 零起点 TensorFlow 与量化交易

```
dense_1 (Dense) (None, 1) 177
Total params: 125,489
Trainable params: 125,489
```

# ### 8.3.2 量化回溯

以上是数据整理环节的程序，下面开始进入第二个环节——量化回溯分析。量化分析的核心是策略，而检测策略好坏的方法就是回溯测试。第 7 组代码如下：

```
```python
# 7
print('\\n#7 bt_main')
qx=zbt.bt_main(qx)
# ztq.tq_prWrk(qx)
zt.prxk('\\nusrPools',qx.usrPools)
```
```

调用回溯主函数 bt_main，按照设定的时间周期运行回溯测试流程。如图 8-45 所示是 bt-main 函数运行完成后输出的相关信息。

![](bbox=[0.16, 0.61, 0.84, 0.93])