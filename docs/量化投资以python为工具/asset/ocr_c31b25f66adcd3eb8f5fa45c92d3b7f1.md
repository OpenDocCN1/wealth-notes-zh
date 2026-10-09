```
....: ax2.bar(left1, hight1, color='r')
....: left2=Volume5.index[Close15<Open15]
....: hight2=Volume5[left2]
....: ax2.bar(left2, hight2, color='g')
Out[13]: <Container object of 34 artists>
In [14]: ax2.set_ylabel('成交量')
....: ax2.set_title('中国银行 2015 年收盘价线图')
Out[14]: <matplotlib.text.Text at 0x1fda0bc9e48>
```

![](bbox=[0.04, 0.25, 0.89, 0.61])

![](bbox=[0.04, 0.62, 0.89, 0.97])