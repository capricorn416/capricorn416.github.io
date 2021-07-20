# jQuery
## 一、jQuery的基本使用
### (1) 入口函数
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
### (2) $
+ $是jQuery的别称，可以使用jQuery代替$
+ $是jQuery的顶级对象，相当于原生js中的window
### (3) jQuery对象和DOM对象
+ DOM对象：用原生js获取过来的对象
+ jQuery对象：用jQuery方式获取过来的对象
  - 本质： 通过$把DOM元素进行了封装（**伪数组**形式存储）
  - `$('div')`
+ jQuery对象只能使用jQuery方法，DOM对象则使用原生js的属性和方法
+ DOM对象与jQuery对象之间是可以相互转换的
  - DOM对象转换为jQuery对象：`$(DOM对象)`
  - jQuery对象转换为DOM对象
    * `$('div')[index]` index是索引号
    * `$('div').get[index]` index是索引号
### (4) jQuery冲突问题
#### 1. 释放jQuery的使用权
+ `jQuery.noConflict();`
  - 释放操作必须在编写其他jQuery代码之前，释放之后就不能再使用$，改为使用jQuery
#### 2. 自定义访问符号
+ `var nj = jQuery.noConflict();`

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
#### 3. jQuery内容选择器
+ `:empty` 查找既没有文本内容也没有子元素的指定元素
+ `:parent` 查找有文本内容或有子元素的指定元素
+ `:contains(text)` 查找包含指定文本内容text的指定元素
+ `:has(selector)` 查找包含指定子元素的指定元素
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
+ 链式操作
  - `$("div").css("width", "100px").css("height", "100px")`
+ 参数可以是对象形式，批量设置多组样式
  - 属性名和属性值用冒号隔开
  - 属性可以不加引号（驼峰命名法），值如果是数字可以不用跟单位和引号
#### 2. 设置类样式方法
+ `addClass()` 添加类
  - `$("div").addClass("");`
  - 注意操作类里面的参数不要加点
  - 多个类名之间用空格隔开
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
  - 子元素移入移出不会触发父元素的事件
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
    * 是元素做动画
      - `$("body, html").animate()`而不是`$(document)`
### (4) jQuery属性操作
#### 1. 设置或获取元素固有属性值
+ `prop("属性名")` 获取属性
+ `prop("属性", "属性值")` 设置属性
+ prop方法不仅能够操作属性，还能操作属性节点
+ `removeProp(name)`
#### 2. 设置或获取元素自定义属性值
+ `attr("属性")` 获取属性
  - 无论找到多少个元素，都只返回第一个元素指定的属性节点的值
+ `attr("属性", "属性值")` 设置属性
  - 找到多少个元素，就会设置多少个元素
  - 如果设置的属性节点不存在，那么系统会自动新增
+ 可以获取H5自定义属性：`attr("data-index")`
+ `removeAttr(name)`
  - 会删除所有找到元素的指定的属性节点
  - `$("span").removeAttr("class name");`
+ 在操作属性节点时，具有true和false两个属性的属性节点，如checked，selected或者disabled，使用prop()，其他的使用attr()
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
  - 可用于遍历任何对象，主要用于数据处理，比如数组,伪数组，对象
  - 遍历对象时，index输出的是属性名，element输出的是属性值
#### 2. 创建元素
+ `$("")`
  - `var li = $("<li>我是后来创建的li</li>");`
#### 3. 添加元素
+ 内部添加
  - `element.append("内容")` 把内容放入匹配元素内部最后面
  - `element.prepend("内容")` 把内容放入匹配元素内部最前面
  - 内部添加元素，生成之后，它们是父子关系
+ 外部添加
  - `element.after("内容")` 把内容放入目标元素后面
  - `element.before("内容")` 把内容放入目标元素前面
  - 外部添加元素，生成之后，它们是兄弟关系
#### 4. 删除元素
+ `element.remove()` 删除匹配的元素（本身）
+ `element.empty()` 删除匹配的元素集合中所有的子节点
+ `element.html("")` 清空匹配的元素内容
  
### (7) jQuery尺寸、位置操作
#### 1. jQuery尺寸
+ `width()/height()` 只算width/height
+ `innerWidth()/innerHeight()` 包含padding
+ `outerWidth()/outHeight()` 包含padding、border
+ `outerWidth(true)/outerHeight(true)` 包含padding、border、margin
+ 如果参数为空，则是获取相应值，返回的是数字型
+ 如果参数为数字，则是修改相应值，参数可以不写单位
#### 2. jQuery位置
+ `offset()` 设置或返回被选元素相对于**文档**的偏移坐标，跟父级没有关系
  - 有2个属性`offset().top`和`offset().left`
  - 可以设置元素的偏移：`offset({top: 10, left: 30})`
