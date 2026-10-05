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
```