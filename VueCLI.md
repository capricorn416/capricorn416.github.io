# VueCLI
+ 使用Vue.js开发大型应用时，我们需要考虑代码目录结构、项目结构和部署、热加载、代码单元测试等事情

+ CLI是Command-Line Interface, 翻译为命令行界面, 俗称脚手架

+ [Vue CLI](https://cli.vuejs.org/zh/)是一个官方发布的 vue.js 项目脚手架

+ 使用 vue-cli 可以快速搭建Vue开发环境以及对应的webpack配置

## 一、vue-cli的安装
+ Vue CLI使用前提 ：
  + Node
  + Webpack

+ 安装Vue脚手架：`npm install -g @vue/cli`

  - *上面安装的是**Vue CLI3**的版本，如果需要按照Vue CLI2的方式初始化项目时是不可以的

  + 拉取2.x模板（旧版本）：`npm install -g @vue/cli-init`

+ **Vue CLI2初始化项目：`vue init webpack 项目名称`**
  ![图片2.jpg](图片2.jpg)

+ **Vue CLI3初始化项目：`vue create 项目名称`**

## 二、vue-cli2
### 1. 目录结构
![图片3.jpg](图片3.jpg)

+ 安装CLI错误：删除`C:\Users\Administrator\AppData\Roaming\npm-cache`

+ `./config/index.js`中`useEslint`可以开关Eslint

### 2. runtime+compiler和runtime-only区别
Vue程序运行过程

![图片4.jpg](图片4.jpg)

+ `runtime+compiler`：template -> ast -> render -> vdom -> UI

+ `runtime-only`：render -> vdom -> UI
  - 由vue-template-compiler处理.vue文件中的template
    + 性能更高
    + 代码量更少

#### **render函数**

+ 关于不同版本的vue
  + vue.js与vue.runtime.xxx.js的区别
    - vue.js是完整版的Vue，包含：核心功能+模板解析器
    - vue.runtime.xxx.js是运行版的Vue，只包含：核心功能，没有模板解析器
  + 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用render函数接收到的createElement函数去指定具体内容

```js
new Vue({
  el: '#app',
  render: function(createElement) {
    //1.普通用法：createElenment('标签',{标签的属性(可以不传)},['内容数组'])
    return createElenment('h2',
      {class: 'box'},
      ['Hello World', createElenment('button',['按钮'])])
  }
})
```
```js
new Vue({
  el: '#app',
  render: function(createElement) {
    //1.传入组件对象：
    return createElenment(App)
  }
})
```
+ 如果在之后的开发中，你依然使用template，就需要选择Runtime-Compiler

+ 如果在之后的开发中，使用的是.vue文件夹开发，那么可以选择Runtime-only

**npm run build**
![图片5.jpg](图片5.jpg)

**npm run dev**
![图片6.jpg](图片6.jpg)

## 三、vue-cli3
+ vue-cli 3 是基于 webpack 4 打造，vue-cli 2 还是 webapck 3

+ vue-cli 3 的设计原则是“0配置”，移除的配置文件根目录下的build和config等目录

+ vue-cli 3 提供了 vue ui 命令，提供了可视化配置，更加人性化

+ 移除了static文件夹，新增了public文件夹，并且index.html移动到public中

![图片7.jpg](图片7.jpg)

### 1. 目录结构
![图片8.jpg](图片8.jpg)

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\B8A75E08AF0AB0A6F87CB5046E93CA86.png)

+ /src/main.js

  ```js
  /*
    该文件是整个项目的入口文件
  */
  // 引入Vue
  import Vue from 'vue'
  // 引入App组件，它是所有组件的父组件
  import App from './App.vue'
  // 关闭vue的生产提示
  Vue.config.productionTip = false
  
  // 创建Vue实例对象——vm
  new Vue({
    // 将App组件放入容器中
    render: h => h(App),
  }).$mount('#app')
  ```

