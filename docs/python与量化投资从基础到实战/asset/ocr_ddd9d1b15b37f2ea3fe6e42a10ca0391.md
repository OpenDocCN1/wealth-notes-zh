```
# 定义目标函数
def fun1(x):
    return np.matrix(x) * omega * np.matrix(x).T

def fun2(x):
    tmp = (omega * np.matrix(x).T).A1
    risk = x * tmp
    delta_risk = [sum((i - risk)**2) for i in risk]
    return sum(delta_risk)

def fun3(x):
    den = x * omega.diagonal().T
    num = np.sqrt(np.matrix(x) * omega * np.matrix(x).T)
    return num/den

# 初始值 + 约束条件
x0 = np.ones(omega.shape[0]) / omega.shape[0]
bnds = tuple((0,None) for x in x0)
cons = ({'type':'eq', 'fun': lambda x: sum(x) - 1})
options={'disp':False, 'maxiter':1000, 'ftol':1e-20}

if method == 'min variance':
    res = minimize(fun1, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'risk parity':
    res = minimize(fun2, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'max diversification':
    res = minimize(fun3, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'equal weight':
    return pd.Series(index=cov_mat.index, data=1.0 / cov_mat.shape[0])
else:
    raise ValueError('method should be min variance/risk parity/max diversification/equal weight!!! ')

# 权重调整
if res['success'] == False:
    # print res['message']
    pass
wts = pd.Series(index=cov_mat.index, data=res['x'])
```