+ `position()` 返回被选元素相对于**带有定位的父级**偏移坐标，如果父级都没有定位，则以文档为准
  - 只能获取不能设置
+ `scrollTop()/scrollLeft()` 设置或获取元素被卷去的头部和左侧

## 三、jQuery事件
### (1) jQuery事件注册
+ 单个事件注册
  - `element.事件(function() {})`
    * 可以添加多个相同或者不同类型的事件，不会覆盖
### (2) jQuery事件处理
#### 1. `on()` 绑定事件
+ 在匹配元素上绑定一个或多个事件
  - `element.on(events, [selector], fn)`
    * events：一个或多个用空格分隔的事件类型
    * selector：元素的子元素选择器
    * 可以添加多个相同或者不同类型的事件，不会覆盖
    ```
    $("div").on({
      mouseenter: function() {},
      click: function() {}
    })
    ```
    ```
    $("div").on("mouseenter mouseleave", function() {})
    ```
+ 可以事件委派：把原来加给子元素身上的事件绑定在父元素身上
  - `$("ul").on("click", "li", function() {});`
  - `$("ul").delegate("li", "click", function() {})`
+ 动态创建的元素，click()没有办法绑定事件，on()可以给后来动态生成的元素绑定事件
#### 2. `off()` 解绑事件
+ `$("p").off();` 解绑p元素所有事件处理程序
+ `$("p").off("click");` 解绑p元素上面所有的点击事件
+ `$("button").off("click", test1)` 解绑指定类型的指定事件
+ `$("ul").off("click", "li");` 解绑事件委托
#### 3. `one()` 绑定只触发一次的事件
#### 4. `trigger()` 自动触发事件
+ `element.click()`
+ `element.trigger("type")`
  - 会触发事件冒泡
  - 会触发元素的默认行为，不会触发<a>的默认跳转
+ `element.triggerHandler("type")`
  - 不会触发事件冒泡
  - 不会触发元素的默认行为，不会触发<a>的默认跳转
  ```
  $("div").on("click", function() {});
  // $("div").click();
  // $("div").trigger("click");
  $("div").triggerHandler("click");
  ```
+ 解决方法：```<a href=""><span>注册</span></a>``` 绑定事件到span上

### (3) jQuery事件对象
+ `element.on(events, [selector], function(event) {})`
+ `event.preventDefault()`或者`return false` 阻止默认行为
+ `event.stopPropagation()`或者`return false` 阻止冒泡
### (4) 自定义事件
+ 事件必须是通过on绑定的
+ 事件必须通过trigger/triggerHandler来触发
  ```
  $("div").on("myClick", function() {});
  $("div").trigger("myClick");
  ```
### (5) 事件命名空间
+ 事件是通过on绑定的
+ 事件通过trigger/triggerHandler来触发
  ```
  $("div").on("click.zs", function() {});
  $("div").on("click.ls", function() {});
  $("div").trigger("click.ls");
  ```
+ 利用trigger触发子元素带命名空间的事件，则父元素带命名空间的事件也会被触发，而父元素没有命名空间的事件不会被触发
+ 利用trigger触发子元素不带命名空间的事件，则子元素和父元素所有相同类型的事件都会被触发
  
## 四、jQuery其他方法
### (1) jQuery拷贝对象
+ 如果想要把某个对象拷贝（合并）给另外一个对象使用，此时可以使用$.extend()方法
+ `$.extend([deep], target, object1, [objectN])`
  - deep：默认为false，浅拷贝；true为深拷贝
    * 浅拷贝是把被拷贝
  - target：要拷贝的目标对象
  - object1：待拷贝到第一个对象的对象
### (2) 移入移出
+ mouseover/mouseout事件，子元素移入移出也会触发父元素的事件
+ mouseenter/mouseleave事件，子元素移入移出不会触发父元素的事件
## 五、jQuery静态方法
### (1) each()
+ `$.each(object, function(index, element) {})`
  - 默认的返回值：遍历谁就返回谁
  - 不支持在回调函数中对遍历的数组进行处理
