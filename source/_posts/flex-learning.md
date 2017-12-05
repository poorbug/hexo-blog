---
title: Flex 布局学习
date: 2017-12-01
tags:
- Flex layout
- 弹性布局
---

#### 参考：

- [ Flex 布局教程：语法篇 - 阮一峰的网络日志 ](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

Flex 布局，顾名思义，就是弹性布局，它的作用是为页面提供响应式布局。目前已经被大部分浏览器所支持，远古浏览器请自觉绕道...

Flex 布局所能实现的效果其实用其他方法也能实现，但是，很多时候需要涉及 `float`、 `display`、 `position`，可能还会有 `text-align`、 `vertical-align`、 `line-height` 等等诸多属性同时配合使用才能实现，更别提利用 `JS` 来操作 `CSS` 属性了。但是使用 Flex 布局，仅仅需要利用 Flex 相关的几个属性，就能实现我们想要的 **居中**、**填满**、**均分** 等效果，明显的好处是 **简洁**、**优雅**、**不挖坑**。

<iframe src='https://caniuse.com/flexbox/embed/agents=desktop,ios_saf,android' style="width:100%;height: 300px;border:none"></iframe>

## 基本概念

> 采用 Flex 布局的元素，称为 Flex 容器（flex container）。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item）。

容器默认存在两个带方向的轴：主轴（main axis）和垂直于主轴的交叉轴（cross axis）。同时两个轴各有起点与终点。主轴与交叉轴是相对的，通过 `flex-direction` 来决定水平或者垂直的轴是主轴，同时也决定起点终点。

container 的 `flex-direction` 默认为 `row`，item 默认沿主轴排列。
![flex-direction](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/flex/flexDirection.jpg)

Flex 布局一个很重要的概念是 **剩余空间** 的分配。所谓 **剩余空间**，指的是 container 的主轴尺寸在减去 item 的 **既定尺寸** 后剩余的空间。**既定尺寸** 由元素的内容、width、flex-basis 决定。

``` html
<div style="font-size: 12px">
  <style>
    .free-space-container {
      display: flex;
      width: 500px;
      background-color: #00E676;
    }
    .free-space-container > div {
      border: 1px solid #ccc;
      text-align: center;
      background-color: #FF5722;
    }
  </style>
  <div class="free-space-container">
    <div style="width: 100px;">width: 100px;</div>
    <div style="flex-basis: 50%;">flex-basis: 50%;</div>
    <div>auto</div>
    <div style="flex-grow:1;background: #42A5F5;">free space</div>
  </div>
</div>
```

{% raw %}
<div style="font-size: 12px" class="html-code">
  <style>
    .free-space-container {
      display: flex;
      width: 500px;
      background-color: #00E676;
    }
    .free-space-container > div {
      border: 1px solid #ccc;
      text-align: center;
      background-color: #FF5722;
    }
  </style>
  <div class="free-space-container">
    <div style="width: 100px;">width: 100px;</div>
    <div style="flex-basis: 50%;">flex-basis: 50%;</div>
    <div>auto</div>
    <div style="flex-grow:1;background: #42A5F5;">free space</div>
  </div>
</div>
{% endraw %}

## 属性

### container 中的属性

(1). **flex-direction**: item 的排布方向

值 | 含义
--- | -------
row | 水平方向为主轴，方向从左到右
row-reverse | 水平为主轴，从右到左
column | 竖直方向为主轴，方向从上到下
column-reverse | 垂直为主轴，方向从下到上

(2). **flex-wrap**: item 元素如何换行

值 | 含义
--- | -------
nowrap | 不换行
wrap | 超出的放下一行
wrap-reverse | wrap 的结果行倒序

![flex-wrap: nowrap](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/flex/nowrap.gif)
![flex-wrap: nowrap](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/flex/wrap.gif)
![flex-wrap: nowrap](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/flex/wrap-reverse.gif)

(3). **justify-content**: item 在主轴上的对齐方式

值 | 含义
--- | -----
flex-start | 向主轴起点对齐
flex-end | 向主轴终点对齐
center | 居中
space-between | 剩余空间均分于 item 之间
space-around | 剩余空间均分于两边

{% raw %}
<div style="font-size: 12px;" class="html-code">
    <style>
    .justify-content-container {
        display: flex;
        width: 200px;
        background-color: #76FF03;
        margin-bottom: 10px;
    }
    .justify-content-container * {
        border: 1px dashed #FF5722;
        box-sizing: border-box;
    }
    .justify-content-item {
        width: 50px;
        text-align: center;
    }
    </style>
    justify-content: flex-start
    <div class="justify-content-container" style="justify-content: flex-start;">
    <div class="justify-content-item">1</div>
    <div class="justify-content-item">2</div>
    <div class="justify-content-item">3</div>
    </div>
    justify-content: flex-end
    <div class="justify-content-container" style="justify-content: flex-end;">
    <div class="justify-content-item">1</div>
    <div class="justify-content-item">2</div>
    <div class="justify-content-item">3</div>
    </div>
    justify-content: center
    <div class="justify-content-container" style="justify-content: center;">
    <div class="justify-content-item">1</div>
    <div class="justify-content-item">2</div>
    <div class="justify-content-item">3</div>
    </div>
    justify-content: space-between
    <div class="justify-content-container" style="justify-content: space-between;">
    <div class="justify-content-item">1</div>
    <div class="justify-content-item">2</div>
    <div class="justify-content-item">3</div>
    </div>
    justify-content: space-around
    <div class="justify-content-container" style="justify-content: space-around;">
    <div class="justify-content-item">1</div>
    <div class="justify-content-item">2</div>
    <div class="justify-content-item">3</div>
    </div>
</div>
{% endraw %}

(4). **align-items**: item 在交叉轴上的对齐方式

值 | 含义
--- | -----
stretch | item 高度为容器高度（填满）
center | 居中
flex-start | 向交叉轴起点对齐
flex-end | 向交叉轴终点对齐
baseline | 向基线对齐


{% raw %}
<div style="font-size: 12px" class="html-code">
    <style>
    .align-items-container {
        display: flex;
        width: 150px;
        height: 30px;
        background-color: #76FF03;
        margin-bottom: 10px;
    }
    .align-items-container * {
        border: 1px dashed #FF5722;
        box-sizing: border-box;
    }
    .align-items-item {
        width: 50px;
        text-align: center;
    }
    </style>
    align-items: stretch
    <div class="align-items-container" style="align-items: stretch;">
    <div class="align-items-item">1</div>
    <div class="align-items-item">2</div>
    <div class="align-items-item">3</div>
    </div>
    align-items: center
    <div class="align-items-container" style="align-items: center;">
    <div class="align-items-item">1</div>
    <div class="align-items-item">2</div>
    <div class="align-items-item">3</div>
    </div>
    align-items: flex-start
    <div class="align-items-container" style="align-items: flex-start;">
    <div class="align-items-item">1</div>
    <div class="align-items-item">2</div>
    <div class="align-items-item">3</div>
    </div>
    align-items: flex-end
    <div class="align-items-container" style="align-items: flex-end;">
    <div class="align-items-item">1</div>
    <div class="align-items-item">2</div>
    <div class="align-items-item">3</div>
    </div>
    align-items: baseline
    <div class="align-items-container" style="align-items: baseline;">
    <div class="align-items-item">1</div>
    <div class="align-items-item">2</div>
    <div class="align-items-item">3</div>
    </div>
</div>
{% endraw %}

注: 如果 item 有固定高度，则 `stretch` 失效

(5). **align-content**: 多行显示是，交叉轴上的对齐方式

值 | 含义
--- | -----
stretch | item 拉伸填满容器
flex-start | 向交叉轴起点对齐
flex-end | 向交叉轴终点对齐
center | 居中
space-between | 剩余空间均分于 行 之间
space-around | 剩余空间均分于两边

![align-content](http://omhr7p9e5.bkt.clouddn.com/hexo-blog/flex/align-content.gif)

(6). **flex-flow**: flex-direction & flex-wrap 的缩写


### item 属性

(1). **order**: item 排序优先级

值 | 含义
----- | -----
整数 | item 的排序优先级，数值越低，排序越靠前

``` html
<div style="font-size: 12px">
    <style>
    .order-container {
        display: flex;
    }
    .order-container > div {
        width: 50px;
        border: 1px solid #ccc;
        text-align: center;
        background: #76FF03;
    }
    </style>
    <div class="order-container">
    <div style="order: 5">1</div>
    <div style="order: 3">2</div>
    <div style="order: 1">3</div>
    <div style="order: 2">4</div>
    <div style="order: 4">5</div>
    </div>
</div>
```

{% raw %}
<div style="font-size: 12px" class="html-code">
    <style>
    .order-container {
        display: flex;
    }
    .order-container > div {
        width: 50px;
        border: 1px solid #ccc;
        text-align: center;
        background: #76FF03;
    }
    </style>
    <div class="order-container">
    <div style="order: 5">1</div>
    <div style="order: 3">2</div>
    <div style="order: 1">3</div>
    <div style="order: 2">4</div>
    <div style="order: 4">5</div>
    </div>
</div>
{% endraw %}


(2). **flex-basis**: item 在主轴上的初始尺寸

值 | 含义
--- | -----
像素 / 百分比 | item 在主轴上的初始尺寸

注1：sum(basis) = 所有 item 的 flex-basis 和

注2：主轴尺寸 - (sum(basis) + width) = 剩余空间


(3). **flex-grow**: 剩余空间分配给 item 比例

值 | 含义
--- | -----
整数 | item 在主轴上占 container 剩余空间的比例

``` html
<div style="font-size: 12px" class="html-code">
    <style>
    .flex-grow-container {
        display: flex;
        width: 350px;
        background-color: #76FF03;
    }
    .flex-grow-container > div {
        border: 1px solid #ccc;
        text-align: center;
    }
    </style>
    <div class="flex-grow-container">
    <div style="flex-grow: 1">50px</div>
    <div style="flex-grow: 2">100px</div>
    <div style="flex-grow: 3">150px</div>
    <div style="flex-grow: 1">50px</div>
    </div>
</div>
```

{% raw %}
<div style="font-size: 12px" class="html-code">
    <style>
    .flex-grow-container {
        display: flex;
        width: 350px;
        background-color: #76FF03;
    }
    .flex-grow-container > div {
        border: 1px solid #ccc;
        text-align: center;
    }
    </style>
    <div class="flex-grow-container">
    <div style="flex-grow: 1">50px</div>
    <div style="flex-grow: 2">100px</div>
    <div style="flex-grow: 3">150px</div>
    <div style="flex-grow: 1">50px</div>
    </div>
</div>
{% endraw %}

注1：sum(grow) = 所有 item 的 flex-grow 和

注2：grow(1) = `flex-grow: 1;` 的尺寸

注3：grow(1) = (主轴尺寸 - sum(basis)) / sum(grow)


(4). **flex-shrink**: 空间不足时 item 的缩放比例

值 | 含义
--- | -----
整数 | 空间不足时 item 的缩放比例

这个比较不好理解，举个例子:

``` html
<div style="font-size: 12px">
    <style>
    .flex-shrink-container {
        display: flex;
        width: 300px;
        background-color: #00E676;
    }
    .flex-shrink-container > div {
        border: 1px solid #ccc;
        text-align: center;
        flex-basis: 100px;
    }
    </style>
    <div class="flex-shrink-container">
    <div style="flex-shrink: 1">80px</div>
    <div style="flex-shrink: 1">80px</div>
    <div style="flex-shrink: 2">60px</div>
    <div style="flex-shrink: 1">80px</div>
    </div>
</div>
```

{% raw %}
<div style="font-size: 12px" class="html-code">
    <style>
    .flex-shrink-container {
        display: flex;
        width: 300px;
        background-color: #00E676;
    }
    .flex-shrink-container > div {
        border: 1px solid #ccc;
        text-align: center;
        flex-basis: 100px;
    }
    </style>
    <div class="flex-shrink-container">
    <div style="flex-shrink: 1">80px</div>
    <div style="flex-shrink: 1">80px</div>
    <div style="flex-shrink: 2">60px</div>
    <div style="flex-shrink: 1">80px</div>
    </div>
</div>
{% endraw %}

container 的大小为 `300px`，item 的 `flex-basis: 100px`, 4 个 item 一共需要 400px 的空间，因此少了 100px。`sum(shrink)` 的值为 5，`100px / 5 = 20px`，因此 `flex-shrink: 1` 的 item 缩小 20px，`flex-shrink: 2` 的 item 缩小 40px。

注1: 必须与 `flex-basis` 同时使用才生效

注2: shrink(1) = (sum(basis) - 主轴尺寸) / sum(shrink)


(5). **flex**: 缩写

flex: flex-grow flex-shrink flex-basis;

(6). **align-self**: item 在交叉轴上的对齐方式

值 | 含义
--- | -------
stretch | item 拉伸填满容器
center | 居中
flex-start | 向交叉轴起点对齐
flex-end | 向交叉轴终点对齐
baseline | 基线对齐

``` html
<div style="font-size: 12px">
    <style>
    .align-self-container {
        display: flex;
        width: 500px;
        height: 30px;
        background-color: #00E676;
        justify-content: space-between;
    }
    .align-self-container > div {
        text-align: center;
        flex-basis: 80px;
        background-color: #FF5722;
    }
    </style>
    <div class="align-self-container">
    <div style="align-self: stretch">stretch</div>
    <div style="align-self: center">center</div>
    <div style="align-self: flex-start">flex-start</div>
    <div style="align-self: flex-end">flex-end</div>
    <div style="align-self: baseline">baseline</div>
    </div>
</div>
```

{% raw %}
<div style="font-size: 12px" class="html-code">
    <style>
    .align-self-container {
        display: flex;
        width: 500px;
        height: 30px;
        background: #00E676;
        justify-content: space-between;
    }
    .align-self-container > div {
        text-align: center;
        flex-basis: 80px;
        background: #FF5722;
    }
    </style>
    <div class="align-self-container">
    <div style="align-self: stretch">stretch</div>
    <div style="align-self: center">center</div>
    <div style="align-self: flex-start">flex-start</div>
    <div style="align-self: flex-end">flex-end</div>
    <div style="align-self: baseline">baseline</div>
    </div>
</div>
{% endraw %}


## 结尾

关于 Flex 布局的基本介绍与语法学习到此结束。