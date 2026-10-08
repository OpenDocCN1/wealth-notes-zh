以下是矩阵求逆：

```
```
import numpy_linalg as nlg
a = np.random.rand(2,2)
a = np.mat(a)
print "a:"
print a
ia = nlg.inv(a)
print "inverse of a:"
print ia
print "a * inv(a)"
print a * ia
a:
[[ 0.86211266  0.6885563 ]
 [ 0.28798536  0.70810425]]
inverse of a:
[[ - 1.71798445 - 1.6705577 ]
 [- 0.69870271   2.09163573]]
a * inv(a)
[[1.  0.]
 [0.  1.]]
```
```

以下是求特征值和特征向量：

```
```
a = np.random.rand(3,3)
eig_value, eig_vector = nlg.eig(a)
print "eigen value:"
print eig_value
print "eigen vector:"
print eig_vector
eigen value:
[ 1.35760609   0.43205379 - 0.53470662]
eigen vector:
[[ - 0.76595379   0.88231952  - 0.07390831]
 [ - 0.55170557   0.21659887  - 0.74213622]
 [ - 0.33005418   0.41784829    0.66616169]]
```
```

以下是按列拼接两个向量构成一个矩阵：

```
```
a = np.array((1,2,3))
b = np.array((2,3,4))
print np.column_stack((a,b))
[1 2]
[2 3]
[3 4]
```
```

在循环处理某些数据得到结果后，将结果拼接成一个矩阵是十分有用的，可以通过 vstack 和 hstack 完成：

```
```
a = np.random.rand(2,2)
b = np.random.rand(2,2)
print "a:"
```
```