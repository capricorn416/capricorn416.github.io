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
+ 创建起始路径
+ 使用画图命令画出路径
+ 把路径闭合
+ 通过描边或填充绘制图形
#### 1. 三角形
```
ctx.beginPath();
// 起始点
ctx.moveTo(x1, y1);
// 二点
ctx.lineTo(x2, y2);
// 三点
ctx.lineTo(x3, y3);

// 填充三角形
// ctx.fill();

// 描边三角形
// ctx.lineTo(x1, y1);
// ctx.stroke();

// 描边三角形
ctx.closePath();
ctx.stroke();
```

#### 2. 圆形
```
ctx.strokeStyle = 'orange';
ctx.lineWidth = 10;

cxt.beginPath();
ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);

ctx.stroke();
```
+ 开始角度和结束角度
  - 0是三点钟方向
  - 用 Math.PI 的倍数表示
  - 代表从三点钟方向顺时针转过取得的角度
+ anticlockwise是boolean值，默认为 false（顺时针）

### （三）渐变
#### 1. 线性渐变
```
// 参数1：起点x1； 参数2：起点x2； 参数3：终点x2； 参数4：终点y2
var lingrad = ctx.createLinearGradient(x1,y1,x2,y2);

// 参数1：0.0~1.0之间的数值，表示颜色所在的相对位置； 参数2：颜色
lingrad.addColorStop(0, '#cc6677');
lingrad.addColorStop(0.5, '#fff');
lingrad.addColorStop(0.5, '#c6c776');
lingrad.addColorStop(1, '#fff');

ctx.fillStyle = lingrad;
ctx.fillRect(10,10,130,130);
```
