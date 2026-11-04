```
cd app-practice
pwd
```

最后一行 pwd 会显示你现在所在的位置：
Mac 一般是 /Users/你的用户名
/Documents/app-practice， Windows 一般是 C:\Users\你的用户名\Documents\app-practice。前半段和你的不完全一样没关系，末尾是 app-practice 就对了。打开 Mac 访达里的「文稿」或 Windows 的「文档」，也能看到这个新文件夹。不会操作，就把这几行发给编程助手，让它针对你的系统一步步带你，并回读当前路径。

# 步骤6：创建工程，启动开发服务。

依次运行下面三行：

```
npx create-expo-app@latest followup-notebook
cd followup-notebook
npx expo start
```

在国内，下载很慢或报 ETIMEDOUT（连接超时）很常见，不是你装错了。镜像源就是国内的下载加速站：同一批工具包，换从国内的服务器下载。它和第 13 章后端说的"镜像"（打包好的服务器程序）是两回事，只是名字像。把完整报错发给编程助手，再加一句：

> 请按官方方式把 npm 的下载地址换成国内镜像源，告诉我改了什么、以后怎么改回去。

改好以后，重跑出错的那一行。

+   • 第一行会创建一个叫 followup-notebook 的新工程。中途如果让你选模板或 SDK 版本，选和手机上 Expo Go 对得上的那一版，并记下你实际选了什么。