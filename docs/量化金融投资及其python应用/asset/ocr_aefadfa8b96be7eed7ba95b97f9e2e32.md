```
scheme.roll_back()
endTime = time.time()
print('{0:<40}{1:.4f}'.format('执行时间(s) -- 使用 scipy.linalg: ', endTime - startTime)
执行时间(s) -- 不使用 scipy.linalg: 6.6161
执行时间(s) -- 使用 scipy.linalg: 1.5357
```

第 14 章和本章介绍了偏微分方程差分格式的基本知识，在此基础上，就可以处理金融工程中实际遇到的方程。在第 16 章中，将把这些知识运用到金融工程史上最重要的方程——Black-Scholes-Merton 偏微分方程中。

# ## 练习题

对本章中的例题数据，使用 Python 重新操作一遍。