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
①创建组件构造器

②注册组件

③使用组件
```html
<div id='app'>
  <!--3.使用组件-->
  <mycpn></mycpn>
  <mycpn></mycpn>
  <div>
    <mycpn></mycpn>
  </div>
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
  
  //2.注册组件(全局组件)
  Vue.component('mycpn',cpnC)
  
  const app = new Vue({
    el: '#app',
    data : {
      message: ''
    }
  })
</script>
```
### 二、全局组件和局部组件
#### 1.全局组件

上面方法注册的是全局组件

全局组件意味着可以在任意Vue的实例下面使用

#### 2.局部组件
如果我们注册的组件是挂载在某个实例中，那么就是一个局部组件
```javascript
<script>
  const cpnC = Vue.extend({
      template: `
        <div>
          <h2>我是标题</h2>
          <p>我是正文</p>
        </div>`
    })
  const app = new Vue({
    el: '#app',
    data :{
      message: ''
    },
    components: {
      mycpn: cpnC
    }
  })
</script>
```
### 三、父组件和子组件
```javascript
<script>
  //子组件
  const cpnC1 = Vue.extend({
    template: `
      <div>
        <h2>我是子标题</h2>
        <p>我是子内容</p>
      </div>
    `
  })
  
  //父组件
  const cpnC2 = Vue.extend({
    template: `
      <div>
        <h2>我是父标题</h2>
        <p>我是父内容</p>
        <cpn1></cpn1>
      </div>
    `,
    components: {
      cpn1: cpnC1
    }
  })
  
  // root组件
  const app = new Vue({
    el: '#app',
    data : {
      message: ''
    }，
    components: {
      cpn2: cpnC2
    }
  })
</script>
```
父子组件错误用法：以子标签的形式在Vue实例中使用

上述代码中cpn1是无法直接在html中使用的

#### 注册组件语法糖
```javascript
<script>
  //注册全局组件
  Vue.component('mycpn',{
    template: `
      <div>
        <h2></h2>
        <p><p>
      </div>
    `
  })
</script>
```
```javascript
<script>
  //注册局部组件
  var app = new Vue({
    el: '#app',
    components: {
      'mycpn': {
        template: `
          <div>
            <h2></h2>
            <p><p>
          </div>
        `
      }
    }
  })
</script>
```

#### 组件模板分离
```
<template id='mycpn'>
  <div>
    <h2>我是标题</h2>
    <p>我是正文</p>
  </div>`
</template>
```
```javascript
<script>
  //注册全局组件
  Vue.component('mycpn',{
    template: '#mycpn'
  })
</script>
```
```javascript
<script>
  //注册局部组件
  var app = new Vue({
    el: '#app',
    components: {
      'mycpn': {
        template: '#mycpn'
      }
    }
  })
</script>
```
#### 组件中的数据存放问题
组件是一个单独功能模块的封装，组件中不能直接访问Vue实例中的data，组件有自己保存数据的地方

组件对象也有一个data属性，**这个data属性必须是个函数**，而且这个函数返回一个对象，对象内部保存着数据
```javascript
<script>
  //注册全局组件
  Vue.component('mycpn',{
    template: '#mycpn',
    data() {
      return {
        title: 'abc'
      }
    }
  })
</script>
```
#### 父子组件通信
##### 1.父组件通过props向子组件传递数据


##### 2.通过事件向父组件发送消息


