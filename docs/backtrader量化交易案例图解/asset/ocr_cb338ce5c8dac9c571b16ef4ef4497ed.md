# 8.11 案例：绘制多指标金融图

本节案例程序文件名是： btr_e011pltMInd.py，介绍如何绘制带有多
种专业指标的金融图。

以下是案例程序， main主流程部分的代码：

```
print('\n#1,设置 BT 量化回测程序入口')
cerebro = bt.Cerebro()

#
print('\n#2,设置 BT 回测初始参数: 起始资金等')
dmoney0=100000.0
cerebro.broker.setcash(dmoney0)
dcash0=cerebro.broker.startingcash

#
#
print('\n\t#2-2,设置数据文件, 需要按时间字段正序排序')
```