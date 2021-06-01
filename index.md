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
  const cpnC1 = Vue.extend({
    template: `
      <div>
        <h2>我是子标题</h2>
        <p>我是子内容</p>
      </div>
    `
  })
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
