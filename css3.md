# CSS3
## 一、CSS简介
### 1. CSS语法规范
+ CSS规则由两个主要的部分构成：选择器以及一条或多条声明
### 2. CSS代码风格
+ 空格规范
  - 属性值前面，冒号后面，保留一个空格
  - 选择器（标签）和大括号中间保留空格
## 二、CSS选择器
### 1. 基础选择器
+ 标签选择器
+ 类选择器
  - 多类名：多个类名中间用空格分开
+ id选择器
+ 通配符选择器：`*`，表示选取页面中所有元素（标签）
## 三、CSS引入方式
+ 行内样式表（行内式）
  - style其实就是标签的属性
  - 在**双引号**中间，写法要符合CSS规范
+ 内部样式表（嵌入式）
  - <style>标签理论上可以放在HTML文档的任何地方，但一般会放在文档的<head>标签中
+ 外部样式表（链接式）
  - ```
  <link rel='stylesheet' href=''>
  ```
## 四、字体属性
+ `font-family`：字体系列
+ `font-size`：字体大小
  - 谷歌浏览器默认字体大小为16px
+ `font-weight`：字体粗细
  - 400 = normal
  - 700 = bold
+ `font-style`：文字样式
  - normal
  - italic（斜体）
+ 复合属性
  - ```font: font-style font-weight font-size/line-height font-family;```
  - 不需要设置的属性可以省略（取默认值），但必须保留**font-size**和**font-family**属性，否则font属性将不起作用
## 五、文本属性
+ `color`：文本颜色
  - 开发中最常用的是十六进制
+ `text-align`：对齐文本
  - 用于设置元素内文本内容的**水平**对齐方式
  - left（左对齐）
  - right（右对齐）
  - center（居中对齐）
+ `text-decoration`：装饰文本
  - none
  - underline
  - overline（上划线）
  - line-through（删除线）
+ `text-indent`：文本缩进
  - 用来指定文本的第一行的缩进，通常是将段落的首行缩进
  - 2em => em是一个相对单位，就是当前元素一个文字的大小，如果当前元素没有设置大小，则会按照父元素的一个文字大小
+ `line-height`：行间距（行高）
  - 行间距（行高） = 上间距+文本高度+下间距
## 六、
