库或者模组没有内置于 Python 环境中，则需要先下载安装该函数库或者模组，然后才能被使用。一般通过 pip 指令来安装包，安装指令为：

```
```
pip install name
```
```

比如要安装 NumPy，打开电脑控制台，输入指令：

```
```
pip install numpy
```
```

即可安装 NumPy 这一函数库。

然而，某些函数库的使用可能依赖于其他函数库的成功安装，不同的库和模组之间存在依赖关系，使得有些包不一定能够通过简单的 pip 指令能够实现，可能还需要其他安装细节与技巧。这些安装技巧对于 Python 初学者来说，则可能略显复杂和麻烦。

# 1.2.2 下载安装 Anaconda

如果专注于科学计算功能，读者可以直接安装 Anaconda。Anaconda 是 Python 的科学计算环境，内置 Python 安装程序，其主要功能如下。

+   - 安装简单：下载 Anaconda 的.exe 执行文件，双击执行文件，按照界面提示即可进行安装。

+   - 配置众多科学计算包：Anaconda 集合了 400 个以上的科学计算与数据分析功能的包 (Packages)，如 NumPy、Pandas、SciPy、Matplotlib 和 IPython 等。安装 Anaconda 时，这些包可也会被成功安装。

+   - 支持多种操作系统：Anaconda 支持 Windows、Linux 和 Mac OX 等平台。

+   - 兼容 Python 多种版本：2.X 和 3.X 版本的 Python 在 Anaconda 环境中可以同时并存，在 Anaconda 中通过适当指令可以自由切换使用的 Python 版本。

Anaconda 相关版本种类较多，有 Anaconda、Anaconda Pro、Anaconda Workgroup 以及 Anaconda Enterprise 等版本，部分版本的 Anaconda 需要付费使用。如果对功能要求不高，读者可以下载 Anaconda 的免费版，进入 Anaconda 的下载页面，如图 1.3 所示。

以 Windows 系统为例，Anaconda 有两种版本选择，一种内置 Python 版本为 Python 2.7，另一种内置有 Python 3.5 的安装版本。此外，Anaconda 版本还有 32 位系统与 64 位系统之分，读者可以根据电脑配置与功能需求选择相应版本下载执行文档，通过双击执行文档并按照提示即可进行安装。

# Conda 在 Anaconda 中的应用

Conda 是众多开源包的管理系统与环境管理系统，在 Anaconda 环境中，通过 Conda 相关指令可以进行查看已安装包、更新包、安装其他包等操作。通过 pip 环境进行安装的包也可以由 Conda 来管理。