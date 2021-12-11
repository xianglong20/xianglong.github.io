---
title: "Less 快速入门"
author: "imByte"
date: 2021-12-08T20:48:08+08:00
draft: true
# 用在主页预览的文章特色图片
featuredImagePreview: "/posts_img/44.png"
---
# Less 概述
---
Less （Leaner Style Sheets 的缩写） 是一门向后兼容的 CSS 扩展语言。

Less 作为 CSS 的一种形式扩展，并没有减少 CSS 的功能，而是在现有的 CSS 语法上，添加了程序式语言的特性。

来看下面这段 Less 代码：
```css
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```
实际上它等价于 CSS 代码：

```css
#header {
  width: 10px;
  height: 20px;
}
```
这就是 Less 中变量的应用，除此之外，Less 中还引入了混合（Mixin）、嵌套、运算、函数等功能，简化了 CSS 的编写，为样式编写提供了巨大便利。
>  [>> Less 官方文档](https://less.bootcss.com/)


# Less 编译
---
在本质上，Less 包含一套自定义的语法以及一个解析器，用户根据这些语法去定义自己的样式规则。

这些规则最终会通过解析器，编译生成对应的 CSS 文件。

那么，如何进行解析呢，这需要利用 VS code 中插件 <font color = red>>> Easy Less</font> 。

<center><img src='https://img-blog.csdnimg.cn/2fe47a8c82554c449381551638ec4fda.png'></center>

此后，每次保存 .less 文件后，就会自动生成对应的 .css 文件。当用到该样式时，直接在 .html 引入对应生成的 .css 文件即可。


了解编译原理和如何引入后，下面来介绍 Less 语法。
# Less 变量
---
在设置样式时如果不想给定固定值，则可使用变量来替代。

```css
@变量名: 值;
```
<font size = 4>**命名变量也是要有规范的，如下**</font>
1. 必须以 @ 开头
2. 不能包含特殊字符
3. 不能以数字开头
4. 大小写敏感

```css
@color: tomato;
@font20: 20px;

.box h1{
  background-color: @color;
  font-size: @font20;
}
```
# Less 嵌套
---
对于下面这段 CSS 代码：
```css
.box {
  width: 200px;
}
.box .son {
  color: tomato;
}
.box .son a {
  font-size: 15px;
}
```
我们可以利用 Less 来这样编写：

```css
.box {
  width: 200px;
  .son {
    color: tomato;
    a {
      font-size: 15px;
    }
  }
}
```
这就有点像 HTML 结构了，层层嵌套，减少代码量的同时也使样式代码更具逻辑性。

<font size = 4>**此外，当遇到（交集|伪类|伪元素）选择器时，需要在内层选择器前加 `&` 。**</font>

```css
a {
  font-size: 15px;
  &:hover {
    color: orange;
  }
}
```
或者是这样：

```css
.nav {
  .logo{
    width: 100px;
  }
  &::before {
    content: '';
  }
}
```
否则，`:hover` 和 `::before` 则会被当做后代选择器来看待：（注意`a` 和 `:hover` 之间有空格）

```css
.box .son a :hover {
  color: orange;
}
```
# Less 运算
---
算术运算符 `+、-、*、/` 可以对任何数字、颜色或变量进行运算。如果可能的话，算术运算符在加、减或比较之前会进行单位换算。


```css
.box {
  width: 200px * 2;
  height: 200px - 100;	
}
```
转换为 CSS

```css
.box {
  width: 400px;
  height: 100px;
}
```
注意，在使用 `/` 运算时，需要加 `()` 才能正常运算：

```css
.box {
  width: (100px / 2);
  height: 100px / 2; 
}
```

```css
.box {
  width: 50px;
  height: 100px / 2;
}
```

此外，关于单位，有几点注意事项：
1. 如果运算数中只有一个单位，以该单位为准
2. 如果有多个单位，则以最左侧单位为准

# Less 混合
---
在 Less 编写样式时，可以直接将其他类的样式整体引入。
```css
.shape {
  width: 100px;
  height: 100px; 
}

.box {
  color: tomato;
  .shape();
}
```
转为 CSS 代码

```css
.shape {
  width: 100px;
  height: 100px;
}
.box {
  color: tomato;
  width: 100px;
  height: 100px;
}
```
# Less 函数
---
Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。

例如，下面是逻辑函数 `if()` 和数学函数 `sqrt()` 的应用：
```css
div {
  margin: if(2 > 1, 0, 3px);
  padding: sqrt(25cm);
}
```
转为 CSS 代码
```css
div {
  margin: 0;
  padding: 5cm;
}
```

Less 内置函数较多，具体可参见 [>> Less 函数手册](https://less.bootcss.com/functions/)



