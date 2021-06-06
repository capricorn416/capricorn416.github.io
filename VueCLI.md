# VueCLI
使用Vue.js开发大型应用时，我们需要考虑代码目录结构、项目结构和部署、热加载、代码单元测试等事情

CLI是Command-Line Interface, 翻译为命令行界面, 俗称脚手架

[Vue CLI](https://cli.vuejs.org/zh/)是一个官方发布的 vue.js 项目脚手架

使用 vue-cli 可以快速搭建Vue开发环境以及对应的webpack配置

## 一、vue-cli的安装
Vue CLI使用前提 - Node

Vue CLI使用前提 - Webpack

安装Vue脚手架：`npm install -g @vue/cli`

*上面安装的是Vue CLI3的版本，如果需要按照Vue CLI2的方式初始化项目时是不可以的

拉取2.x模板（旧版本）：`npm install -g @vue/cli-init`

**Vue CLI2初始化项目：`vue init webpack 项目名称`**
![图片2.jpg](图片2.jpg)

**Vue CLI3初始化项目：`vue create 项目名称`**

## 二、vue-cli2
### 目录结构
![图片3.jpg](图片3.jpg)


安装CLI错误：删除`C:\Users\Administrator\AppData\Roaming\npm-cache`

`./config/index.js`中`useEslint`可以开关Eslint
### runtime+compiler和runtime-only区别
Vue程序运行过程

![图片4.jpg](图片4.jpg)

`runtime+compiler`:template -> ast -> render -> vdom -> UI

`runtime-only`：render -> vdom -> UI
*由vue-template-compiler处理.vue文件中的template

·性能更高

·代码量更少

**render函数**
```main.js
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
```
new Vue({
  el: '#app',
  render: function(createElement) {
    //1.传入组件对象：
    return createElenment(App)
  }
})
```
如果在之后的开发中，你依然使用template，就需要选择Runtime-Compiler

如果在之后的开发中，使用的是.vue文件夹开发，那么可以选择Runtime-only

**npm run build**
![图片5.jpg](图片5.jpg)

**npm run dev**
![图片6.jpg](图片6.jpg)

## 三、vue-cli3
·vue-cli 3 是基于 webpack 4 打造，vue-cli 2 还是 webapck 3

·vue-cli 3 的设计原则是“0配置”，移除的配置文件根目录下的build和config等目录

·vue-cli 3 提供了 vue ui 命令，提供了可视化配置，更加人性化

·移除了static文件夹，新增了public文件夹，并且index.html移动到public中

![图片7.jpg](图片7.jpg)

### 目录结构
![图片8.jpg](图片8.jpg)

自定义配置

添加`./src/vue.config.js`：
```
module.exports = {

}
```
*箭头函数中this的使用：
向外层作用域中，一层层查找this，直到有this的定义
```
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
# Vue-router
## 一、认识路由
路由就是通过互联的网络把信息从源地址传输到目的地址的活动

路由器提供了两种机制: 路由和转送

**路由**是决定数据包从来源到目的地的路径

**转送**将输入端的数据转移到合适的输出端

路由中有一个非常重要的概念叫路由表：路由表本质上就是一个**映射表**, 决定了数据包的指向
### 后端路由阶段

**后端渲染**：早期的网站开发整个HTML页面是由服务器来渲染的；服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示

**后端路由**：后端处理URL和页面之间的映射关系

一个页面有自己对应的网址, 也就是URL；

URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理；

Controller进行各种处理, 最终生成HTML或者数据, 返回给前端；

这就完成了一个IO操作

*当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端

*这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化

#### 后端路由的缺点:

·整个页面的模块由后端人员来编写和维护的

·前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码

·通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情

### 前后端分离阶段
**前后端分离**：后端只负责提供数据，不负责任何界面的内容

随着Ajax的出现, 有了前后端分离的开发模式

后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中

**优点：**

前后端责任清晰, 后端专注于数据上, 前端专注于交互和可视化上；

并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可

### 前端路由阶段
**前端渲染**：浏览器中显示的网页中的大部分内容，都是由前端写的js代码