### (2) map()
+ `$.map(arr, function(value, index)) {}`
  - 可以遍历伪数组
  - 默认的返回值是一个空数组
  - 可以在回调函数中通过return对遍历的数组进行处理，然后生成一个新的数组返回
### (3) trim()
+ `$.trim(string)`
  - 返回去除两端空格之后的字符串
### (4) isWindow()
+ `$.isWindow(obj)`
  - 判断传入的对象是否是window对象
  - 返回值是布尔值
### (5) isArray()
+ `$.isArray(arr)`
  - 判断传入的对象是否是真数组
  - 返回值是布尔值
### (5) isFunction()
+ `$.isFunction(fn)`
  - 判断传入的对象是否是函数
  - jQuery框架本质上是一个函数
  - 返回值是布尔值
### (6) holdReady()
+ `$.holdReady(true);` 暂停入口函数执行
+ `$.holdReady(false);` 恢复入口函数执行
  
# php
+ 基础语法
  ```
  <?php
    // 定义变量
    $num = 10;
    // 打印内容
    echo $num;
    // 定义数组
    $arr = array(1, 3, 5);
    print_r($arr);
    echo "<br>";
    echo $arr[1];
    // 定义对象（字典）
    $dict = array("name"=> "lnj", "age"=> 33);
    print_r($dict);
    echo $dict['name'];
    // 分支语句
    $age = 18;
    if($age >= 18) {
      echo '成年人';
    }else {
      echo '未成年人';
    }

    switch($age) {
      case 0:
        echo '0';
        break;
      case 18:
        echo '18';
        break;
      default:
        echo '未成年人';
        break;
    }
    // 三目
    $res = ($age >= 18) ? '成年人' : '未成年人';
    echo $res;
    // 循环
    for($i = 0; $i < count($arr); $i++) {
      echo $arr[$i];
    }
    $index = 0;
    while($index < count($arr)) {
      echo $arr[$index];
      $index++;
    }
  ?>
  ```
+ get请求
  - get请求会将提交的数据拼接到URL后面?name1=value1&name2=value2
  - ```<form action="xxx.php" method="GET">```
  - php中可以通过`print_r($_GET);`或者`echo($_GET[name]);`获取前端通过get请求提交的数据
  - GET请求对数据有大小限制，用于提交非敏感数据和小数据
+ post请求
  - ```<form action="xxx.php" method="POST">```
  - post请求会将提交的数据放到请求头（开发者工具——Network）中
  - php中可以通过`print_r($_POST);`或者`echo($_POST[name]);`获取前端通过get请求提交的数据
  - POST请求对数据没有大小限制，用于提交敏感数据和大数据
  - 文件上传
    * `<form action="xxx.php" method="POST" enctype="multipart/form-data">`
    * php
      - 上传的文件在PHP中可以通过`$_FILES`获取
      - PHP中文件默认会上传到一个临时目录，接收完毕之后会自动删除
      ```
      <?php
        // 获取上传文件对应的字典
        $fileInfo = $_FILES['upFile'];
        // 获取上传文件的名称
        $fileName = $fileInfo['name'];
        // 获取上传文件保存的临时路径
        $filePath = $fileInfo['tmp_name'];
        // 移动文件
        move_uploaded_file($filePath, './source/'.$fileName);
      ?>
      ```
# AJAX
## 一、GET基本使用
### (1) 步骤
#### 1. 创建一个异步对象
```var xmlhttp = new XMLHttpRequest();```
#### 2. 设置请求方式和请求地址
```xmlhttp.open(method, url, async);```
  - method：请求的类型 GET或POST
  - url：文件在服务器上的位置
  - async：true（异步）或false（同步）
#### 3. 发送请求
```xmlhttp.send();```
#### 4. 监听状态的变化
+ readyState
  - 0：请求未初始化
  - 1：服务器连接已建立
  - 2：请求已接收
  - 3：请求处理中
  - 4：请求已完成，且响应已就绪
#### 5. 处理返回的结果
```
xmlhttp.onreadystatechange = function() {
  if(xmlhttp.readyState === 4){
    if(xmlhttp.status >= 200 && xmlhttp.status < 300 || xmlhttp.status === 304) {
      console.log(xmlhttp.responseText);
    }else{
      console.log("没有接收到服务器返回的数据");                                              
    }                                                
  }
}
```
+ 服务器响应
  - responseText 获得字符串形式的响应数据                                                  
  - responseXML 获得XML形式的响应数据
