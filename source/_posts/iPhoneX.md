---
title: 翻译：为 iPhone X 设计网站
date: 2018-01-21
tags:
- iPhone
- CSS
- env(safe-area-inset-*)
---

翻译自 WebKit Blog, 原文: [https://webkit.org/blog/7929/designing-websites-for-iphone-x/](https://webkit.org/blog/7929/designing-websites-for-iphone-x/)

作者 Timothy Horton 于 2017年9月22日

> 以下关于内嵌安全区域的章节更新于 2017年10月31日，用于体现 iOS 11.2 beta 中的改变。

开箱即用，Safari 在新的 iPhone X 上的全面屏(edge-to-edge display)上漂亮地显示着你现在的网站。网站的内容自动内嵌到屏幕的安全区域内，使得它们不会被圆角或者刘海儿遮挡。

内嵌区域会被页面的 `background-color` (`<body>` 或者 `<html>`)所填充以融入到页面其余部分中。对于许多网站来说，这就足够了。如果你的页面背景色固定，且只有文字和图片，那么默认的内嵌方案看起来就挺不错。

其他页面 —— 特别是那些有全屏宽度(full-width)的水平导航栏，如下方页面的 —— 可以选择更进一步地使用这块新屏幕的全部优势。[iPhone X 界面指南](https://developer.apple.com/ios/human-interface-guidelines/overview/iphone-x/) 详细列出了一些需要牢记的通用设计原则，[UIKit 文档](https://developer.apple.com/documentation/uikit/uiview/positioning_content_relative_to_the_safe_area) 讨论了原生应用适配 iPhone X 可以采用的具体实现机制。你的网站可以引入一些类似于 iOS 11 中介绍的 WebKit API 来使用全面屏的优势。

当你在读这篇文章的时候你可以点击任意一张图片来访问一个真实地响应式页面并且可以查看其中的代码。

[![Safari’s default insetting behavior.
](https://webkit.org/wp-content/uploads/default-inset-behavior.png)](https://webkit.org/demos/safe-area-insets/1-default.html)


## 使用整个屏幕

首个新特性的名称是 `viewport-fit`, 是当前 `viewport` meta 标签的一个扩展，用来控制内嵌的表现。`viewport-fit` 在 iOS 11 开始生效。

`viewport-fit` 的默认值是 `auto`, 这个值的效果是自动内嵌，如上所示。如果想禁用掉这个效果并且使页面铺满整个屏幕，你可以设置 `viewport-fit` 的值为 `cover`。此时你的 `viewport` meta 标签应该类似下面：

``` html
<meta name='viewport' content='initial-scale=1, viewport-fit=cover’>
```

重新加载页面后，导航栏看着好多了，从左到右铺满屏幕。但是，很明显地，遵循系统的内嵌安全区域原则很重要：一些页面的内容会被刘海儿挡住，底部的导航栏用起来也会很不友好。

[![Use `viewport-fit=cover` to fill the whole screen.
](https://webkit.org/wp-content/uploads/viewport-fit-cover.png)](https://webkit.org/demos/safe-area-insets/2-viewport-fit.html)


## 遵循安全区域原则

在使用了 `viewport-fit=cover` 后，再一次提高页面可用性的下一步是选择性地对含有重要内容的元素应用 padding 以使它们不被屏幕的形状所遮挡。这么做的好处是通过动态调整来避开边角、刘海儿和 Home 键标识，从而使页面能够充分利用 iPhone X 宝贵的屏幕(screen real eatate)增长。

[![The safe and unsafe areas on iPhone X in the landscape orientation, with insets indicated.
](https://webkit.org/wp-content/uploads/safe-areas-1.png)](https://webkit.org/demos/safe-area-insets/safe-areas.html)

为了实现这个效果，WebKit 在 iOS 11 中包含了一个[新的 css 函数](https://github.com/w3c/csswg-drafts/pull/1817) — `env()` 和一组 [4个预定义的环境变量](https://github.com/w3c/csswg-drafts/pull/1819) — `safe-area-inset-left`, `safe-area-inset-right`, `safe-area-inset-top` 和 `safe-area-inset-bottom`。使用时(When combined)，这几个变量允许样式声明实时引用每一个边当前的安全内嵌区域大小<sup>1</sup>。

1(译者注)：随着屏幕方向(orientation)的改变，`safe-area-inset-*` 的值也会跟着改变。

> env() 函数在 iOS 11 中以 constant() 为名开始使用。从 Safari Technology Preview 41 和 iOS 11.2 beta 开始，constant() 被移除并且以 env() 替代之。如果需要的话，你可以使用 CSS 回退机制来同时支持两个版本，但是应该更侧重于 env()。

所有 `var()` 可以运行的地方 `env()` 都可以 — 例如在 `padding` 属性中：

``` CSS
.post {
    padding: 12px;
    padding-left: env(safe-area-inset-left);
    padding-right: env(safe-area-inset-right);
}
```

对于不支持 `env()` 的浏览器而言，包含此的样式将会被忽略；对于此很重要的进一步的做法是，为使用了 `env()` 的声明单独指定回退方案。

[![Respect safe area insets so that important content is visible.
](https://webkit.org/wp-content/uploads/safe-area-constants.png)](https://webkit.org/demos/safe-area-insets/3-safe-area-constants.html)


## 使用 min() 与 max() 将以上这些结合起来

> 这个章节的内容覆盖了从 Safari Technology Preview 41 与 iOS 11.2 beta 才开始用的特性。

如果你在你的网站设计中使用了内嵌安全区域，你应该会注意到指定除内嵌安全区域外你想要的最小 padding 是有些困难的。在上面的页面中，我们使用了 `env(safe-area-inset-left)` 来指定 12像素 的左边 padding，但是当我们旋转回竖屏，左边的内嵌安全区域变成 0像素，而文字内容直接贴到了屏幕的边缘。

[![Safe area insets are not a replacement for margins.
](https://webkit.org/wp-content/uploads/no-margins.png)](https://webkit.org/demos/safe-area-insets/3-safe-area-constants.html)

为了解决这个问题，浏览器需要知道默认的 padding 或者 内嵌安全区域中较大的是哪一个，而这个较大值就是真实的 padding。在未来的某个 Safari Technology Preview 发行版中，可以使用 [全新的 CSS 函数 `min()` 和 `max()`](https://drafts.csswg.org/css-values/#calc-notation)来实现。这两个函数都接受任意数量的参数，并且返回一个最小值或最大值。它们可以在 `calc()` 内或者嵌套到彼此之内来使用，并且允许 `calc()` 类的函数在其内部使用。

在这种情况下，我们需要使用 `max()`:

``` CSS
@supports(padding: max(0px)) {
    .post {
        padding-left: max(12px, env(safe-area-inset-left));
        padding-right: max(12px, env(safe-area-inset-right));
    }
}
```

在我们的示例页中，在竖屏方向下，`env(safe-area-inset-left)` 解析的值为 0px，所以 `max()` 函数最后的返回值为 12px。在横屏下，应为刘海儿的原因， `env(safe-area-inset-left)` 的值更大，所以 `max()` 的返回值就是 `env(safe-area-inset-left)` 本身的值，确保重要的内容一直可见。

[![Use max() to combine safe area insets with traditional margins.
](https://webkit.org/wp-content/uploads/max-safe-areas-insets.png)](https://webkit.org/demos/safe-area-insets/4-min-max.html)

经验老道的网页开发人员之前可能就遇见过 "CSS locks<sup>2</sup>" 机制，通常用于将 CSS 属性值限制在一定的范围内。同时使用 `min()` 和 `max()` 可以使其更容易地实现，并且未来在实现响应式设计时会发挥重要的作用。

译者注2: [CSS "locks"](https://css-tricks.com/css-locks/)