
# Python 机器学习与量化投资

## 10. Decomposition (矩阵分解)

sklearn.decomposition: 矩阵分解模块。包括矩阵分解方法。例如 PCA、ICA 或 NMF 等方法。该模块可作为降维使用

- PCA: 主成分分析
- IncrementalPCA: 增量主成分分析
- KernelPCA: 核主成分分析 (KPCA)
- FactorAnalysis: 因子分析 (FA)
- FastICA: 快速独立成分分析方法
- TruncatedSVD: 使用低秩奇异值分解降维 (又称作 LSA)
- NMF: 非负矩阵分解
- SparsePCA: 稀疏主成分分析
- MiniBatchSparsePCA: 小批量稀疏主成分分析
- SparseCoder: 稀疏编码
- DictionaryLearning: 学习词典
- MiniBatchDictionaryLearning: 小批量学习词典
- LatentDirichletAllocation: 潜在狄利克雷分配变分贝叶斯算法
- FastICA: 快速独立成分分析
- dict_learning: 贪婪学习字典矩阵分解问题
- dict_learning_online: 在线贪婪学习字典矩阵分解问题
- sparse_encode: 稀疏编码

## 11. Dummy (伪变量)

sklearn.dummy: 伪变量模块。

- DummyClassifier: 简单规则预测方法

## 12. Ensemble（集成）方法

sklearn.ensemble: 集成算法模块，包括集成分类方法、回归方法和其他方法。

- DummyRegressor: 哑回归方法
- AdaBoostClassifier: AdaBoost 迭代分类方法，其核心思想是迭代训练出不同的分类器（弱分类器），然后把这些弱分类器集合起来，形成一个更强的最终分类器（强分类器）
- AdaBoostRegressor: AdaBoost 回归方法
- BaggingClassifier: Bagging 方法。训练多个基估计器，对多次训练、多个样本返回的多数次预测结果进行投票
- BaggingRegressor: Bagging 回归方法
- ExtraTreesClassifier: 完全随机树方法
- ExtraTreesRegressor: 完全随机树回归方法
- GradientBoostingClassifier: 梯度提升分类方法
- GradientBoostingRegressor: 梯度提升回归方法
- IsolationForest: 孤立森林方法
- RandomForestClassifier: 随机森林方法
- RandomTreesEmbedding: 完全随机树方法
- RandomForestRegressor: 随机森林回归方法
- VotingClassifier: 投票方法，多机器学习方法的集成式方法。

部分基于 Tree 决策的方法如下

- partial_dependence.partial_dependence: 部分依赖方法
- partial_dependence.plot_partial_dependence: 部分依赖关系绘制方法

## 13. feature_selection（特征选择）

sklearn.feature_selection: 特征选择模块，包括各种特征选择方法，如

单变量的选择方法、滤波器特征选择方法、递归特征消除方法等

- GenericUnivariateSelect: 递归单变量特征选择器的基类
- SelectPercentile: 按百分位选择算法
- SelectKBest: 按最佳 K 个特征选择算法
- SelectFpr: 滤波器，在 FPR 测试中选择低于 alpha 的 p 值
- SelectFdr: 滤波器，为控制错误发现率选取 p 值
- SelectFromModel: 元学习特征选择算法，用于选择的似然统计
- SelectFwe: 过滤器，依据多重比较错误率选取 p 值
- RFE: 递归特征消除算法的特征排序
- RFECV: 递归特征消除算法的交叉验证特征排序
- VarianceThreshold: 特征选择方法，剔除所有低方差特征
- chi2: 卡方统计每个类别特征相关
- f_classif: 计算样本的方差分析 F 值
- f_regression: 一元线性回归检验
- mutual_info_classif: 估计离散类别交叉的互信息
- mutual_info_regression: 评估连续类别交叉的互信息

## 14. feature_extraction (特征提取)

sklearn.feature_extraction: 特征提取模块，主要用于从原始数据提取特征数据，包括提取数值、文本、图像等数据的特征提取。

- DictVectorizer: 变换的特征映射为向量
- FeatureHasher: 把特征向量采用 Hash（哈希）函数进行散列

(1) 图像特征提取

sklearn.feature_extraction.image: 从图像中提取特征。

- image.img_to_graph: 像素-像素结构连接图
- image.grid_to_graph: 像素-像素连接的邻图
- image.extract_patches_2d: 把 2D 图像切分为小块
- image.reconstruct_from_patches_2d: 从小块重建图像
- image.PatchExtractor: 从图像中抽取小块

(2) 文本特征提取

sklearn.feature_extraction.text 模块：从文本信息建立特征向量。

