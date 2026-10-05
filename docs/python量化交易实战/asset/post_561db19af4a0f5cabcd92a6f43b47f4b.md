
```
symbol_feature_columns = []
for key in train_x.keys():
    symbol_feature_columns.append(tf.feature_column.numeric_column(key=key))

classifier = tf.estimator.DNNClassifier(
    # 这个模型接受哪些输入的特征
    feature_columns=symbol_feature_columns,
    # 包含两个隐藏层，每个隐藏层包含 10 个神经元
    hidden_units=[128, 128,128],
    # 最终结果要分成几类
    n_classes=2)

def train_func(train_x,train_y):
    dataset=tf.data.Dataset.from_tensor_slices((dict(train_x), train_y))
    dataset = dataset.shuffle(1000).repeat().batch(100)
    return dataset

classifier.train(
    input_fn=lambda:train_func(train_x,train_y),
    steps=1000)

def predict_input_fn(features, labels, batch_size):
    features = dict(features)
    if labels is None:
        # No labels, use only features.
        inputs = features
    else:
        inputs = (features, labels)
    dataset = tf.data.Dataset.from_tensor_slices(inputs)

    assert batch_size is not None, "batch_size must not be None"
    dataset = dataset.batch(batch_size)
    return dataset

df = history_n(symbol, frequency="1d", end_time=last_day, count=1,
fields="open,close,high,low,volume", df=True)
    test =
[df["open"].values,df["close"].values,df["high"].values,df["low"].values,df["volume"].values]

    test_data = pd.DataFrame({'open':test[0],'close':test[1],'high':test[2],
'low':test[3],'volume':test[4]},index=[0])


predict_result = []
predictions = classifier.predict(
    input_fn=lambda:predict_input_fn(test_data,labels=None,batch_size=1))
for predict in predictions:
    predict_result.append(predict['probabilities'].argmax())

return (predict_result)
```

通过模型训练前 60 日的涨跌情况判断当日的涨跌情况，并将数据返回。若返回值是 1，则为买入信号，若返回值为 0，则为卖出信号。

## 9.4.3 使用 TensorFlow 的逻辑回归进行回测

下面使用逻辑回归的策略进行数据回测。使用的是沪深 300 指数作为买卖标的，代码如下：

【程序 9-6】
```
import numpy as np
from gm.api import *
import 单股票的逻辑回归分析 as fun
import os
import csv

import datetime
def init(context):
    schedule(schedule_func=algo, date_rule='1d', time_rule='09:31:00')
    context.symbol = "SHSE.000300"

def algo(context):
    now = context.now
    order_close_all()
    flag = fun.get_check_flag(context.symbol,now)
    print(flag)
    if flag:
        order_target_percent(symbol=context.symbol, percent=1,
        order_type=OrderType_Market,
                            position_side=PositionSide_Long)
def on_backtest_finished(context, indicator):
    pass

if __name__ == "__main__":
    run(
        strategy_id='9f53e15f-6870-11e8-801c-4cedfb681747',
        filename=(os.path.basename(__file__)),
        mode=MODE_BACKTEST,
```

```
token='e8978d765c4822e5a85fcaa73e044065cf17b58b',
backtest_start_time="2018-01-15 09:30:00",
backtest_end_time='2018-07-10 15:00:00',
backtest_initial_cash=10000000,
backtest_adjust=ADJUST_PREV
)
```

这里导入的主策略判定就是前面所设置的逻辑回归模型，根据值的判定从而决定数据的买卖。最终结果如图 9-14 所示。

![](img/66b32d04cb77b904cead314891984c93_212_0.png)

图 9-14 逻辑回归的回测结果

这里可以看到，回测曲线非常好地拟合了沪深 300 的走势，但是由于在回测期间沪深 300 为下跌状态，因此其收益为负。

此时，如果把买入指标和卖出指标互换，那么回测结果如图 9-15 所示。

![](img/66b32d04cb77b904cead314891984c93_212_1.png)

图 9-15 买卖指标互换之后的回测结果

