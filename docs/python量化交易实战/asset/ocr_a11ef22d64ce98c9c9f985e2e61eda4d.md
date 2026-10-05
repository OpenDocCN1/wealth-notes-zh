```
def train_func(train_x,train_y):
    dataset=tf.data.Dataset.from_tensor_slices((dict(train_x), train_y))
    dataset = dataset.shuffle(1000).repeat().batch(100)
    return dataset
```

train_func 是准备训练集函数，这里设置了训练参数和训练结果值，并且对数据进行了打乱处理，最终将打乱后的结果返回。

```
classifier.train(
    input_fn=lambda:train_func(train_x,train_y),
    steps=1000)
```

下面是训练函数，这里使用上文的训练数据集进行输出。

```
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
```

这里是输出函数，将待测数据放入函数中，使用训练好的模型和参数进行测试。

整体代码段如下：

```
def get_check_flag(symbol,now):
    last_day = get_previous_trading_date("SHSE",now)
    last_last_day = get_previous_trading_date("SHSE",last_day)
    df = history_n(symbol, frequency="1d", end_time=last_last_day, count=60,
fields="open,close,high,low,volume", df=True)
    df["flag"] = 0
    for _ in range(1, len(df)):
        res = (df.loc[_ - 1, "close"]) - (df.loc[_ - 1, "open"])
        if (res) > 0:
            df.loc[_, "flag"] = 1
    train_x, train_y = df, df.pop('flag')
```