- text.CountVectorizer: 将文本集合转换为词频统计向量矩阵
- text.HashingVectorizer: 采用哈希算法，将文本转换为向量矩阵
- text.TfidfTransformer: 把统计矩阵转换为归一化 tf 及 tf-idf 特征
- text.TfidfVectorizer: 把原文档转换为 tf-idf 特征向量

## 15. gaussian_process（高斯过程）

sklearn.gaussian_process 模块实现了基于高斯过程回归和分类算法。

- GaussianProcessRegressor: 高斯过程回归（GPR）算法
- GaussianProcessClassifier: 高斯过程分类（GPC）算法

Kernels 子模块功能：

- kernels.Kernel: 核的基类
- kernels.Sum: k1、k2 两个核求和运算
- kernels.Product: k1、k2 两个核乘积运算
- kernels.Exponentiation: 乘方运算，根据给定的指数运算
- kernels.ConstantKernel: 常数核
- kernels.WhiteKernel: 白色核
- kernels.RBF: 核半径函数，径向基核函数（又名平方指数核）
- kernels.Matern: 马特核
- kernels.RationalQuadratic: 有理二次核
- kernels.ExpSineSquared: 正弦平方核
- kernels.DotProduct: 点积核
- kernels.PairwiseKernel: sklearn.metrics.pairwise 内的核
- kernels.CompoundKernel: 由一组其它核组成的核
- kernels.Hyperparameter: 命名元组形式的超参数核机制

## 16. Isotonic（保序）回归模块

sklearn.isotonic 模块：保序回归方法

- IsotonicRegression: 保序回归模型
- isotonic_regression: 保序回归拟合求解方法
- check_increasing(x,y): y 关于 x 的单调相关确认方法

## 17. kernel_approximation（核近似）模块

sklearn.kernel_approximation 模块收录了核近似方法，基于随机特征映射的核近似方法。

- AdditiveChi2Sampler: 加性卡方核近似特征映射
- Nystroem: 奈斯特姆 Nystroem 内核近似方法，使用训练数据的一个随机子集
- RBFSampler: RBF 内核近似方法，基于蒙特卡洛随机特征变换的 RBF 映射
- SkewedChi2Sampler: 近似特征映射的“斜卡方”核，基于蒙特卡洛随机特征映射方法

## 18. kernel_ridge（核岭回归）模块

sklearn.kernel_ridge 模块：实现核岭回归方法。

- KernelRidge: 内核岭回归方法

## 19. discriminant_analysis（判别分析）

sklearn.discriminant_analysis 模块：判别分析方法。

- LinearDiscriminantAnalysis: 线性判别分析方法
- QuadraticDiscriminantAnalysis: 二次判别分析方法

## 20. linear_model（线性模型）

sklearn.linear_model 模块：通用线性模型方法，也包含回归方法、分类回归方法、贝叶斯方法、弹性方法与最小二乘回归方法等，它还实现了相关随机梯度下降方法。

- ARDRegression: ARD 贝叶斯回归方法
- BayesianRidge: 贝叶斯岭回归方法
- ElasticNet: 使用 L1 和 L2 正则的线性回归方法
- ElasticNetCV: 正则路径迭代拟合的交叉验证弹性网络
- HuberRegressor: 对异常值鲁棒的线性回归方法
- Lars: 最小角回归方法
- LarsCV: 交叉验证最小角回归方法
- Lasso: 采用 L1 正则化的线性方法（又称 Lasso 套索）
- LassoCV: 正则路径迭代拟合的交叉验证 Lasso 线性方法
- LassoLars: 套索与最小角拟合回归方法
- LassoLarsCV: 交叉验证套索与最小角拟合回归方法
- LassoLarsIC: 套索与最小角拟合回归方法，使用 AIC 或 BIC 选择模型
- LinearRegression: 普通最小二乘线性回归方法
- LogisticRegression: 逻辑回归方法（又称 logit_MaxEnt）
- LogisticRegressionCV: 交叉验证逻辑回归方法（又称 logit_MaxEnt）
- MultiTaskLasso: 多任务 Lasso 算法，使用 L1/L2 混合正则
- MultiTaskElasticNet: 多任务弹性网络算法，使用 L1/L2 混合正则
- MultiTaskLassoCV: 交叉验证多任务 Lasso 算法
- MultiTaskElasticNetCV: 交叉验证多任务弹性网络算法
- OrthogonalMatchingPursuit: 正交匹配追踪（OMP）算法
- OrthogonalMatchingPursuitCV: 交叉验证正交匹配追踪（OMP）算法
- PassiveAggressiveClassifier: 被动攻击分类算法
- PassiveAggressiveRegressor: 被动攻击回归算法
- Perceptron: 感知器算法
- RandomizedLasso: 随机套索算法
- RandomizedLogisticRegression: 随机逻辑回归算法
- RANSACRegressor: RANSAC 算法，随机采样一致性算法
- Ridge: 岭回归算法
- RidgeClassifier: 岭回归分类算法
- RidgeClassifierCV: 交叉验证岭回归分类算法
- RidgeCV: 交叉验证岭回归算法
- SGDClassifier: 线性分类算法，使用随机梯度下降算法训练数据
- SGDRegressor: 正则化线性拟合算法，采用最小化经验损失的随机梯度下降算法
- TheilSenRegressor: Theil-Sen 算法，采用稳健多变量回归模型
- lars_path: 最小角回归算法，使用 LARS 路径
- lasso_path: 计算套索路径坐标下降路径
- lasso_stability_path: 用于随机套索稳定性评估的稳定路径算法

