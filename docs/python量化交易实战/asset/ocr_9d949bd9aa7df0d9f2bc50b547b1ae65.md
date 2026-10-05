# 2.2.4 threading 中的 join 类

join 类是 threading 中用于堵塞当前主线程的类，其作用是阻止全部的线程继续运行，直到被调用的线程执行完毕或者超时。具体代码如【程序 2-3】所示。

# 【程序 2-3】

```
import threading, time
def doWaiting():
    print('start waiting:', time.strftime('%S'))
    time.sleep(3)
    print('stop waiting', time.strftime('%S'))
thread1 = threading.Thread(target = doWaiting)
thread1.start()
time.sleep(1)                #确保线程 thread1 已经启动
print('start join')
thread1.join()               #将一直堵塞，直到 thread1 运行结束
print('end join')
```

程序的运行结果如图 2-18 所示。

![](bbox=[0.368, 0.41, 0.59, 0.5])

图 2-18 程序运行结果

其中的 time 方法设定了当前的时间，当 join 启动后，堵塞了调用整体进程的主进程，而只有当被堵塞的进程执行完毕后，后续的进程才继续执行。

除此之外，对于线程的使用，Python 还有很多其他的方法，例如 threading.Event、threading.Condition 等，这些都是在程序设计时能够极大地帮助程序设计人员编写合适程序的工具。限于篇幅，这里不再一一进行介绍，在后续的使用过程中，作者会带领读者了解和掌握更多的相关内容。

# 2.3 小结

本章介绍了 Python 的基本安装和编译器的使用。在这里推荐读者使用 PyCharm 免费版作为 Python 编辑器，这有助于更好地安排工程文件的配置和程序的编写。

同时，本章中还介绍了最常用的一些类库，这里只是对线程类做了一个详细的介绍，线程类是 Python 最为重要的一个类库，在后面的代码编写中会频繁遇到。

本章是 Python 最基础的内容，后面的章节将以 Python 的使用为主，并且还会介绍更多的 Python 类库，希望读者能够掌握相关内容。