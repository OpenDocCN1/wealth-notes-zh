# # 第 4 章
## 人工智能与趋势预测

# ## 4.1  TFLearn简化接口

TFLearn 是 TensorFlow 采用 SKLearn 风格的简化接口，其函数 API 接口与 SKLearn 基本兼容，类似于 SKLearn 针对 TensorFlow 的扩展模块。

从源码结构来说，TFLearn 比 TensorLayer 复杂很多，它的最大优点如下。

+   - 其函数接口与经典机器学习 SKLearn 类似，学习成本很低。

+   - 谷歌 TensorFlow 内置了 TFLearn 模块，从 TensorFlow 1.1 版本起，提高了 TFLearn 权重，从 contrib 第三方开发者模块，提升到内部的 estimator 模型评测级别，大大提高了对 TFLearn 的支持。

+   - TFLearn 内置了大量完整的神经网络模型，可以直接调用，大大简化了开发流程。

此外，TFLearn 还具有以下特色。

+   - 容易使用和易于理解的高层次 API，用于实现深度神经网络，附带教程和例子。

+   - 通过高度模块化的内置神经网络层、正则化器、优化器等快速原型设计。

+   - 对 TensorFlow 完全透明，所有函数都是基于 TensorFlow 的，可以独立于 TFLearn 使用。

+   - 通过强大的辅助函数，训练任意 TensorFlow Graph，支持多输入、多输出和优化器。

+   - 简单而美观的图形可视化，支持权值、梯度、特征图等细节参数图形。