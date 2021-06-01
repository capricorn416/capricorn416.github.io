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
### 一、组件化的基本使用
#### 1.创建组件构造器
#### 2.注册组件
#### 3.使用组件
```html
<div id='app'>
  <!--3.使用组件-->
  <mycpn></mycpn>
  <mycpn></mycpn>
  <mycpn></mycpn>
</div>
```
```javascript
<script>
  //1.创建组件构造器对象
  const cpnC = Vue.extend({
    template: `
      <div>
        <h2>我是标题</h2>
        <p>我是正文</p>
      </div>`
  })
  //2.注册组件
  Vue.component('mycpn',cpnC)
  
  const app = new Vue({
    el: '#app',
    data : {
      message: ''
    }
  })
</script>
```
