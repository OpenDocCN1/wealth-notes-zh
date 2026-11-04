+   1. 手机的数据连接是否可用
+   2. 手机是否解锁并信任这台电脑
+   3. WDA 的开发团队和签名是否匹配
+   4. WDA 能否在目标手机真正启动
+   5. Appium 能否建立 session，并取得截图和页面源
+   6. 目标按键能否定位，点击后页面是否发生预期变化

同一时间只有一个操控者，不要让另一套驱动或保活同时抢手机。

涉及信任、系统确认或需要真人操作时，明确停在哪一步。

把每一关的结果、错误和证据位置留下来。

让 AI 先说明安装会改变什么，再按当前官方文档准备。已有环境先检查，免得重复安装，或升级到互不兼容的组合。下面几条是在 Mac 终端查版本、设备和驱动；

```
xcodebuild -version
xcrun devicectl list devices
appium --version
appium driver list --installed
```

缺什么再装。核对于 2026 年 10 月，官方安装命令如下（Appium 用 npm 装，需要先有 Node.js）：

```
npm install -g appium
appium driver install xcuitest
```

WDA 用哪个开发团队签名、装到哪台手机，要按你的实际环境定。

Appium 安装 · XCUITest 驱动官方指南 · Apple 命令行工具

装好后，在一个终端启动 Appium，让它的日志一直开着；

```
appium
```