这里可以看到，对于买卖指标进行互换，收益并没有改善多少，因此可以说，简单改变买卖指标进行股票的买卖对于收益并没有多少影响。

这里有一点需要注意，对于胜率来说，图 9-14 和图 9-15 的胜率和为 90%，而不是前面所写的 100%，这里可能是产生了数据缺失，请读者注意。

## 9.5 机器学习策略——支持向量机

本节将简略介绍支持向量机的用法。

支持向量机是机器学习的一种，相对于线性回归和逻辑回归来说，支持向量机实际上是一种有监督的机器学习算法。将机器学习算法应用于股市分类中，与上一节的逻辑回归类似，实际上也是通过训练模型学习不同因子对未来的走势的影响去做出判断。

### 9.5.1 支持向量机的基本概念

支持向量机解决的是有监督的二元分类问题（supervised binary classification）。

例如，对于股票市场来说，对于某只股，希望根据它的属性以及一系列已经分类好的历史样本，将这个新样本分到两个不同的目标类中的某一类。

股市的涨跌识别就是一个例子：一只股票在股市中每日的行情要么涨要么跌。因此，对于股票的走势判定，我们希望机器学习算法能够自动对它分类。为此，我们首先通过人工对大量的历史数据进行标识，然后用这些标识后的历史数据对机器学习算法进行训练。

在处理这类分类问题时，SVM 的作用对象是样本的特征空间（feature space），它是一个有限维度的向量空间，每个维度对应着样本的一个特征，而这些特征组合起来可以很好地描述被分类的样本。

为了对新的样本分类，SVM 算法会根据历史数据在特征空间内构建一个超平面（hyperplane），它将特征空间线性分割为两部分，对应着分类问题的两类，分别位于超平面的两侧。构建超平面的过程就是模型训练过程。对于一个给定的新样本，根据它的特征值，它会被放在超平面两侧中的某一侧，这便完成了分类。不难看出，SVM 是一个非概率的线性分类器。这是因为 SVM 模型回答的是非此即彼的问题，新样本会被确定地分到两类中的某一类。

在数学表达上，每一只股票的历史样本点由一个（X, y）元组表示，其中粗体的 X 是特征向量，即 X=（x₁, …, xₚ），而其中每一个 xⱼ 代表股票的某一个特征，而 y 代表该样本的已知分类（通常用+1 和 0 表示涨和跌）。SVM 会根据这些给定的历史数据来训练算法的参数，找到最优的线性超平面。

理想情况下，这个超平面可以将两类涨跌完美的分开（即没有错分的情况）。对于给定的训练数据，可以将它们完美分开的超平面很可能不是唯一的，比如一个超平面稍微旋转一个角度，便得到一个仍然能够完美分割的超平面。在众多能够实现分类的超平面中，只有

个是最优的。

在实际应用中，很多数据并非是线性可分的。SVM 的强大之处在于，它不仅仅局限于是一个高维空间的线性分类器。它通过非线性的核函数（kernel functions）把原始的特征空间映射到更高维的特征空间（可以是无限维的），在高维空间中再将这些样本点线性分割。高维空间的线性分割对应着原始特征空间的非线性分割，因此在原始特征空间中生成了非线性的决策边界。此外，这么做并不以增加计算机的计算负担为代价。因此，SVM 相当高效。

### 9.5.2 使用支持向量机进行回测

对于这部分内容，由于难度关系，只给出回测代码，有兴趣的读者可以自行研究完成。策略的判定：

- 选取 7 个特征变量组成滑动窗口长度为 15 天的训练集，随后训练一个二分类（上涨/下跌）的支持向量机模型。
- 若没有仓位，则在每个星期一的时候输入标的股票近 15 个交易日的特征变量进行预测，并在预测结果为上涨的时候购买标的。
- 若已经持有仓位，则在盈利大于 10% 的时候止盈，在星期五损失大于 2% 的时候止损。
- 特征变量为：收盘价/均值、现量/均量、最高价/均价、最低价/均价、现量、区间收益率、区间标准差。