### (2) 兼容IE
+ 现代浏览器均支持XMLHttpRequest对象，IE5和IE6使用ActiveXObject                                                  
+ 在IE浏览器中，如果通过Ajax发生GET请求，那么IE浏览器认为同一个URL只有一个结果，导致没有办法获取服务器最新的数据
  - 保证每一次发送请求，url地址每次都不一样
    ```xmlhttp.open("GET", "get.php?t="+(new Date().getTime()), true);```                                                     
### (3) Ajax封装
```
function obj2str(obj) {
  obj.t = new Date().getTime();                                                  
  var res = [];
  for(var key in obj) {
      res.push(key+'='+obj[key]);
  }
  return res.join("&");
}
```                                                    
```
function ajax(url, obj, timeout, success, error) {
  var str = obj2str(obj);                                                  
  var xmlhttp, timer;
  if(window.XMLHttpRequest){
    xmlhttp = new XMLHttpRequest();                                              
  }else {
    xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");                                              
  }                                                
  xmlhttp.open("GET", url+"?"+str , true);
  xmlhttp.send();
  xmlhttp.onreadystatechange = function() {
    if(xmlhttp.readyState === 4){
      if(xmlhttp.status >= 200 && xmlhttp.status < 300 || xmlhttp.status === 304) {
        success(xmlhttp);
      }else{
        error(xmlhttp);                                       
      }                                                
    }
  }
  if(timeout) {
    timer = setInterval(function() {
      console.log("中断请求");                                                
      xmlhttp.abort();
      clearInterval(timer);                                                
    }, timeout);                                                    
  }                                                    
}
```                                                        
## 二、POST
### (1) 使用
```
xmlhttp.open("POST", "xxx.php", true);
xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xmlhttp.send("name1=value1&name2=value2");
```                                                      
### (2) 封装
  ```
  function obj2str(obj) {
    obj.t = new Date().getTime();                                                  
    var res = [];
    for(var key in obj) {
        res.push(encodeURIComponent(key)+'='+encodeURIComponent(obj[key]));
    }
    return res.join("&");
  }
  function ajax(type, url, obj, timeout, success, error) {
    var str = obj2str(obj);                                                  
    var xmlhttp, timer;
    if(window.XMLHttpRequest){
      xmlhttp = new XMLHttpRequest();                                              
    }else {
      xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");                                              
    }
    if(type.toLowerCase() === 'get') {
      xmlhttp.open("GET", url+"?"+str, true);
      xmlhttp.send();
    }else {
      xmlhttp.open("POST", url, true);
      xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
      xmlhttp.send(str);  
    }                                                
    xmlhttp.onreadystatechange = function() {
      if(xmlhttp.readyState === 4){
        clearInterval(timer);
        if(xmlhttp.status >= 200 && xmlhttp.status < 300 || xmlhttp.status === 304) {
          success(xmlhttp);
        }else{
          error(xmlhttp);                                       
        }                                                
      }
    }
    if(timeout) {
      timer = setInterval(function() {
        console.log("中断请求");
        xmlhttp.abort();
        clearInterval(timer);                                                
      }, timeout);                                                    
    }                                                    
  }
  ```                                                      
## 三、jQuery
### (1) jQuery封装                                                        
```
$.ajax({
  type: 'POST',
  url: 'get.php',
  data: 'name=John&location=Boston',
  success: function(msg) {
    alert(msg);
  },
  error: function(xhr) {
    alert(xhr.status);
  }
})
```         
### (2) XML
+ 可扩展标记语言                                                        
```
<?xml version="1.0" encoding="UTF-8" ?>
<dog>
  <name>张三</name>
  <age>33</age>
</dog>
```  
+ php获取文件内容
  ```
  <?php
    // 注意： 执行结果中有中文，必须在php文件顶部设置
    // 如果php中需要返回XML数据，也必须在php文件顶部设置
    header("content-type: text/xml; charset=utf-8");
    echo file_get_contents("1.xml");
  ?>
  ```
+ html处理数据
  ```
  var res = xhr.responseXML;
  console.log(res.querySelector("name").innerHTML);
  ```
### (3) JSON
+ JSON是JS对象的字符串表示法
+ JSON和JS对象相互转换
  - JS对象转换为JSON字符串
    * `JSON.stringify()`
  - JSON转换为JS对象
    * `JSON.parse()`
+ PHP获取文件内容 ```echo file_get_contents('json.txt');```
+ 兼容IE
  - 在低版本的IE中，不可以使用原生的JSON.parse方法
  - 可以使用json2.js这个框架来兼容
  
  
  