## 21. Manifold（流形学习）

sklearn.manifold 模块：流形学习算法、数据嵌入技术等。

- LocallyLinearEmbedding: 局部线性嵌入算法
- Isomap: 等距嵌入算法
- MDS: 多维缩放算法
- SpectralEmbedding: 非线性谱嵌入算法
- TSNE: t 分布随机近邻嵌入算法
- locally_linear_embedding: 执行局部线性嵌入算法分析数据
- spectral_embedding: 基于图拉普拉斯特征向量的投影样本算法
- SMACOF: 多维度缩放算法

## 22. Mixture（高斯混合）模型

sklearn.mixture 模块：高斯混合算法

- GaussianMixture: 高斯混合算法
- BayesianGaussianMixture: 高斯贝叶斯混合算法

## 23. Multiclass（多分类）

- OneVsRestClassifier: 简称为 OvR，一对多多分类/多标签策略算法
- OneVsOneClassifier: 简称为 OvO，一对一多分类策略算法
- OutputCodeClassifier: 输出编码纠错多分类算法

## 24. Multioutput（多输出）回归和分类

sklearn.multioutput 模块：实现多输出回归和分类算法。

- logistic_regression_path: 计算逻辑回归模型，用于正则化参数序列
- orthogonal_mp: 正交匹配追踪（OMP）算法
- orthogonal_mp_gram: Gram 正交匹配追踪（OMP）算法
- MultiOutputRegressor: 多输出回归方法
- MultiOutputClassifier: 多输出分类方法

## 25. naive_bayes（朴素贝叶斯）方法

sklearn.naive_bayes 模块，包含朴素贝叶斯方法，这些是在监督学习方法的基础上，应用贝叶斯定理和强条件独立性假设开发的方法。

- GaussianNB: 高斯朴素贝叶斯方法
- MultinomialNB: 多项式朴素贝叶斯方法
- BernoulliNB: 二元伯努利朴素贝叶斯方法

## 26. Neighbors（近邻）方法

sklearn.neighbors 模块：实现了 K-近邻方法

- NearestNeighbors: 无监督学习邻居搜索方法
- KNeighborsClassifier: K-近邻分类方法
- RadiusNeighborsClassifier: 给定半径内的近邻投票方法
- KNeighborsRegressor: K-近邻回归方法
- RadiusNeighborsRegressor: 给定半径内的近邻回归方法
- NearestCentroid: 最近质心方法
- BallTree: BallTree 快速广义最近邻空间搜索方法
- KDTree: KDTree 快速广义最近邻空间搜索方法
- LSHForest: 基于 LSH 的近邻方法
- DistanceMetric: 距离度量类
- KernelDensity: 核密度估计
- LocalOutlierFactor: LOF 方法，用于识别异常点的无监督异常检测方法
- kneighbors_graph: 估计为点 x 的 k 近邻图
- radius_neighbors_graph: 计算点半径为 X 的近邻图

## 27. neural_network（神经网络）

sklearn.neural_network 模块：包括神经网络模型的方法。

- BernoulliRBM: RBM 方法，伯努利受限玻尔兹曼机方法
- MLPClassifier: 多层感知器分类方法
- MLPRegressor: 多层感知回归方法

## 28. Calibration（校准概率）

sklearn.calibration 模块：包括预测概率的校准方法。

- CalibratedClassifierCV: 概率校准分类方法
- calibration_curve: 计算真实预测概率的校准曲线

## 29. cross_decomposition（交叉分解）方法

sklearn.cross_decomposition 模块：交叉分解方法。

- PLSRegression: PLS 偏最小二乘回归方法
- PLSCanonical: PLS-Canonical 方法
- CCA: 典型相关分析方法
- PLSSVD: 偏最小二乘奇异值分解方法

## 30. Pipeline（管道）

sklearn.pipeline 模块：用于记录 sklearn 机器学习方法的各个流程。

- Pipeline: 基础转换流程
- FeatureUnion: 串接多个转换器
- make_pipeline: 按给定转换器构建管道
- make_union: 按给定转换器构建联合转换
