```
x_ = tf.placeholder(tf.float32, [None, 1])
y_ = tf.placeholder(tf.float32, [None, 1])  # y_为测试集结果数据

weight = tf.Variable(tf.ones([1, 1]))
bias = tf.Variable(tf.ones([1]))

y = tf.matmul(x_, weight) + bias

loss = tf.reduce_mean(tf.square(y - y_)) #批量线性回归用这个损失函数
train_step = tf.train.AdamOptimizer(0.0001).minimize(loss)

init = tf.global_variables_initializer()

flag = True
with tf.Session() as sess:
    sess.run(init)

    count = 0
    loss_temp = 0
    while flag:
        feed = {x_: real_A, y_: real_B}
        sess.run(train_step, feed_dict=feed)
        loss_res = sess.run(loss, feed_dict=feed)
        if count%10000 == 0:
            print("正在运行次数为:",count,"  Loss 为: ",loss_res)
        if loss_temp == loss_res:
            flag = False
        count += 1
        loss_temp = loss_res

weight = sess.run(weight)
bias = sess.run(bias)

return weight[0][0],bias[0]
```

这里不做解释，有兴趣的读者可以自行学习。代码的主要作用是使用传递进来的两个参数做出形如

Y=aX+b

的数据格式。这里默认 real_A=X, read_B=Y。返回值 weight[0][0]和 bias[0]分别代表其对应的系数以及截距。