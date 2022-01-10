---
title: HTML基础组件记录
category: [coding, html]
sidebar:
    nav: CODING
---

（更新中）

内容及示例基本来自于[W3SCHOOL](www.w3schools.com)，根据个人需要进行了选择性记录。

## 一、 基础组件

### 1. 标题

`<h1>` 到 `<h6>`定义6种不同等级的标题。

### 2. 段落

（1）Tag: `<p>`

示例：

``` html
<p>This is a paragraph.</p>
```

（2）与段落tag组合的使用的tag通常有`<hr>`和`<br>`，分别是分割线和换行。分割线在用于段落与段落之间的分隔，`<br>`可以用于段落中用来给文本换行，如：

```html
<p>This is<br>a paragraph<br>with line breaks.</p>
```

（3）此外，`<pre>`可以替换`<p>`。该组件可以按照源码中的文本内容的空间格式来显示文本，包括空格和换行。

### 3. 链接

（1）Tag: `<a>`

示例：

```html
<a href="https://codingcm.github.io">This is a link</a>
```

### 4. 图片

（1）Tag: `<img>`

主要参数包括src (source file)，alt (alternative text)，width和height。

示例：

```html
<img src="hello.jpg" alt="Hello!" width="104" height="142">
```

图像的尺寸也可以通过百分比的方式来设定，如：
```html
<img src="hello.jpg" alt="Hello!" width="50%" height="50%">
```

### 5. 组件风格修改

（1）Tag: `<head>`和`<style>`或`<link>`组合使用。

无论是a) 内部的单页的内部风格定义还是b) 整个项目的全局风格定义，都要通过`<head>`来包含该部分的内容。

内部风格定义示例：

```html
<!DOCTYPE html>
<html>
<head>
<style>
body {background-color: powderblue;}
h1   {color: blue;}
p    {color: red;}
</style>
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

全局风格定义：

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="styles.css">
</head>
<body>

<h1>This is a heading</h1>
<p>This is a paragraph.</p>

</body>
</html>
```

其中，style.css的内容为：

```css
body {
  background-color: powderblue;
}
h1 {
  color: blue;
}
p {
  color: red;
}
```










## 二、 组件属性

### 1. title属性

任意一个组件都可以加上一个title，加上title以后，鼠标移动到相应组件上则会显示对应的title。

示例：

```html
<p title="I'm a tooltip">Mouse over this paragraph, to display the title attribute as a tooltip.</p>
```

### 2. style属性



### 3. class属性

任意组件都有class属性。class属性通常是`<head>`中通过内部或外部CSS类自定义的一种组件风格。任意组件都可以使用这个风格。例如：

```html
<!DOCTYPE html>
<html>
<head>
<style>
.note {
  font-size: 120%;
  color: red;
}
</style>
</head>
<body>

<h1>My <span class="note">Important</span> Heading</h1>
<p>This is some <span class="note">important</span> text.</p>

</body>
</html>
```

注意，在第1章第5节中，CSS类的定义方式以组件类型为单位，这里是以风格为单位，即所有的组件都可以使用这种风格（当该组件有这种属性时）。

class和style起到的是相同的作用，class只需要统一定义一次，后面每次使用这个class即可。

### 4. id属性

任一组件的id属性是该组件的唯一标识符。在JS中通常通过id来获取相应的组件。
