掩技术。

# 2. API 接口变动

+   • TensorFlow/Models 已经被移动到一个单独的 github 库。

+   • 除法和模运算符（/、//、%）匹配 Python（flooring）语义。这也适用于 tf.div 和 tf.mod。要获取基于强制整数截断的行为，可以使用 tf.truncatediv 和 tf.truncatemod。

+   • 推荐使用 tf.divide()作为除法函数。tf.div()将保留，但它的语义不会回应 Python 3 或 from future 机制。

+   • tf.reverse()取轴的索引要反转。例如 f.reverse(a,[True, False, True]) 必须写为 tf.reverse(a, [0,2])。tf.reverse_v2()将保持到 TensorFlow 1.0 最终版。

+   • tf.mul、tf.sub 和 tf.neg 不再使用，改为使用 tf.multiply、tf.substract 和 tf.negative。

+   • tf.pack 和 tf.unpack 被弃用，改为使用 tf.stack 和 tf.unstack。

+   • TensorArray.pack 和 TensorArray.unpack 正在被弃用过程中，将来计划启用 TensorArray.stack 和 TensorArray.unstack。

+   • tf.listdir 已被重命名为 tf.setdiff1d，以匹配 NumPy 命名。

+   • tf.inv 已被重命名为 tf.recipproc（组件的倒数），以避免与 np.inv 混淆，后者是矩阵求逆。

+   • tf.round 使用 banker 的舍入（round to even）语义来匹配 NumPy。

+   • tf.split 以相反的顺序并使用不同的关键字接收参数。现在将 NumPy order 匹配为 tf.split(value, num_or_size_splits, axis)。

+   • tf.sparse_split 采用相反顺序的参数，并使用不同的关键字。现在将 NumPy order 匹配为 tf.sparse_split(sp_input, num_split, axis)。注意：暂时要求 tf.sparse_split 需要关键字参数。

+   • tf.concat 以相反的顺序并使用不同的关键字接收参数。注意：现在将 NumPy order 匹配为 tf.concat(values, axis, name)。

+   • 在默认情况下，tf.image.decode_jpeg 使用更快的 DCT 方法，牺牲一点保真度来提高速度。通过指定属性 dct_method = INTEGER_ACCURATE，可以恢复到旧版本行为。

+   • tf.complex_abs 已从 Python 界面中删除。tf.abs 支持复杂量，现在使用 tf.abs。

+   • Template.var_scope 属性被重命名为 variable_scope。

373