BaggingClassifier 模型被实例化为许多相同的基础估计器。

# 分布式学习

从某种意义上说，装袋在许多神经网络（或其他模型）之间分配学习任务。比如，每个神经网络只能看到训练集的某些数据和部分特征。这避免了单个神经网络过拟合完整训练集的风险。预测结果基于所有进行选择性训练的神经网络的输出综合得出。

# 7.7 优化器

Keras 包提供了一系列可与 Sequential 模型结合使用的优化器（optimizer）。不同的优化器可能会表现出不同的性能，包括训练时间和预测准确率。以下 Python 代码使用不同的优化器并对其性能进行了基准测试。在所有情况下，都应该使用 Keras 的默认参数化。样本外性能变化不大。然而，不同优化器的样本内性能差异很大。

```
In [84]: import time
```

```
In [85]: optimizers = ['sgd', 'rmsprop', 'adagrad', 'adadelta',
'adam', 'adamax', 'nadam']
```

```
In [86]: %time
for optimizer in optimizers:
    set_seeds()
    model = create_model(hl=1, hu=128,
            dropout=True, rate=0.3,
            regularizer=False, reg=l2(0.001),
            optimizer=optimizer
        )
    t0 = time.time()
    model.fit(train_[cols], train['d'],
                epochs=50, verbose=False,
                validation_split=0.2, shuffle=False,
                class_weight=class_weight)
    t1 = time.time()
    t = t1 - t0
    acc_tr = model.evaluate(train_[cols], train['d'], verbose=False)[1]
    acc_te = model.evaluate(test_[cols], test['d'], verbose=False)[1]
    out = f'{optimizer:10$} | time[s]: {t:.4f} | in-sample={acc_tr:.4f}
      out-of-sample={acc_te:.4f}'
    print(out)

sgd	| time[s]: 2.8092 | in-sample=0.6363 | out-of-sample=0.6568
rmsprop	| time[s]: 2.9480 | in-sample=0.7690 | out-of-sample=0.6613
adagrad	| time[s]: 2.8472 | in-sample=0.6747 | out-of-sample=0.6499
adadelta	| time[s]: 3.2068 | in-sample=0.7279 | out-of-sample=0.6522
adam	| time[s]: 3.2364 | in-sample=0.7365 | out-of-sample=0.6545
adamax	| time[s]: 3.2465 | in-sample=0.6982 | out-of-sample=0.6476
nadam	| time[s]: 4.1275 | in-sample=0.7944 | out-of-sample=0.6590
CPU times: user 35.9 s, sys: 4.55 s, total: 40.4 s
Wall time: 23.1 s
```