+ /public/index.html

  ![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\042648C8901AFAA8018FEAEBBDF983D6.png)

### 2. 自定义配置

+ 使用`vue inspect > output.js`可以查看到Vue脚手架的默认配置

+ 使用`vue.config.js`可以对脚手架进行个性化配置

  ```js
  module.exports = {
    lintOnSave: false	// 关闭语法检查
  }
  ```

### *箭头函数中this的使用

+ 向外层作用域中，一层层查找this，直到有this的定义

```js
const obj = {
  aaa() {
    setTimeout(function() {
      setTimeout(function() {
        console.log(this);  //window
      })
      
      setTimeout(() => {
        console.log(this);  //window
      })
    })
    
    setTimeout(() => {
      setTimeout(function() {
        console.log(this);  //window
      })
      
      setTimeout(() => {
        console.log(this);  //obj
      })
    })
  }
}
```

### 3. mixin（混入）

+ 功能：可以把多个组件共用的配置提取成一个混入对象

+ 使用方式：

  - 定义混合

    mixin.js

    ```js
    export const hunhe = {
      methods: {},
      mounted() {}
    }
    ```

  - 使用混入

    - 局部混合

      .vue

      ```js
      import {hunhe} from '../mixin'
      export default {
        mixins: [hunhe]
      }
      ```

    - 全局混合

      main.js

      ```js
      import {hunhe} from '../mixin'
      Vue.mixin(hunhe)
      ```


### 4. 插件

+ 功能：用于增强Vue

+ 本质：包含install方法的一个对象，install的第一个参数就是Vue，第二个以后的参数是插件使用者传递的数据

+ 定义插件：`对象.install = function(Vue, options) {}`

  + plugin.js

    ```js
    export default {
      install(Vue, options) {
        // 1.添加全局过滤器
        Vue.filter()
        // 2.添加全局命令
        Vue.directive()
        // 3.配置全局混入
        Vue.mixin()
        // 4.添加实例方法
        Vue.prototype.$myMethod = function() {}
        Vue.prototype.$myProperty = xxx
      }
    }
    ```

+ 使用插件：`Vue.use()`

  + main.js

  ```js
  import plugins from './plugin'
  Vue.use(plugins)
  ```

### 5. scoped样式

+ 作用：让样式在局部生效，防止冲突
+ 写法：`<style scoped>`

### 6. $nextTick

+ 语法：`this.$nextTick(回调函数)`
+ 作用：在下一次DOM更新结束后执行其指定的回调
+ 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行

### 7. 动画和过渡

+ 作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名

+ 写法：

  - 准备好样式：

    - 元素进入的样式：
      - v-enter：进入的起点
      - v-enter-active：进入过程中
      - v-enter-to：进入的终点
    - 元素离开的样式：
      - v-leave：离开的起点
      - v-leave-active：离开过程中
      - v-leave-to：离开的终点

  - 使用`<transition>`包裹要过渡的元素，并配置name属性（可选）

    ```html
    <transition name="hello">
      <h1 v-show="isShow">你好啊！</h1>
    </transition>  
    ```

  - 备注：若有多个元素需要过渡，则需要使用：`<transition-group>`，且每个元素都要指定`key`值

### 8. 配置代理

+ 方法一：

  在vue.config.js中添加如下配置

  ```js
  devServer: {
    proxy: "http://localhost:5000"
  }
  ```

  + 说明：
    - 优点：配置简单，请求资源时直接发给前端（8080）即可
    - 缺点：不能配置多个代理，不能灵活地控制请求是否走代理
    - 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源）