【程序 9-7】
```
# coding=utf-8
from __future__ import print_function, absolute_import, unicode_literals
from datetime import datetime
import numpy as np
from gm.api import *
import sys
try:
    from sklearn import svm
except:
    print('请安装 scikit-learn 库和带 mkl 的 numpy')
    sys.exit(-1)

def init(context):
    # 订阅浦发银行的分钟 bar 行情
    context.symbol = 'SHSE.600000'
    subscribe(symbols=context.symbol, frequency='60s')
    start_date = '2016-03-01'  # SVM 训练起始时间
    end_date = '2017-06-30'  # SVM 训练终止时间
    # 用于记录工作日
# 获取目标股票的daily历史行情
recent_data = history(context.symbol, frequency='1d', start_time=start_date, end_time=end_date, fill_missing='last',
                        df=True)
days_value = recent_data['bob'].values
days_close = recent_data['close'].values
days = []
# 获取行情日期列表
print('准备数据训练SVM')
for i in range(len(days_value)):
    days.append(str(days_value[i])[0:10])
x_all = []
y_all = []
for index in range(15, (len(days) - 5)):
    # 计算三星期共15个交易日相关数据
    start_day = days[index - 15]
    end_day = days[index]
    data = history(context.symbol, frequency='1d', start_time=start_day, end_time=end_day, fill_missing='last',
                   df=True)
    close = data['close'].values
    max_x = data['high'].values
    min_n = data['low'].values
    amount = data['amount'].values
    volume = []
    for i in range(len(close)):
        volume_temp = amount[i] / close[i]
        volume.append(volume_temp)
    close_mean = close[-1] / np.mean(close)  # 收盘价/均值
    volume_mean = volume[-1] / np.mean(volume)  # 现量/均量
    max_mean = max_x[-1] / np.mean(max_x)  # 最高价/均价
    min_mean = min_n[-1] / np.mean(min_n)  # 最低价/均价
    vol = volume[-1]  # 现量
    return_now = close[-1] / close[0]  # 区间收益率
    std = np.std(np.array(close), axis=0)  # 区间标准差
    # 将计算出的指标添加到训练集x
    # features 用于存放因子
    features = [close_mean, volume_mean, max_mean, min_mean, vol, return_now, std]
    x_all.append(features)
# 准备算法需要用到的数据
for i in range(len(days_close) - 20):
    if days_close[i + 20] > days_close[i + 15]:
        label = 1
    else:
        label = 0
    y_all.append(label)
x_train = x_all[:-1]
y_train = y_all[:-1]
# 训练 SVM
context.clf = svm.SVC(C=1.0, kernel='rbf', degree=3, gamma='auto',
coef0=0.0, shrinking=True, probability=False,
                tol=0.001, cache_size=200, verbose=False, max_iter=-1,
                decision_function_shape='ovr', random_state=None)
context.clf.fit(x_train, y_train)
print('训练完成!')
def on_bar(context, bars):
    bar = bars[0]
    # 获取当前年月日
    today = bar.bob.strftime('%Y-%m-%d')
    # 获取数据并计算相应的因子
    # 于星期一的 09:31:00 进行操作
    # 当前 bar 的工作日
    weekday = datetime.strptime(today, '%Y-%m-%d').isoweekday()
    # 获取模型相关的数据
    # 获取持仓
    position = context.account().position(symbol=context.symbol,
    side=PositionSide_Long)
    # 如果 bar 是新的星期一且没有仓位，就开始预测
    if not position and weekday == 1:
        # 获取预测用的历史数据
        data = history_n(symbol=context.symbol, frequency='1d', end_time=today,
        count=15,
                        fill_missing='last', df=True)
        close = data['close'].values
        train_max_x = data['high'].values
        train_min_n = data['low'].values
        train_amount = data['amount'].values
        volume = []
        for i in range(len(close)):
            volume_temp = train_amount[i] / close[i]
            volume.append(volume_temp)
        close_mean = close[-1] / np.mean(close)
        volume_mean = volume[-1] / np.mean(volume)
        max_mean = train_max_x[-1] / np.mean(train_max_x)
        min_mean = train_min_n[-1] / np.mean(train_min_n)
```
