# jQuery
## 一、jQuery的基本使用
#### 1. 入口函数
```
$(function() {

});
```
```
$(document).ready(function(){

})
```
+ 等着DOM结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成
+ 相当于原生js中的DOMContentLoaded
#### 2. $
+ $是jQuery的别称，可以使用jQuery代替$
+ $是jQuery的顶级对象，相当于原生js中的window
#### 3. jQuery对象和DOM对象
+ DOM对象：用原生js获取过来的对象
+ jQuery对象：用jQuery方式获取过来的对象
  - 本质： 通过$把DOM元素进行了封装（伪数组形式存储）
  - `$('div')`
+ jQuery对象只能使用jQuery方法，DOM对象则使用原生js的属性和方法
+ DOM对象与jQuery对象之间是可以相互转换的
  - DOM对象转换为jQuery对象：`$(DOM对象)`
  - jQuery对象转换为DOM对象
    * `$('div')[index]` index是索引号
    * `$('div').get[index]` index是索引号
 
## 二、jQuery常用API
### (1) jQuery选择器
#### 1. jQuery选择器
+ 基础选择器
  - `$("选择器")` 里面选择器直接写css选择器即可，但是要加引号
+ 层级选择器
  - 子代选择器
    * `$("ul>li")`
  - 后代选择器
    * `$("ul li")`
#### 2. 隐式迭代
+ 遍历内部DOM元素（伪数组形式存储）的过程就叫做隐式迭代
#### 3. jQuery筛选选择器
+ `:list` 获取第一个li元素
+ `:last` 获取最后一个li元素
+ `:eq(index)` 获取到的元素中，选择索引号为index的元素（index从0开始）
+ `:odd` 获取到的元素中，选择索引号为奇数的元素
+ `:even` 获取到的元素中，选择索引号为偶数的元素
+ `:checked` 查找被选中的表单元素
#### 4. jQuery筛选方法
+ `parent()` 父级（亲爸爸）
+ `parents(selector)` 指定祖先元素
+ `children(selector)` 最近一级（亲儿子）
+ `find(selector)` 后代
+ `siblings(selector)` 兄弟节点，不包括自己本身
+ `nextAll([expre])` 当前元素之后的所有同辈元素
+ `prevtAll([expre])` 当前元素之前的所有同辈元素
+ `eq(index)`
+ `hasClass(class)` 检查当前的元素是否含有某个特定的类，如果有，则返回true
#### 5. 排他思想
```
$("button").click(function() {
  $(this).css("background", "pink");
  $(this).siblings("button").css("background", "");
});
```
+ 获得索引号
`var index = $(this).index();`
#### 6. 链式编程
`$(this).css("color", "pink").siblings().css("color", "");`

### (2) jQuery样式操作
#### 1. 操作css方法
+ 参数只写属性名，则是返回属性值
  - `$(this).css('color');`
+ 参数是属性名，属性值，逗号分隔，是设置一组样式
  - 属性必须加引号，值如果是数字可以不用跟单位和引号
+ 参数可以是对象形式，设置多组样式
  - 属性名和属性值用冒号隔开
  - 属性可以不加引号（驼峰命名法），值如果是数字可以不用跟单位和引号
#### 2. 设置类样式方法
+ `addClass()` 添加类
  - `$("div").addClass("");`
  - 注意操作类里面的参数不要加点
+ `removeClass()` 删除类
+ `toggleClass()` 切换类
+ 原生js中className会覆盖元素原先里面的类名
+ jQuery里面类操作只是对指定类进行操作，不影响原先的类名

### (3) jQuery效果
#### 1. 显示隐藏
  - `show([speed, [easing], [fn]])` 显示
    * 参数都可以省略，无动画直接显示
    * speed: slow/normal/fast/表示动画时长的毫秒数值
    * easing："swing"(默认)/"linear"
    * fn：回调函数，在动画完成时执行的函数，每个元素执行一次
  - `hide([speed, [easing], [fn]])` 隐藏
  - `toggle([speed, [easing], [fn]])` 切换
#### 2. 滑动
  - `slideDown([speed, [easing], [fn]])` 下滑
  - `slideUp([speed, [easing], [fn]])` 上滑
  - `slideToggle([speed, [easing], [fn]])` 滑动切换
#### 3. 事件切换
  - `hover([over,] out)`
    * over：鼠标移到元素上要触发的函数
    * out：鼠标移出元素要触发的函数
  - 如果只写一个函数，那么鼠标经过和离开都会触发这个函数
#### 4. 动画队列及其停止排队方法
  - 动画或效果队列
    * 动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行
  - 停止排队
    * `stop()` 用于停止动画或效果
    * 写到动画或者效果的前面，相当于停止结束上一次的动画
#### 5. 淡入淡出
  - `fadeIn([speed, [easing], [fn]])` 
  - `fadeOut([speed, [easing], [fn]])`
  - `fadeToggle([speed, [easing], [fn]])`
  - `fadeTo(([speed, opacity, [easing], [fn]]))` 渐进方式调整到指定的不透明度
    * opacity：透明度必须写，取值0~1之间
    * speed：必须写
#### 6. 自定义动画
  - `animate(params, [speed], [easing], [fn])`
    * params: 想要更改的样式属性，以对象形式传递，必须写。属性名可以不用带引号（驼峰命名法）

### (4) jQuery属性操作
#### 1. 设置或获取元素固有属性值
+ `prop("属性名")` 获取属性
+ `prop("属性", "属性值")` 设置属性
#### 2. 设置或获取元素自定义属性值
+ `attr("属性")` 获取属性
+ `attr("属性", "属性值")` 设置属性
+ 可以获取H5自定义属性：`attr("data-index")`
#### 3. 数据缓存
+ 在指定的元素上存取数据，并不会修改DOM元素结构。一旦页面刷新，之前存放的数据都将被移除
+ `data("name", "value")` 附加数据
+ `data("name")` 获取数据 
+ 可以读取H5自定义属性data-index：`data("index")`，返回的是数字型

### (5) jQuery文本属性值
#### 1. 普通元素内容
+ `html()` 获取元素的内容
+ `html("")` 设置元素的内容
#### 2. 普通元素文本内容
+ `text()` 获取元素的文本内容
+ `text("")` 设置元素的文本内容
#### 3. 表单值
+ `val()`
+ `val("")`
#### 4. 保留小数
+ `toFixed(number)`

### (6) jQuery元素操作
#### 1. 遍历元素
+ `each(function(index, domEle) {})`
  - domEle是每个DOM元素对象，不是jQuery对象 
  ```
  var arr = ["red", "green", "blue"];
  $("div").each(function(i, domEle) {
    $(domEle).css("color", arr[i]);
  })
  ```
+ `$.each(object, function(index, element) {})`
  - 可用于遍历任何对象，主要用于数据处理，比如数组，对象
  - 遍历对象时，index输出的是属性名，element输出的是属性值
#### 3. 
  
