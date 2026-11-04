# Mac / Linux 在终端里运行：

```
curl -fsSL https://claude.ai/install.sh | bash
```

# Windows 在 PowerShell 里运行：

```
irm https://claude.ai/install.ps1 | iex
```

装完新开一个终端窗口，输入下面这行，打印出版本号就是装好了：

```
claude --version
```

再输入 claude，按浏览器里的提示登录。Claude Code 需要 Pro、Max、Team、Enterprise 或 Console 账号，免费版不包含。不想用命令行，也可以下载 Claude 桌面 App。

（图 8-1）编程助手官方安装页，复制对应系统的那一行命令

![](bbox=[0.06, 0.53, 0.94, 0.73])

# 步骤 2：在电脑上装 Node.js（选 LTS 版）。

Node.js 是让电脑能运行 JavaScript 工具的基础软件，Expo 要靠它跑起来。装 Node.js 时会顺带装上 npm，它是一个帮你下载各种工具包的“应用商店”。

+   101. 打开 Node.js 下载页。