# Canvas
## 一、canvas介绍
+ HTML5的canvas元素使用JavaScript在网页上绘制图像
+ 画布是一个矩形区域，您可以控制其每一像素
+ canvas 拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法
```
<canvas id="canvas" width="300" height="150">
  你的浏览器太low了，请更新
</canvas>
```
```
var canvas = document.getElementById('canvas');
if (canvas.getContext){
  var ctx = canvas.getContext('2d');
}else{
  elert('你的浏览器太low了，请更新');
}
```
## 二、基础模板
```
function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext){
    var ctx = canvas.getContext('2d');
  }
}
```
## 三、简单例子
### （一）矩形
```
// 填充矩形
ctx.fillStyle = 'rgb()';
ctx.fillRect(x, y, width, height);
```
+ `Style`必须写在`Rect`之前
+ 画布元素的左上角是原点

```
// 描边矩形
ctx.strokeStyle = 'rgb()';
ctx.strokeRect(x, y, width, height);
```
```
// 清除矩形
ctx.clearRect(x, y, width, height);
```
### （二）绘制路径
+ 创建起始点
+ 使用画图命令画出路径
+ 把路径闭合
+ 通过描边或填充绘制图形
#### 1. 三角形
