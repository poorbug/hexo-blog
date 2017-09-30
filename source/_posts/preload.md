---
title: CSS preload 初试
date: 2017-09-30
tags:
- link
- preload
- CSS 优化
- 页面加载实践优化
---

## 背景
当你访问一个页面时，浏览器对资源文件的请求顺序大概是以下这样：

1. 请求 HTML
2. 请求 `<head>` 中的 JS 与 CSS 文件
3. 请求 `<head>` 中 JS 与 CSS 中的图片、字体等文件

此时，问题出现了：

1. JS 与 CSS 文件需要等 HTML 文件加载完后才能被获知并请求；
2. 图片、字体等文件需要等 JS 与 CSS 加载完后才能被获知并请求；

**上面 3 步中的加载时间起点分别依赖上一步中的时间终点。**

![请求依赖](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/load-sequence.png)

可以让资源文件不依赖 JS 与 CSS 提前请求到，从而减少页面的加载时间嘛？

## 原理
[通过rel="preload"进行内容预加载 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Preloading_content)

因为开发者是知道需要哪些资源文件的，所以在 HTML 中请求 JS 与 CSS 文件的时候利用浏览器可以并发请求的特点顺便把资源文件请求了。
`preload` 即是把相应的资源文件写在 HTML 的 `<head>` 中。

## 实践
以我的博客为例：

优化前：

``` HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <link rel="stylesheet" href="/css/style.css">
</head>
<body></body>
</html>
```

优化后：

``` HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <link rel="stylesheet" href="/css/style.css">
  
  <!-- 加入以下三行代码 -->
  <link rel="preload" href="https://omhr7p9e5.bkt.clouddn.com/blog/Orbitron-Black.ttf" as="font" type="font/ttf" crossorigin="anonymous">
  <link rel="preload" href="https://omhr7p9e5.bkt.clouddn.com/blog/bg.png" as="image">
  <link rel="preload" href="https://omhr7p9e5.bkt.clouddn.com/hexo-blog/fontawesome-webfont.woff?v=" as="font" type="font/woff" crossorigin="anonymous">
  
</head>
<body></body>
</html>
```
如此，则浏览器在加载 `style.css` 的同时也加载了其后的三个资源文件，后续解析 CSS 时则不需在请求这三个资源文件。

## 结果
### 优化前：

![优化前请求瀑布流](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/preload-1.png)

三个资源文件需要在 `style.css` 文件被加载后能被加载，试了几次时间大多在 **300ms** 以上。

### 优化后：

![优化后请求瀑布流](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/preload-2.png)

三个资源的请求时间起点与 `style.css` 是并列的，时间在 **300ms以内**, 大概保持在 **250ms - 270ms** 左右。

因为资源文件较小，总体的请求时间也较短，所以并看不出很大效果。

但是可以预测，如果在 CSS 文件与资源文件较大的情况，还是可以优化比较多时间的。

举个例子：

1. CSS 文件为 **2M**，图片大小为 **2M**
2. 请求时间分别为 **5S**
3. 如果没有 `preload`，则总时间至少为 **10S**
4. 如果图片 `preload`, 则 CSS 文件与图片并行请求，理论上事情时间可压缩到 **5S**

此时效果就很显著了（但是只是理论，目前并未有实验支持，）。

在 [CSSconf EU 2017 | Patrick Hamann: CSS and the first meaningful paint - Youtube](https://www.youtube.com/watch?v=4pQ2byAoIX0&index=3&list=PL37ZVnwpeshF0XmpjKBJ3-0kvr3b5ZpJR) 的实验中，速度提升了 **64%**。

## 弊端

1. 代码不好维护；如果文件地址更改，则需要维护两个地方。
2. 由于文件提前加载会占用一定的系统计算资源，因此其他资源的加载时间会稍微慢一点点。
3. 兼容性不容乐观，如下图所示

![preload 兼容性](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/preload/caniuse.png)

## 注意事项
1. 不宜将所有资源文件都用 `preload` 加载，页面的加载速度反而会变慢，`preload` 也将失去其初衷。