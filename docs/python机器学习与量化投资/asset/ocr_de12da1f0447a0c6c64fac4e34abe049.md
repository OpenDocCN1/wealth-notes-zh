# 第 5 章

# 模型验证优化

通常关于模型验证优化的内容都放在较后的章节来讲，但经验主义，对于初学者而言，模型的估计（比如谷歌 TensorFlow 深度学习平台中的可视化分析模块 TensorBoard）越早学习，效果越好

在 sklearn 块件当中，内含了多项模型验证优化（Optimizers Model Validation）函数，常用的有如下几种。

+   - cross_val_score: 交叉验证评估划分。
- cross_val_predict: 对每一个输入数据点生成交叉验证评估器。
- permutation_test_score: 评估针对交叉验证测试划分的置换。
- learning_curve: 学习曲线。
- validation_curve: 验证曲线

# 5.1 交叉验证评估器

交叉验证评估器，函数名称是 cross_val_predict，可以对每一组输入数据点生成交叉验证评估器。

对于机器学习而言，通常需要 train_data（训练数据）和 test_data（测