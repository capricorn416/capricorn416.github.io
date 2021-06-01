# Vue
## 基础语法
```html
<div id='app'>
  {{ message }} 
</div>
```
```javascript
<script>
  //编程范式：声明式编程
  var app = new Vue({
    el:'#app',  //用于挂载要管理的元素
    data: { //定义数据
      message: 'Hello World'
    }
  })
</script>
```
## 组件化
### 一、组件的使用步骤
1.创建组件构造器
2.注册组件
3.使用组件
