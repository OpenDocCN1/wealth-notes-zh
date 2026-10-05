（1）启动 MongoDB 数据库。

在同一个命令提示窗口，输入下面命令：

```
"C:\Program Files\MongoDB\Server\4.0\bin\mongod.exe" --dbpath="c:\data\db"
```

（2）连接到 MongoDB。

在 C 盘根目录下新开命令提示窗口，输入下面命令，用于连接 MongoDB，成功连接后如图 3-6 所示。

```
"C:\Program Files\MongoDB\Server\4.0\bin\mongo.exe"
```

![](bbox=[0.22, 0.388, 0.739, 0.746])

图 3-6 成功连接到 MongoDB

MongoDB 4.0 版本是默认开机启动的，不需要像以前的版本需要手动将 MongoDB 注册为 Windows 系统服务。重启系统后打开 Windows 任务管理器，可以发现已经成功在后台上启动，如图 3-7 所示。