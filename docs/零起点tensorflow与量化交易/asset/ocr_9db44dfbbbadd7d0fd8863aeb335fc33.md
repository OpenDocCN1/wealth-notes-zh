# 零起点 TensorFlow 与量化交易

+   * SyncReplicasOptimizer 已被删除, SyncReplicasOptimizerV2 被重命名为 SyncReplicas Optimizer.

+   * tf.zeros_initializer() 和 tf.ones_initializer()返回一个必须用 initializer 参数调用的可调用值,在代码中用 tf.zeros_initializer()替换 tf.zeros_initializer。

+   * SparseTensor.shape 已被重命名为 SparseTensor.dense shape 。与 SparseTensorValue.shape 相同。

+   * 分别替换 tf.sclar_summary、tf.histogram_summary、tf.audio_summary、tf.image_summary 为 tf.summary.sclar、tf.summary.histogram、tf.summary.audio、tf.summary.image。新的摘要 ops 以名字（而不是标签）作为它们的第一个参数,这意味着现在摘要 ops 遵从 TensorFlow 名称范围。

+   * 使用 tf.summary.FileWriter 和 tf.summary.FileWriterCache 替 tf.train.SummaryWriter 和 tf.train. SummaryWriterCache。

+   * 从公共 API 中删除 RegisterShape。使用 C++形状函数注册。

+   * Python API 中的_ref dtypes 已被弃用。

+   * 在 C++ API（in tensorflow/cc）中, Input、Output 等已经从 tensorflow::ops 命名空间移动到 Tensorflow。

+   * 将{softmax, sparse_softmax, sigmoid}_cross_entropy_with_logits 的 arg order 更改为(labels, predictions), 并强制使用已命名的 args。

# 3. 变量函数

tf.VARIABLES → tf.GLOBAL_VARIABLES
tf.all_variables → tf.global_variables
tf.initialize_all_variables → tf.global_variables_initializer
tf.initialize_local_variables → tf.local_variables_initializer
tf.initialize_variables → tf.variables_initializer

# 4. 函数更新

tf.audio_summary → tf.summary.audio
tf.contrib.deprecated.histogram_summary → tf.summary.histogram
tf.contrib.deprecated.sclar_summary → tf.summary.sclar
tf.histogram_summary → tf.summary.histogram