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
