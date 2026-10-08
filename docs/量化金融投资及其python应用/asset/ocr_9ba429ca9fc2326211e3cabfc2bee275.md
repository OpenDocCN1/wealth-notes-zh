# ## 8.3 基于风险价值的蒙特卡洛方法

```
```
days = 365
dt = 1./days
mu = rets.mean()['600050']
sigma = rets.std()['600050']

def stock_monte_carlo(start_price, days, mu, sigma):
    price = np.zeros(days)
    price[0] = start_price
    shock = np.zeros(days)
    drift = np.zeros(days)

    for x in xrange(1, days):
        shock[x] = np.random.normal(loc=mu * dt, scale=sigma * np.sqrt(dt))
        drift[x] = mu * dt
        price[x] = price[x-1] + (price[x-1] * (drift[x] + shock[x]))

    return price
start_price = 2.924
#100 次蒙特卡洛模拟 (图 8-17)
for run in xrange(100):
    plt.plot(stock_monte_carlo(start_price, days, mu, sigma))
plt.xlabel("Days")
plt.ylabel("Price")
```
```

![](bbox=[0.24, 0.51, 0.62, 0.72])

```
```
#10 000 次蒙特卡洛模拟 (图 8-18)
runs = 10000
simulations = np.zeros(runs)
np.set_printoptions(threshold = 5)
for run in xrange(runs):
    simulations[run] = stock_monte_carlo(start_price, days, mu, sigma)[days - 1]

q = np.percentile(simulations, 1)
plt.hist(simulations, bins = 200)
```
```