+ 方法二：

  编写vue.config.js配置具体代理规则

  ```js
  module.exports = {
    devServer: {
      proxy: {
        '/api': {	// 匹配所有以'/api'开头的请求路径
          target: 'http://localhost:5000',	// 代理目标的基础路径
          pathRewrite: {'^/api': ''},
          // ws: true,	// 用于支持websocket
          // changeOrigin: true	// 用于控制请求头中的host值
        }
      }
    }
  }
  /*
    changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
    changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
    changeOrigin默认值为true
  */
  ```

  + 说明：
    + 优点：可以配置多个代理，且可以灵活地控制请求是否走代理
    + 缺点：配置略微繁琐，请求资源时必须加前缀

# Vue-router

+ vue-router：vue的一个插件库，专门用来实现SPA应用

## 一、认识路由
+ 路由就是通过互联的网络把信息从源地址传输到目的地址的活动

+ 路由器提供了两种机制: 路由和转送
  - **路由**是决定数据包从来源到目的地的路径
  - **转送**将输入端的数据转移到合适的输出端

+ 路由中有一个非常重要的概念叫路由表：路由表本质上就是一个**映射表**, 决定了数据包的指向

### 1. 后端路由阶段

+ **后端渲染**：早期的网站开发整个HTML页面是由服务器来渲染的；服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示

+ **后端路由**：后端处理URL和页面之间的映射关系

  - 一个页面有自己对应的网址, 也就是URL；

  - URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理；

  - Controller进行各种处理, 最终生成HTML或者数据, 返回给前端；

  - 这就完成了一个IO操作

+ 当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端

+ 这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化

#### 后端路由的缺点:

+ 整个页面的模块由后端人员来编写和维护的

+ 前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码

+ 通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情

### 2. 前后端分离阶段
+ **前后端分离**：后端只负责提供数据，不负责任何界面的内容
  + 随着Ajax的出现, 有了前后端分离的开发模式
  + 后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中

+ **优点：**
  + 前后端责任清晰, 后端专注于数据上, 前端专注于交互和可视化上；
  + 并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可

+ **前端渲染**：浏览器中显示的网页中的大部分内容，都是由前端写的js代码在浏览器中执行，最终渲染出来的网页

### 3. 单页面富应用阶段
+ 其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由，也就是前端来维护一套路由规则

+ SPA：单页面富应用。整个网页只有一个html页面

+ **前端路由**的核心：改变URL，但是页面不进行整体的刷新

## 二、前端路由
### 1. URL的hash
+ URL的hash也就是锚点（#），本质上是改变window.location的href属性

+ 可以通过直接赋值location.hash来改变href，但是页面不发生刷新
  + 在控制台输入`location.hash = 'aaa'`

### 2. HTML5的history模式

#### pushState
+ 在控制台输入`history.pushState({},'','home')`

+ *类似栈，可以返回*

#### replaceState
+ 在控制台输入`history.replaceState({},'','home')`

+ *不能返回*

#### go
+ 在控制台输入

  ```js
  history.pushState({},'','home')
  history.pushState({},'','about')
  history.pushState({},'','me')
  history.pushState({},'','test')
  history.go(-1)
  ```

+ *`history.go(-1)`等价于`history.back()`*

+ *`history.go(1)`等价于`history.forward()`*

+ *这三个接口等同于浏览器界面的前进后退*

