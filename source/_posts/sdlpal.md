---
title: 在 iOS 上安装 仙剑奇侠传
date: 2017-11-17
tags:
- SDLPAL
- 仙剑奇侠传
- iOS
---

**该指南适合有一定编程基础的小伙伴**

## 设备

- iOS device
- Mac with XCode


## 步骤

1. 把[代码](https://github.com/sdlpal/sdlpal.git)拉到本地
2. 在根目录下安装子依赖 `git submodule update --init --recursive`
3. 切换目录到 `ios/SDLPal/` 下
4. 如果未安装 `cocoapods` 则安装 `sudo gem install cocoapods`
5. 通过 `CocoaPods` 安装依赖 `pod install`
6. 打开工程 `open SDLPal.xcworkspace/`
7. 如图选中 `SDLPal` - `TARGETS` - `SDLPal` - `General`, 把 `1` 改为类似的包名，在 `2` 中把你的 `AppleID` 添加为帐号并选中

	![图1](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/1.png)
8. 手机连上 mac, 如图选中自己的手机，安装，中间提示输入密码

	![图2](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/2.png)
	![图3](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/3.png)
9. 把资源文件拽进应用里，搞定，至于资源文件...自己找吧，或者留邮箱找我要

	![图4](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/4.png)
	![图5](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/5.png)

10. 完美运行

	![图6](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/6.png)
	![图7](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/sdlpal/7.png)


## One more thing

突然有种想学习 iOS 开发的想法....