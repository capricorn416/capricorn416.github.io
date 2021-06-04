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

Vue CLI2初始化项目：`vue init webpack 项目名称`

Vue CLI3初始化项目：`vue create 项目名称`

