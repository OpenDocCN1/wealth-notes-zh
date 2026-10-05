+   - PredefinedSplit: 预定义分割交叉验证对象。

+   - TimeSeriesSplit: 时间序列交叉验证对象

# （2）Splitter Functions（分割函数）

+   - train_test_split: 训练和测试数据集分割函数

+   - check_cv: 建立交叉验证输入检查器。

# （3）Hyper-parameter（超参数优化）

+   - GridSearchCV: 网格搜索交叉验证。

+   - RandomizedSearchCV: 随机搜索交叉验证。

+   - ParameterGrid: 对行一个函数进行多参数网格搜索。

+   - ParameterSampler: 指定分布按参数抽样生成器。

+   - fit_grid_point: 按一组参数运行拟合。

# （4）optimizers Model validation（模型验证优化）

+   - cross_val_score: 交叉验证评估评分。

+   - cross_val_predict: 对给定一个输入数据点生成交叉验证评估器。

+   - permutation_test_score: 评估是否交叉验证式评分的敏感性。

+   - learning_curve: 学习曲线。

+   - validation_curve: 验证曲线。

# 9. Datasets（数据集）模块

sklearn.datasets: 数据集模块库模块，包括一些常用的测试数据集，以及数据的加载函数，同时还提供一些人工数据集生成器函数。

# （1）Loader（数据加载）

+   - clear_data_home: 清除所有加载的数据。

+   - get_data_home: 返回 scikit-learn 数据 dir 路径。

+   - fetch_20newsgroups: 抓取 20 组新闻数据集的文件名和数据。