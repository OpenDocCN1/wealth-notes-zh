金融现实，则很难对金融收益率进行正态性假设。

```
```
In [28]: symbols = ['.SPX', 'AMZN.O', 'EUR=', 'GLD']
In [29]: for syn in symbols:
print(‘\n{}’.format(syn))
print(45 * ‘=’)
print_statistics(rets[syn].values)

.SPX
RETURN SAMPLE STATISTICS
Skew of Sample Log Returns -0.497160
Skew Normal Test p-value 0.000000
Kurt of Sample Log Returns 4.598167
Kurt Normal Test p-value 0.000000
Normal Test p-value 0.000000

AMZN.O
RETURN SAMPLE STATISTICS
Skew of Sample Log Returns 0.135268
Skew Normal Test p-value 0.005689
Kurt of Sample Log Returns 7.344837
Kurt Normal Test p-value 0.000000
Normal Test p-value 0.000000

EUR=
RETURN SAMPLE STATISTICS
Skew of Sample Log Returns -0.053959
Skew Normal Test p-value 0.268203
Kurt of Sample Log Returns 1.788899
Kurt Normal Test p-value 0.000000
Normal Test p-value 0.000000

GLD
RETURN SAMPLE STATISTICS
Skew of Sample Log Returns -0.581025
Skew Normal Test p-value 0.000000
```
```