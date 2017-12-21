---
title: iOS webview 离线包 file 协议
date: 2017-12-21
tags:
- iOS
- webview
- file protocol
- http protocol
- https protocol
---

在 Hybrid 开发中，iOS 将前端离线包下载到本地，加载资源的协议为 `file://`。

在 CSS 文件中，`background-image: url(//omhr7p9e5.bkt.clouddn.com/blog/monk.gif)` 时，系统将加载 `file://omhr7p9e5.bkt.clouddn.com/blog/monk.gif`，因此图片将会`404`。

此时需要将图片的协议写死，`http` 或 `https`。

安卓下不存在这个问题。

注：在 native 端应该有办法拦截处理这样的问题。
