---
layout:     post
title:      "为什么上线的App没有Sketch里的设计稿好看？"
subtitle:   ""
date:       2018-05-22 12:00:00
author:     "Yice"
header-img: "img/test-index-bg.jpg"
tags:
    - Design
---

本文翻译自Nathan Gitter 的文章     [原文链接](https://medium.com/@nathangitter/why-your-app-looks-better-in-sketch-3a01b22c43d7?from=singlemessage&isappinstalled=0) 

# 发现不同

你能看出这两张图的差别吗？
![](https://user-gold-cdn.xitu.io/2018/5/19/16378e50cc017ef3?w=2000&h=669&f=jpeg&s=143343)
仔细看，你就会注意到细微的差别，右边的图片：  
* 阴影范围更大；  
* 渐变的颜色更暗；  
* 段落中文字的第一行有一个单词“in”； 

左边的图片是Sketch中的设计稿的截图，右边的图片是在iOS中的还原。这些差异会在图像渲染时出现，尽管两张图中用了完全相同的字体、行间距、阴影半径、颜色和渐变属性。  

![](https://user-gold-cdn.xitu.io/2018/5/19/16378e83a65c4e57?w=600&h=460&f=gif&s=318227)
如你所见，把设计稿转换成真实的代码时，一些细节就会丢失。接下来我会深入探讨这些细节并告诉你如何做才能避免信息的丢失。
---- ---

# 为什么要关注细节？

设计对于一款应用的成功与否至关重要，尤其在iOS系统中，因为用户会更喜欢使用流畅和好看的App。  

如果你是一个移动端app的设计师或者开发者，你就会知道细节对于用户体验的重要性。高质量的软件来自于开发者对于每个细节的深度考虑。  

App没有设计原稿好看有很多原因，我将探讨最微妙的原因——Sketch和iOS有不同的渲染方式。  
![](https://user-gold-cdn.xitu.io/2018/5/19/16378eb486099323?w=2000&h=888&f=jpeg&s=44623)
----- ---

# 转化中丢失的信息

几种UI元素在Sketch和iOS中有显著的差异：  
* 字体  
* 阴影  
* 渐变  

## 1、排版

字体和排版可以用很多方式实现，本文中我使用UILabel进行测试验证（Sketch中用的是“Text”，iOS中用的Label）。  

让我们看一个例子：
![](https://user-gold-cdn.xitu.io/2018/5/19/16378ee78fa11aac?w=2000&h=952&f=jpeg&s=303847)
上面例子中最大的区别是换行的位置。第三段的文字在Sketch中是从“25”开始断行，而在app中则从“point”断行。同样的问题也出现在文本段落中——断行不一致。  

另一个细微的差别是Sketch中的行距和字间距会稍微大一些。  

下面的动图可以更清楚地看到两张图片的差异：  
![](https://user-gold-cdn.xitu.io/2018/5/19/16378f11fb2efebb?w=450&h=550&f=gif&s=238560)
使用其他的字体会怎样呢？用Lato（另一种常用的免费字体）替换掉San Francisco会得到下面结果：
![](https://user-gold-cdn.xitu.io/2018/5/19/16378f1a14e47ebb?w=450&h=536&f=gif&s=202841)
好多了！  

虽然他们的行距和字间距依然有差别，但这些差别非常细微。但是也要注意，如果文本需要与其他元素（例如背景图片）对齐，这些差异在视觉上就会被放大。 

### 怎么解决？  

造成以上问题的原因，一部分是由于iOS的默认字体：San Francisco，当iOS渲染系统默认字体时，系统会自动的调整字间距，可以在[苹果官网](https://developer.apple.com/fonts/)上找到，如果你的设计字体采用了SF,强烈推荐SF Font Fixer的Sketch插件Sketch plugin 可以解决这个比较烦人的问题。  
>小贴士：在设计时，尽量让Text box适配文字所占的大小，不要用过大的Textbox，并且可以设置自动对其，然后再裁剪text的宽度，如果textbox有着较多的冗余宽度，则非常容易造成其他的还原错误。  

## 2、阴影

不同于字体排版有着通用的设计规则，阴影的设计则因人而异。
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c7f35b20984d?w=2000&h=1002&f=jpeg&s=65188)
例如上图的图片，iOS中的默认阴影比较大，这使得上面的例子中矩形的顶部边缘的差异最大。  

阴影的设置也是有小技巧的，iOS和Sketch的阴影参数也有着一些差异，主要的原因是由于iOS中的CALayer中没有 “spread”的概念，尽管可以通过增大包含阴影的layer的大小来解决这个问题。
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c80047c77be0?w=2000&h=511&f=jpeg&s=86437)
Sketch给出的设计稿与真实的应用中的阴影差异有可能是非常大的，甚至有些在Sketch上看着很不错的阴影效果，但是在设备上运行时，却几乎看不到。  
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c818b50b3692?w=450&h=635&f=gif&s=60614)

### 怎么解决？

调节阴影参数，将其还原的与设计稿一致的小技巧:把阴影的圆角降低，同时提高阴影的不透明度，代码如下：  
```
// old  
layer.shadowColor = UIColor.black.cgColor  
layer.shadowOpacity = 0.2  
layer.shadowOffset = CGSize(width: 0, height: 4)  
layer.shadowRadius = 10  

// new  
layer.shadowColor = UIColor.black.cgColor  
layer.shadowOpacity = 0.3  
layer.shadowOffset = CGSize(width: 0, height: 6)  
layer.shadowRadius = 7
```
当然，随着大小、颜色以及形状的不同，参数的改变也是不一样的，这里我们仅仅需要做有限的几处改动。

## 3、渐变

渐变的还原也是一个令人头痛的问题。  

下面三个渐变图中，只有橙色（上面）和蓝色（右下角）与设计稿是有差异的。
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c8a1158b648b?w=2000&h=1190&f=jpeg&s=134864)
在设计稿中，橙色的渐变色更加偏向于水平方向的渐变，但是在iOS中却更偏向于垂直渐变，因此整体来说，应用的颜色看起来要比设计稿中更暗。  

这种差异在蓝色中更加明显，iOS中的渐变更偏向于垂直方向。蓝色的渐变是由三种颜色定义而成，左下角为浅蓝，中间为深蓝，右上角为粉色。
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c8a763109ee6?w=450&h=636&f=gif&s=370193)

### 怎么解决？  

因此，在iOS中的，如果发现渐变的角度与还原的不一致，需要将起始点和终点进行调整，可以将<code>CAGradientLayer</code>的<code>startPoint</code>和<code>endPoint</code>进行微调，调节设计稿与还原之间的差异。  
```
// old  
layer.startPoint = CGPoint(x: 0, y: 1)  
layer.endPoint = CGPoint(x: 1, y: 0)  

// new  
layer.startPoint = CGPoint(x: 0.2, y: 1)  
layer.endPoint = CGPoint(x: 0.8, y: 0)  
```
当然，这里其实也没有一个通用的魔法公式，都是需要亲自手工的不断调整并将差异缩小。  

Jirka Třečák 发表了一篇很棒的文章，解释了在iOS中的渐变是如何渲染的，如果你想深入了解的话，可以认真研究下[这篇文章](https://medium.com/@JiriTrecak/as-for-the-gradients-there-actually-is-a-magic-formula-89055944b52a)。
---- ---

# 更多信息

文中所提到的iOS与Sketch的差异，我都通过Demo的方式进行展示，包括插图中的每个例子，以及原始的Sketch文件，通过Demo,你可以更直观了解到这些差异。你可以展示给你的团队，这种方式可以更好的理解设计与开发区别，从而开发出更好的应用。    

Demo的地址为:[https://github.com/nathangitter/sketch-vs-ios](https://github.com/nathangitter/sketch-vs-ios)
![](https://user-gold-cdn.xitu.io/2018/5/20/1637c952ce36f212?w=1393&h=888&f=jpeg&s=137925)
---- ---

# 小贴士

永远不要迷信相同的值会有着相同的结果，尽管数字相同，但其视觉表现是不同的。
最后，每个好的设计都是在不断的迭代中产生的，开发哥哥和设计小姐姐的良好协作，也是保证高质量产品的关键。


