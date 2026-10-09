# 附录 C

# 卷积神经网络

本书第三部分重点介绍了两种标准的神经网络类型：密集神经网络（DNN）和循环神经网络（RNN）。DNN 的魅力在于它是很好的通用逼近器，例如，书中关于强化学习的示例会使用 DNN 来逼近最优动作策略。RNN 则专门用于处理序列数据（如时间序列数据），这在尝试预测金融时间序列的未来值时很有帮助。

然而，卷积神经网络（CNN）是另一种在实践中被广泛使用的标准神经网络类型，尤其用于计算机视觉领域。CNN 能够在许多标准测试和挑战（如 ImageNet 挑战）中设立新的基准，与此相关的更多信息，请参阅《经济学家》（2016）或 Gerrish（2018）。计算机视觉在自动驾驶汽车、安全和监视等领域也很重要。

本附录简要说明了 CNN 在金融时间序列数据预测中的应用。有关 CNN 的详细信息，请参阅 Chollet（2017）的第 5 章和 Goodfellow 等（2016）的第 9 章。

# C.1 特征和标签数据

以下 Python 代码首先处理了所需的引用和自定义设置，然后导入了包含多种金融工具日线数据的数据集，该数据集在本书中被用在了不同的示例中。

```
In [1]: import os
import math
import numpy as np
import pandas as pd
from pylab import plt, mpl
plt.style.use('seaborn')

mpl.rcParams['savefig.dpi'] = 300
mpl.rcParams['font.family'] = 'serif'
os.environ['PYTHONHASHSEED'] = '0'
```