# 31. Preprocessing（预处理和归一化）

sklearn.preprocessing 模块：缩放、中心化、正则化、归一化及插补方法。

+   - Binarizer: 按阈值对数据二值化（特征设为 0 或 1）
- FunctionTransformer: 从任意函数中构建转换器
- Imputer: 完成缺失数据插补
- KernelCenterer: 中心化核矩阵
- LabelBinarizer: 在一对多余方式中二元化标签
- LabelEncoder: 用 0 到 n_class-1 编号标签
- MultiLabelBinarizer: 迭代器, 多标签格式之间的转换器
- MaxAbsScaler: 对每个特征按其绝对值缩放
- MinMaxScaler: 对每个特征按给定范围缩放
- Normalizer: 分别归一化为单位范数
- OneHotEncoder: 使用热编码的离散的分类特征
- PolynomialFeatures: 生成多项式和交互特征
- RobustScaler: 使用统计缩放特征, 对异常值稳健
- StandardScaler: 标准缩放特征, 通过消除均值来缩放至单位方差
- add_dummy_feature: 通过添加伪特征创建截距项
- binarize: 阈值二值化的数组或 SciPy 稀疏矩阵
- label_binarize: 在多标签方式中二元化标签
- maxabs_scale: 不破坏稀疏性的情况下将每个特征缩放至[-1,1]
- minmax_scale: 对每个特征按给定范围缩放
- normalize: 缩放输入向量至单位范数
- robust_scale: 沿任意轴标准化数据