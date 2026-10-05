# 6.3.2 数据准备

# 1. 安装 RQData

输入下面命令可在 Windows 系统上安装 RQData。

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --extra-index-url https://rquser:ricequant99@py.r icequant.com/simple/ rqdatac==1.0.0a29
```

# 2. 下载数据

在 “examples\DataService\RqdataDataService” 文件夹内的 config.json 上填写 RQData 的账号和密码，保存退出后启动 downloadData.py 下载期货全品种的日线级别数据。启动操作是用 “Shift 键+鼠标右键” 选择 “在此处打开命令窗口”，然后输入下面命令即可。

```
python downloadData.py
```

本次投资组合回测时间区间是 2014 年 1 月 1 日以后的数据，故在该回测上会剔除比较新的品种：

```
''''
#立即下载数据到数据库中，用于手动执行更新操作
''''
from dataService import *
if __name__ == '__main__':

    #中金所
    downloadDailyBarBySymbol('IF99')
    downloadDailyBarBySymbol('IC99')        #中证 500 从 2015 年开始
    downloadDailyBarBySymbol('IH99')        #上证 50 从 2015 年开始

    #上期所
    downloadDailyBarBySymbol('CU99')
    downloadDailyBarBySymbol('AL99')
    downloadDailyBarBySymbol('ZN99')
    downloadDailyBarBySymbol('PB99')
    downloadDailyBarBySymbol('NI99')        #镍从 2015 年开始
    downloadDailyBarBySymbol('AU99')
```