## 三、Vue-router
+ [vue-router](https://router.vuejs.org/zh/)是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用
  + vue-router是基于路由和组件的

+ 路由用于设定访问路径, 将路径和组件映射起来

+ 在vue-router的单页面应用中, 页面的路径的改变就是组件的切换

### 1. vue-router的安装和配置
+ 安装vue-router：`npm install vue-router --save`

+ 在模块化工程中使用它（因为是一个插件, 所以可以通过Vue.use()来安装路由功能）：

  ①导入路由对象，并且调用Vue.use(VueRouter)

  ②创建路由实例，并且传入路由映射配置

  ③在Vue实例中挂载创建的路由实例

```js
// src/router/index.js
// 该文件专门用于创建整个应用的路由器
//配置路由相关的信息
import VueRouter from 'vue-router'
import Vue from 'vue'

//1.通过Vue.use(插件)，安装插件
Vue.use(VueRouter)

//2.创建VueRouter对象
const routes = []

// 创建一个路由器
const router = new VueRouter({
  // 配置路由和组件之间的应用关系
  routes
})

//3.将router对象传入到vue实例
export default router
```
```js
// main.js
import router from './router'

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

### 2. vue-router的使用
①创建路由组件

②配置路由映射: 组件和路径映射关系
```js
import Home from '../components/Home'
import About from '../components/About'

const router = [
  {
    path: '/home',
    component: Home
  },
  {
    path: '/about',
    component: About
  }
]
```
③使用路由: 通过<router-link>和<router-view>
```vue
<template>
  <div id='app'>
    <router-link active-class="active" to='/home'>首页</router-link>
    <router-link active-class="active" to='/about'>关于</router-link>
    <!--指定组件的呈现位置-->
    <router-view></router-view>
  </div>
</template>
```
+ `<router-link>`: 该标签是一个vue-router中已经内置的组件, 它会被渲染成一个<a>标签

+ `<router-view>`: 该标签会根据当前的路径, 动态渲染出不同的组件 
  + 相当于一个占位的东西
  + 网页的其他内容, 比如顶部的标题/导航, 或者底部的一些版权信息等会和<router-view>处于同一个等级
  + 在路由切换时, 切换的是<router-view>挂载的组件, 其他内容不会发生改变

### *几个注意点

+ 路由组件通常存放在`pages`文件夹，一般组件通常存放在`components`文件夹
+ 通过切换，“隐藏”了的路由组件，默认是被销毁的，需要的时候再去挂载
+ 每个组件都有自己的`$route`属性，里面存储着自己的路由信息
+ 整个应用只有一个router，可以通过组件的`$router`属性获取到

### 3. 路由的默认值
+ 让路径默认跳到到首页, 并且<router-view>渲染首页组件

  ```js
  const routes = [
    {
      path: '',
      //重定向
      redirect: '/home'
    }
  ]
  ```

+ *改变路径的方式有两种：URL的hash和HTML5的history*

  + 默认情况下, 路径的改变使用的URL的hash => localhost:8081/#/home

  + *如果希望使用HTML5的history模式，配置:

    ```js
    const router = new VueRouter({
      routes,
      mode: 'history'
    })
    ```

### 4. router-link的属性补充
+ `tag`：tag可以指定<router-link>之后渲染成什么组件, 而不是默认的<a>标签

  ```vue
  <router-link tag='button'></router-link>
  ```

+ `replace`：浏览器的历史记录有两种写入模式，分别为push和replace，push是追加历史记录，replace是替换当前记录；路由跳转时候默认为push；

  replace不会留下history记录, 所以指定replace的情况下, 不能返回到上一个页面中

  ```vue
  <router-link replace></router-link>
  ```

+ `active-class`：当<router-link>对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称

  ```vue
  <router-link active-class='active'></router-link>
  ```

  + 批量改

    ```js
    const router = new VueRouter({
      linkActiveClass: 'active'  
    })
    ```

### 5. 编程式路由导航

+ push：*可以返回

  `this.$router.push('/home')`

+ replace：*不能返回

  `this.$router.replace('/home')`

+ back：后退

  `this.$router.back()`

+ forward：前进

  `this.$router.forward()`

+ go：前进（正数）/后退（负数）

  `this.$router.go(2)`

#### 动态路由
```js
import User from '../components/User'
const routes = [
  {
    path: '/user/:userId',
    component: User
  }  
]
```
```vue
<router-link to='/user/zhangsan'></router-link>
```
```vue
<router-link :to="'/user/'+userId"></router-link>

export default {
  name: 'App'
  data() {
    return{
      userId: 'lisi'
    }
  }
}
```
```vue
<h2>{{ userId }}</h2>
<h2>{{ $route.params.userId }}</h2>
  
  export default {
    name: 'User',
    computed: {
      userId() {
        return this.$route.params.userId
      }
    }
  }
```
### 6. 路由的懒加载
+ 路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块

  只有在这个路由被访问到的时候, 才加载对应的组件
  ![图片9.jpg](图片9.jpg)

  + *app： 当前应用程序开发的所有代码（业务代码）

  + *manifest：为打包的代码做底层支撑

  + *vendor：提供商，第三方/vue/vue-router/axios/bs

#### 路由懒加载的方式
①结合Vue的异步组件和Webpack的代码分析
```js
const Home = resolve => { require.ensure(['../components/Home.vue'], () => { resolve(require('../components/Home.vue')) })};
```
②AMD写法
```js
const About = resolve => require(['../components/About.vue'], resolve);
```
③在ES6中, 我们可以有更加简单的写法来组织Vue异步组件和Webpack的代码分割
```js
const Home = () => import('../components/Home.vue')
```
### 7. 嵌套路由
①创建对应的子组件, 并且在路由映射中配置对应的子路由
```js
  {
    path: '/home',
    component: Home,
    children: [
      {
        path: '',
        redirect: 'news'
      },
      {
        path: 'news',
        component: Homenews
      },
      {
        path: 'message',
        component: HomeMessage
      }
    ]
  }
```
②在组件内部使用<router-view>标签
```vue
<router-link to='/home/news'></router-link>
<router-link to='/home/message'></router-link>
<router-view></router-view>
```

### 8. 传递参数
+ 传递参数主要有两种类型: params和query

#### ①params
+ 配置路由格式： `/router/:id`
+ 传递的方式：在path后面跟上对应的值
+ 传递后形成的路径：/router/123

```js
{
  path: 'home',
  children: [
    {
      path: 'detail/:id/:title'	// 使用占位符声明接收params参数
    }
  ]
}
```

```vue
<!--to的字符串写法-->
<router-link to="home/detail/666/你好啊"></router-link>
```

```vue
<!--to的对象写法-->
<!--只能用name，不能用path-->
<router-link :to='{name: 'xiangqing', params: {name: 'why', age: 18, height: 1.88}}'></router-link> 
```

+ 获取参数：`$route.params.id`

#### ②query

+ 配置路由格式：`/router`，也就是普通配置
+ 传递的方式：对象中使用query的key作为传递方式
+ 传递后形成的路径：/router?id=123

```vue
<!--to的字符串写法-->
<router-link to="/home/detail?id=666&title=你好啊！"></router-link>
```

```vue
<router-link :to="`/home/detail?id=${}&title=${}`"></router-link>
```

```vue
<!--to的对象写法-->
<!--name和path都能用-->
<router-link :to='{path: '/profile', query: {name: 'why', age: 18, height: 1.88}}'></router-link> 
```

```js
this.$router.push('/user/' + this.userId)
this.$router.push({
  path: '/profile',
  query: {
    name: 'kobe',
    age: 19,
    height: 1.87
  }
})
```

+ 获取参数通过`$route`对象获取

```vue
<h2>{{ $route.query.name }}</h2>
```

### *$route和$router的区别
+ $router为VueRouter实例，想要导航到不同URL，则使用$router.push方法

+ $route为当前router跳转对象，里面可以获取name、path、query、params等 
+ *所有的组件都继承自Vue类的原型

### 9. props配置

+ 第一种写法：

  值为对象，该对象中的所有key-value都会以props的形式传给Detail组件

  ```js
  {
    path: 'detail/:id/:title',
    component: Detail,
    props: {a: 1, b: 'hello'}
  }
  ```

+ 第二种写法：

  值为布尔值，若布尔值为真，就会把该路由组件收到的所有**params**参数，以props的形式传给Detail组件

  ```js
  {
    path: 'detail/:id/:title',
    component: Detail,
    props: true
  }
  ```

+ 第三种写法：

  值为函数

  ```js
  {
    path: 'detail',
    component: Detail,
    props($route) {
      return {id: $route.query.id, title: $route.query.title, a:1, b: 'hello'}
    }
    // 解构赋值
    /*
    prop({query}) {
      return {id: query.id, title: query.title}
    }
    */
    // 连续解构赋值
    /*
    prop({query: {id, title}}) {
      return {id, title}
    }
    */
  }
  ```

### 10. 缓存路由组件

+ `keep-alive` 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染（避免组件被频繁创建、销毁）

  ```
  <keep-alive><router-view/></keep-alive>
  ```

#### keep-alive的属性

+ `include`：字符串或正则表达，只有匹配的组件会被缓存

+ `exclude`：字符串或正则表达式，任何匹配的组件都不会被缓存

+ 引号里的是组件名

  ```vue
  <keep-alive exclude='Profile,User'> <!--逗号后面不能加空格-->
  <!--<keep-alive :exclude="['Profile','User']">-->
    <router-view/>
  </keep-alive>
  ```

#### 两个新的生命周期钩子

+ 作用：路由组件所独有的两个钩子，用于捕获路由组件的激活状态

+ `activated`：路由组件被激活时触发

+ `deactivated`：路由组件失活时触发

  + activated和deactivated只有该组件被使用了keep-alive时，才是有效的

  ```js
  activated(){
    this.$router.push(this.path);
  }
  beforeRouteLeave(to, from, next) {
    this.path = this.$route.path;
    next()
  }
  ```

### 11. 导航守卫

+ vue-router提供的**导航守卫**主要用来监听路由的进入和离开，对路由进行权限控制

+ vue-router提供了`beforeEach`和`afterEach`的钩子函数, 它们会在路由即将改变前和改变后触发

#### 生命周期函数

+ `created()`：组件创建出来时回调

  ```js
  created() {
    document.title = '用户'  
  }  
  ```

+ `mounted()`：组件挂载到DOM上时回调

+ `updated()`：界面更新时回调

+ `destroy()`：组件销毁时回调

#### 全局导航守卫
+ 前置守卫(guard)

  ```js
  const routes = [
    {
      path: '/home',
      component: Home,
      meta: {	// 路由元信息
        title: '首页'
      }
    }  
  ]
  // 全局前置路由守卫——初始化的时候被调用、每次路由切换之前被调用  
  router.beforeEach((to, from, next) => {
    // console.log(to, from)
    document.title = to.meta.title
    next()  
    // next({name: ''})
  })
  ```

  + to：即将要进入的目标的路由对象
  + from：当前导航即将要离开的路由对象
  + next：必须调用该方法后, 才能进入下一个钩子

+ 后置钩子(hook)

  ```js
  // 全局后置路由守卫——初始化的时候被调用、每次路由切换之后被调用
  router.afterEach((to, from) => {})
  ```

#### [其他守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E8%B7%AF%E7%94%B1%E7%8B%AC%E4%BA%AB%E7%9A%84%E5%AE%88%E5%8D%AB)
+ 路由独享的守卫

+ 组件内的守卫

### 12. 路径别名
+ **vue-cli2**的`webpack.base.conf.js`中：

  ```js
    resolve: {
      extensions: ['.js','.vue','.json'],
      alias: {
        '@': resolve('src'),
        'assets': resolve('@/assets'),
        'components': resolve('@/components'),
        'views': resolve('@/views')
      }
    }
  ```

+ 在**vue-cli3**中新建`vue.config.js`：

  ```js
  module.exports = {
      configureWebpack: {
          resolve: {
              alias: {
                  'assets': '@/assets',
                  'common': '@/common',
                  'components': '@/components',
                  'network': '@/network',
                  'views': '@/views',
              }
          }
      }
  }
  ```

+ 在**html**中使用时要加`~`

  ```vue
  <img src='~assets/img/tabbar/home.svg'>
  ```

# [Promise](Promise.md)
