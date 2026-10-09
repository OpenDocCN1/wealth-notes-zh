# # 第9章 Python标准库与数据操作

Python 广受推崇与应用的原因之一是其具有众多功能强大的库（Library）。Python 自带的标准库（Standard Library）可以满足大多数基本功能需求，比如，文本处理、数值与数学运算、文件与目录管理、操作系统、代码调试等。

# ## 9.1 模块、包和库

在 Python 中，除了函数库以外，模块（Module）和包（Package）也常被人提及。在介绍 Python 函数库之前，先解释一下 Python 中模块、包和库的概念。

# ### 9.1.1 模块

模块是一种以.py 为后缀的文件，在.py 文件中定义了一些常量和函数。一般来说，这些常量和函数组合在一起能够满足某种特定功能。举例来说，拿到某一股票交易的日内交易价格数据之后，需要找出该股票交易日内的开盘价、收盘价、最高价和最低价。打开文本编辑器，分别定义函数求这四种价格信息，代码如下：

```
#priceSequence为某一股票交易日内开市后的日内成交价序列

#求开盘价
# 将股票日内交易一天内第一笔成交的成交价格定义为“开盘价”
def OpenPrice(priceSequence):
	Open=priceSequence[0]
	return(Open)

#求收盘价
# 将股票日内交易的最后一笔成交的成交价格定义为“收盘价”
def ClosePrice(priceSequence):
	Close=priceSequence[-1]
	return(Close)

#求最高价
# 将股票日内交易价格序列的最大值定义为“最高价”
def HighPrice(priceSequence):
	High=priceSequence[0]
	for price in priceSequence:
		if price>High:
			High=price
	return(High)

#求最低价
# 将股票日内交易价格序列的最大值定义为“最高价”
```