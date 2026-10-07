E[ (D − p(Xi))Yi / (p(Xi)(1 − p(Xi))) | Xi ]
= E[ Yi / p(Xi) | Xi, Di = 1 ] p(Xi)
+ E[ −Yi / (1 − p(Xi)) | Xi, Di = 0 ] (1 − p(Xi))
= E[Yi | Xi, Di = 1] − E[Yi | Xi, Di = 0]
= E[Y1i − Y0i | Xi]  (6.45)

从而，总体平均因果效应为，

τATE = E[Y1i − Y0i] = E[ E[Y1i − Y0i | X] ]
= E[ (D − p(Xi))Yi / (p(Xi)(1 − p(Xi))) ]

类似地，干预组平均因果效应的逆概加权测度为：

τATT = E[Y1i − Y0i | Di = 1] = E[ E[Y1i − Y0i | Xi, Di = 1] | Di = 1 ]
= E[ E[Y1i − Y0i | Xi] | Di = 1 ]
= E[ E[ (D − p(Xi))Yi / (p(Xi)(1 − p(Xi))) | Xi ] | Di = 1 ]
= Σx E[ (D − p(x))Yi / (p(x)(1 − p(x))) | Xi = x ] Pr[Xi = x | Di = 1]
= Σx E[ (D − p(x))Yi / (p(x)(1 − p(x))) | Xi = x ] p(x)Pr[Xi = x] / Pr[Di = 1]
= 1/Pr[Di = 1] Σx E[ (D − p(x))Yi / (1 − p(x)) | Xi = x ] Pr[Xi = x]
= 1/Pr[Di = 1] E[ (D − p(Xi))Yi / (1 − p(Xi)) ]  (6.46)

（6.44）和（6.46）说明利用逆概加权方法也需要两个基本的步骤，首先估计倾向指数，然后利用倾向指数对两组个体进行加权，相应的估计量如下：

τATE^ipw = 1/N Σi=1^N Yi (Di − p̂(Xi)) / [ p̂(Xi)(1 − p̂(Xi)) ]  (6.47)

τATT^ipw = 1/Nt Σi=1^N Yi (Di − p̂(Xi)) / [1 − p̂(Xi)]  (6.48)

逆概加权方法和匹配方法需要相同的识别条件：CIA 和共同区间假设。逆概加权估计量（6.47）和（6.48）显示，逆概加权估计量依赖于倾向指数，如果倾向指数模型存在模型误设，则会产生较大估计偏差，尤其在极端的倾向指数附近，估计量将变得不稳健，比如倾向指数接近于 1 或 0，τATE^ipw 将趋于无穷大；同样