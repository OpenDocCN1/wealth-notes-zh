```
    risky = self.risky.copy()
    risky = risky.resample(self.freq,convention = 'end').fillna("nearest")
    risky_return = risky.pct_change()
    portfolio = risky_return.copy()
    portfolio['risk_free'] = self.rf[self.freq] * (self.freq_coef()/365)
    portfolio.dropna(inplace = True)
    excessive_return = pd.DataFrame()
    for c in risky.columns.tolist():
        excessive_return[c] = portfolio[c] - portfolio['risk_free']
    return excessive_return*(365/self.freq_coef())  #取年化收益率
    def get_varcov(self):
        excessive_return = self.get_expected_excessive_return()
        varcov = excessive_return.cov()
        return varcov
    def general_markowitz(self):
        excessive_return = self.get_expected_excessive_return()
        varcov = self.get_varcov()
        expected_excessive_return = excessive_return.mean() * (self.freq_coef()/365)
        #利用矩阵公式直接计算权重
        weight = np.matmul(inv(varcov.values),np.array(expected_excessive_return.tolist()))/np.matmul(np.matmul(np.ones([1,top]),inv(varcov.values)),np.array(expected_excessive_return.tolist()))
        return dict(zip(excessive_return.columns.tolist(),weight)),weight,np.matmul(weight,expected_excessive_return),varcov
allocator = CapitalAllocator(rf,risky,freq)
#获得资金分配权重
allocation,weight,er,varcov = allocator.general_markowitz()
```

最终得到的部分结果如图15-7所示。

读者肯定已经发现，这个投资组和的权重十分疯狂，在A股市场下是完全不可能实现的，卖空限制以及杠杆限制使得整体的资金分配变