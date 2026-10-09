```
return(spread)

def cointegration(self, priceX, priceY):
    if priceX is None or priceY is None:
        print("缺少价格对象.")
    priceX=np.log(priceX)
    priceY=np.log(priceY)
    results=sm.OLS(priceY, sm.add_constant(priceX)).fit()
    resid =results.resid
    adfSpread=ADF(resid)
    if adfSpread.pvalue>=0.05:
        print("无法形成稳定无交易关系.")
        P-value of ADF test: %f
        Coefficients of regression:
        Intercept: %f
        Beta: %f
        ''' % (adfSpread.pvalue, results.params[0], results.params[1]))
    return(None)
    else:
        print("交易价格有稳定交易关系.")
        P-value of ADF test: %f
        Coefficients of regression:
        Intercept: %f
        Beta: %f
        ''' % (adfSpread.pvalue, results.params[0], results.params[1]))
        return(results.params[0], results.params[1])

def CointegrationSpread(self, priceX, priceY, formPeriod, tradePeriod):
    if priceX is None or priceY is None:
        print("缺少价格对象.")
    if not (re.fullmatch('^d{4}-d{2}-d{2}:d{4}-d{2}-d{2}', formPeriod) or re.fullmatch('^d{4}-d{2}-d{2}:d{4}-d{2}-d{2}', tradePeriod)):
        print("形成周期交易形式错误.")
    formX=priceX[formPeriod.split(': ')[0]:formPeriod.split(': ')[1]
    formY=priceY[formPeriod.split(': ')[0]:formPeriod.split(': ')[1]
    coefficients=self.cointegration(formX, formY)
    if coefficients is None:
        print("未计算成稳定关系, 无法配对.")
    else:
        spread=(np.log(priceY[tradePeriod.split(': ')[0]:tradePeriod.split(': ')[1]]) - coefficients[0] - coefficients[1] * np.log(priceX[tradePeriod.split(': ')[0]:tradePeriod.split(': ')[1])))
    return(spread)

def calBound(self, priceX, priceY, method, formPeriod, width=1.5):
    if not (re.fullmatch('^d{4}-d{2}-d{2}:d{4}-d{2}-d{2}', formPeriod) or re.fullmatch('^d{4}-d{2}-d{2}:d{4}-d{2}-d{2}', tradePeriod)):
        print("形成期模式错误.")
    if method=='SSD':
        spread=self.SSDSpread(priceX[formPeriod.split(': ')[0]:formPeriod.split(': ')[1], priceY[formPeriod.split(': ')[0]:formPeriod.split(': ')[1]])
    mu=np.mean(spread)
    sd=np.std(spread)
    UpperBound=mu+width*sd
    LowerBound=mu-width*sd
    return(UpperBound, LowerBound)
```