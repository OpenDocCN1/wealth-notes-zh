```
x = tf.placeholder(tf.float32, [None, 1])
y_ = tf.placeholder(tf.float32, [None, 1])  # y_为测试集结果数据

weight = tf.Variable(tf.zeros([1, 1]))
bias = tf.Variable(tf.zeros([1]))

#y = tf.matmul(x, weight) + bias  #不用矩阵乘法
y = tf.multiply(weight,x) + bias    #简单的线性乘法

loss = tf.reduce_mean(tf.square(y - y_)) #批量线性回归用这个损失函数
train_step = tf.train.AdamOptimizer(0.00001).minimize(loss)

init = tf.global_variables_initializer()

flag = True

with tf.Session() as sess:
    sess.run(init)
    
    count = 0
    loss_temp = 0
    loss_flag = 0
    while flag:
        feed = {x: xs, y_: ys}
        sess.run(train_step, feed_dict=feed)
        loss_res = sess.run(loss, feed_dict=feed)
        
        if count%10000 == 0:
            print("正在运行次数为:",count,"  Loss 为: ",loss_res)
        
        if loss_temp == loss_res:
            loss_flag += 1
            if loss_flag > 10:
                flag = False
        count += 1
        loss_temp = loss_res

weight = sess.run(weight)
bias = sess.run(bias)

return weight[0][0],bias[0]
```

这里使用 TensorFlow 进行一元线性回归，而使用 TensorFlow 进行多元线性回归的代码