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
## 三、CSS字体属性
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
  - 不需要设置的属性可以省略（取默认值），但必须保留font-size和font-family属性，否则font属性将不起作用
