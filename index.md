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
1. 创建组件构造器
2. 注册组件
3. 使用组件
```javascript
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
```
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
### 二、全局组件和局部组件
#### 1.全局组件
+ 上面方法注册的是全局组件
+ 全局组件意味着可以在任意Vue的实例下面使用

#### 2.局部组件
如果我们注册的组件是挂载在某个实例中，那么就是一个局部组件
```javascript
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
+ 父子组件错误用法：以子标签的形式在Vue实例中使用
  - 上述代码中cpn1是无法直接在html中使用的

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
![image](https://img.imgdb.cn/item/601508053ffa7d37b3ce7091.png)
##### 1.父组件通过`props`向子组件传递数据
```
<div id='app'>
  <cpn v-bind:cmovies='movies' :cmessage='message'></cpn>
</div>

<template id='cpn'>
  <div>
    <ul>
      <li v-for='item in cmovies'>{{ item }}</li>
    </ul>
    <h2>{{ cmessage }}</h2>
  </div>
</template>
```
props的值有两种方式：
+ **字符串数组**，数组中的字符串就是传递时的名称
+ **对象**，对象可以设置传递时的类型，也可以设置默认值等
```javascript
<script>
  const cpn = {
    template: '#cpn'，
    //props: ['cmovies','cmessage']
    props: {
      //类型限制
      cmovies: Array,
      //提供默认值
      cmessage: {
        type: String,
        default: '',
        required: true  //必传
      }
    }
  }
  
  const app = new Vue({
    el: '#app',
    data : {
      message: '',
      movies: ['海贼王','海尔兄弟','海的女儿']
    },
    components: {
      cpn //属性增强写法
    }
  })
</script>
```
+ 类型是对象或者数组时，默认值必须是一个函数
+ 当有自定义构造函数时，验证也支持自定义的类型
```javascript
<script>
  function Person(firstname, lastname) {
    this.firstname = firstname
    this.lastname = lastname
  }
  
  const cpn = {
    template: '#cpn'，
    props: {
      //多个可能的类型
      propA: [String, Number],
      //对象或数组默认值必须从工厂函数获取
      propB: {
        type: Array,
        default() {
          return []
        }
      },
      //自定义类型
      propC: {
        author: Person
      },
      //自定义验证函数
      PropD: {
        validator: function(value){
          //这个值必须匹配下列字符串中的一个
          return ['success','warning','danger'].indexof(value) !== -1
        }
      }
    }
  }
</script>
```
* 组件模板必须包含一个根元素(比如用外层div包起来)
* html不支持驼峰命名（不分大小写），可以转换成短线连接

##### 2.子组件通过`$emit()`事件向父组件发送消息
+ v-on不仅可以用于监听DOM事件，也可以用于组件间的自定义事件
  - 自定义事件的流程：
    * 在子组件内，通过`$emit()`来触发事件
    * 在父组件内，通过`v-on`来监听子组件事件
```html
<!--父组件模板-->
<div id='app'>
  <cpn @itemclick='cpnClick'></cpn>
</div>
<!--子组件模板-->
<template id='cpn'>
  <div>
    <button v-for='item in categories' @click='btnclick(item)'>{{ item.name }}</button>
  </div>
</template>
```
```javascript
<script>
  //子组件
  const cpn = {
    template: '#cpn'，
    data() {
      return {
        categories: [
         {id: '01', name: '热门推荐'},
         {id: '02', name: '手机数码'},
         {id: '03', name: '家用家电'}
        ]
      }
    },
    methods: {
      btnclick(item) {
        this.$emit('itemclick',item)
      }
    }
  }
  //父组件
  const app = new Vue({
    el: '#app',
    components: {
      cpn //属性增强写法
    },
    methods: {
      cpnClick(item) {
        console.log(item)
      }
    }
  })
</script>
```
##### 结合双向绑定
```
<div id='app'>
  <cpn :number='num' @numchange='numchange'/>
</div>

<template id='cpn'>
  <div>
    <h2>props:{{ number }}</h2>
    <h2>data: {{ dnumber }}</h2>
    <!--<input type='text' v-model='dnumber'>-->
    <input type='text' :value='dnumber' @input='numInput'>
  </div>
</template>
```

```
<script>
  const app = new Vue({
    el:'#app',
    data: {
      num: 1
    },
    methods: {
      numchange(value) {
        this.num = parseInt(value)
      }
    },
    components: {
      cpn: {
        template: '#cpn',
        props: {
          number: Number
        },
        data() {
          return {
            dnumber: this.number
          }
        },
        methods: {
          numInput(event){
            //将input中的value赋值到dnumber中
            this.dnumber = event.target.value;
            //为了让父组件可以修改值，发出一个事件
            this.$emit('numchange',this.dnumber)
          }
        }
      }
    }
  })
</script>
```
--
`watch`实现
```
  components: {
    cpn: {
      template: '#cpn',
      props: {
        name: ''
      },
      data() {
        return {}
      },
      watch: {
        name(newValue, oldValue) {
        }
      }
    }
  }
```
#### 父子组件的访问方式
##### 1.父组件访问子组件：使用`$children`或`$refs`
①`$children` => 数组类型，它包含所有子组件对象，访问其中的子组件必须通过索引值

②`$refs` => 对象类型，默认是一个空的对象，必须给特定的子组件绑定ref=''
```
<div id='app'>
  <cpn></cpn>
  <cpn></cpn>
  <cpn ref='a'></cpn>
  <button @click='btnClick'>按钮</button>
</div>
```
```
<script>
  const app = new Vue({
    el: '#app',
    data: {},
    methods: {
      btnClick() {
        //1.$children
        console.log(this.$children);
        for (let c of this.$children) {
          console.log(c.name);
          c.showMessage()
        }
        
        //2.$refs
        console.log(this.$refs.a.name)
      }
    }/
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            name:''
          }
        }
        methods: {
          showMessage() {
            console.log('showMessage');
          }
        }
      }
    }
  })
</script>
```
##### 2.子组件访问父组件：使用`$parent`

##### 访问根组件：`$root`
```
<template id='cpn'>
  <div>
    <ccpn></ccpn>
  </div>
</template>

<template id='ccpn'>
  <div>
    <button @click='btnClick'>按钮</button>
  </div>
</template>
```
```
<script>
  const app = new Vue({
    el: '#app',
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            name: ''
          }
        },
        components: {
          ccpn: {
            template: '#ccpn',
            methods: {
              btnClick() {
                //1.访问父组件
                console.log(this.$parent);
                console.log(this.$parent.name);
                //2.访问根组件
                console.log(this.$root)
              }
            }
          }
        }
      }
    }
  })
</script>
```
### 四、slot-插槽
+ 组件的插槽是为了让我们封装的组件更加具有扩展性
+ 最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽
+ 一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容
#### 插槽的基本使用
```
<div id='app'>
  <cpn><button>按钮</button></cpn>
  <cpn><span>呵呵</span></cpn>
</div>
```
```
<template id='cpn'>
  <div>
    <h2></h2>
    <p></p>
    <slot></slot>
  </div>
</template>
```
+ 插槽的默认值：`<slot>`中的内容表示，如果没有在该组件中插入任何其他内容，就默认显示该内容

`<slot><button>按钮</button></slot>`

+ 如果有多个值同时放入到组件进行替换时，一起作为替换元素
```
<div id='app'>
  <cpn>
    <i>我是替换插槽的内容</i>
    <p>我也是替换插槽的内容</p>
  </cpn>
</div>
```

#### 具名插槽
+ 给`slot`元素一个`name`属性即可
```
<div id='app'>
  <cpn><span slot='center'>标题</span></cpn>
  <cpn><button slot='left>标题</button></cpn>
</div>
```
```
<template id='cpn'>
  <div>
    <slot name='left'><span>左边</span></slot>
    <slot name='center'><span>中间</span></slot>
    <slot name='right'><span>右边</span></slot>
  </div>
</template>
```
#### 编译作用域
+ 父组件模板的所有东西都会在父级作用域内编译
+ 子组件模板的所有东西都会在子级作用域内编译
```
<div id='app'>
  <cpn v-show='isShow'></cpn> <!--Vue实例里的isShow-->
  <cpn v-for='item in names'></cpn>
</div>
```
```
<template id='cpn'>
  <div>
    <button v-show='isShow'></button> <!--components里cpn的isShow-->
  </div>
</template>
```
#### 作用域插槽
+ 父组件替换插槽的标签，但是内容由子组件来提供
```
<div id='app'>
  <cpn></cpn>
  <cpn>
    <!--目的是获取子组件中的pLanguages-->
    <template slot-scope='slot'>
      <span v-for='item in slot.data'>{{ item }}-</span>
      <span>{{ slot.data.join('-') }}</span>
    </template>
  </cpn>
</div>

<template id='cpn'>
  <div>
    <slot :data='pLanguages'>
      <ul>
        <li v-for='item in pLanguages'>{{ item }}</li>
      </ul>
     </slot>
  </div>
</template>

<script>
  const app = new Vue({
    el: '#app',
    data: {},
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            pLanguages: ['JavaScript','C++','Java','C#','Python']
          }
        }
      }
    }
  })
</script>
```
## 模块化开发
+ 简单写js代码带来的问题
+ 闭包引起代码不可复用
*模块最基础的封装
```
//在匿名函数内部，定义一个对象
//给对象添加各种需要暴露到外面的属性和方法(不需要暴露的直接定义即可)
//最后将这个对象返回，并且在外面使用了一个MoudleA接受
var ModuleA = (function() {
  //1.定义一个对象
  var obj = {}
  //2.在对象内部添加变量和方法
  obj.flag = true
  obj.myFunc = function (info) {
    console.log(info);
  }
  //3.将对象返回
  return obj
})
```
```
//在main.js中只需要使用属于自己模块的属性和方法即可
if(ModuleA.flag){
  console.log;
}
ModuleA.myFunc('')
console.log(ModuleA)
```
模块化有两个核心：**导出**和**导入**

CommonJS的导出
```
module.exports = {}
```
CommonJS的导入
```
let {} = require('moduleA')
```
### 一、ES6的模块化实现
#### 1.ES6模块化的导入和导出
##### `export`用来**导出**变量
```
<script src='aaa.js' type='module'></script>
```
①导出方式一
```
export {
  flag, sum
}
```
②导出方式二
```
export var num = 1000;
export var height = 1.88
```
③导出函数/类
```
export function sum(num1, num2) {
  return num1 + num2
}

export class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  run() {
    console.log('在奔跑')
  }
}
```
④export default

+ 某些情况下，一个模块中包含某个功能，我们并不希望给这个功能命名，而是让导入者可以自己来命名
  - 这个时候就可以使用export default
```
const address = '北京市'
export default address
```
```
export default function (argument) {
	console.log(argument)
}
```
+ export default在同一个模块中，不允许同时存在多个

##### `import`用来**导入**模块
```
import {flag, sum, Person} from './aaa.js';

const p = new Person()
p.run()
```
导入export default中的内容时不需要加`{}`
```
import addr from './aaa.js'
```
统一全部导入
```
import * as info from './aaa.js'
console.log(info.flag)
```
## Webpack
+ 从本质上来讲，webpack是一个现代的JavaScript应用的静态**模块** **打包**工具
+ webpack其中一个核心就是让我们可能进行模块化开发，并且会帮助我们处理模块间的依赖关系
  - JavaScript、CSS、图片、json文件等等在webpack中都可以被当做模块来使用
+ webpack为了可以正常运行，必须依赖node环境
  - node环境为了可以正常执行很多代码，必须其中包含各种依赖的包 => npm工具(node packages manager)
### 一、webpack的使用
#### 1.准备工作
+ **`dist`文件夹：用于存放之后打包的文件**
+ **`src`文件夹：用于存放我们写的源文件**
+ `main.js`：项目的入口文件
  - mathUtils.js：定义了一些数学工具函数，可以在其他地方引用，并且使用
+ `index.html`：浏览器打开展示的首页html

#### js文件的打包
```
webpack ./src/main.js ./dist/bundle.js
```
打包后会在dist文件夹下，生成一个bundle.js文件

bundle.js文件，是webpack处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在index.html中引入即可

```
<script src='./dist/bundle.js'></script>
```
#### webpack的配置
`package.json`：通过`npm init`生成的，npm包管理的文件

```npm install```

可以将webpack的入口和出口参数写到配置中，在运行时直接读取:

创建一个`webpack.config.js`文件
```
const path = require ('path')	//依赖node里的包

module.exports = {
	//入口：可以是字符串/数组/对象
	entry: './src/main.js',
	//出口：通常是一个对象，里面至少包含两个重要属性，path和filename
	output: {
		path: path.resolve(__dirname,'dist'),	//绝对路径 => 动态地获取路径
		filename: 'bundle.js'
	}
}
```
以上使用的webpack是全局的webpack

但是一个项目往往依赖特定的webpack版本，全局的版本可能和这个项目的webpack版本不一致，导致打包出现问题

所以通常一个项目，都有自己局部的webpack

①项目中需要安装自己局部的webpack
```
npm install webpack@3.6.0 --save-dev
```
*在终端敲```webpack```命令，用的都是全局的

②通过```node_modules/.bin/webpack```启动webpack打包

我们可以在package.json的scripts中定义自己的执行脚本，让```webpack```命令和```npm run build```命令映射起来

打开`package.json`文件：

```
{
	...
	'scripts': {
		'build': 'webpack'
	},
	...
}
```
执行`npm run build`

package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置:

首先，会寻找本地的node_modules/.bin路径中对应的命令

如果没有找到，会去全局的环境变量中寻找


### 二、loader
loader是webpack中一个非常核心的概念

在我们之前的实例中，我们主要是用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖

但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等

对于webpack本身的能力来说，对于这些转化是不支持的

所有我们需要给webpack扩展对应的loader

大部分loader我们都可以在[webpack](https://webpack.docschina.org/)的官网中找到，并且学习对应的用法

#### loader使用过程
①通过npm安装需要使用的loader

②在webpack.config.js中的module关键字下进行配置

#### css文件的配置
`css-loader`只负责加载css文件，但是并不负责将css具体样式嵌入到文档中

`style-loader`负责将样式添加到DOM中

*style-loader需要放在css-loader的前面。

*因为webpack在读取使用的loader的过程中，是按照**从右向左**的顺序读取的。

在`main.js`（入口文件）中引用：
```
require('./css/normal.css')
```

#### less文件的处理

#### 图片文件的处理
```css
body {
	background: url(../imgs/1.jpg)
}
```
安装`url-loader`

##### ①当加载的图片小于`limit`（默认8196 => 8kb）时，会将图片编译成`base64`字符串形式

##### ②当加载的图片大于`limit`时，需要安装`file-loader`模块进行加载

再次打包，就会发现`dist`文件夹下多了一个图片文件

（webpack自动帮助我们生成一个非常长的名字，这是一个32位hash值，目的是防止名字重复

但是，真实开发中，我们可能对打包的图片名字有一定的要求

比如，将所有的图片放在一个文件夹中，跟上图片原来的名称，同时也要防止重复）

```webpack.config.js
options: {
	limit: 8192,
	name: 'img/[name].[hash:8].[ext]'
}
//img：文件要打包到的文件夹
//name：获取图片原来的名字，放在该位置
//hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位
//ext：使用图片原来的扩展名
```

图片没有显示 => 我们整个程序是打包在dist文件夹下的，所以我们需要在路径下再添加一个dist/：

##### 在`webpack.config.js`中的output下进行配置
```
module.exports = {
	...
	output: {
		...
		publicPath: 'dist/'
	}
}
```

#### ES6转ES5的babel
如果仔细阅读webpack打包的js文件，发现写的ES6语法并没有转成ES5，那么就意味着可能一些对ES6还不支持的浏览器没有办法很好的运行代码

如果希望将ES6的语法转成ES5，那么就需要使用babel

而在webpack中，直接使用babel对应的loader就可以了

配置webpack.config.js文件

#### 配置Vue
```
npm install vue --save
```
使用Vue进行开发
```
import Vue from 'vue'
```
有报错 =>

`runtime-only` -> 代码中不可以有任何的template

`runtime-compiler` -> 代码中可以有template，因为有compiler可以用于编译template

配置webpack.config.js文件

```
module.exports = {
	resolve: {
		alias: {
			'vue$': 'vue/dist/vue.esm.js'
		}
	}
}
```

#### 创建Vue时template和el关系
el用于指定Vue要管理的DOM，可以帮助解析其中的指令、事件监听等等；

而如果Vue实例中同时指定了template，那么template模板的内容会替换掉挂载的对应el的模板

这样做就不需要在以后的开发中再次操作index.html，只需要在template中写入对应的标签即可
```
new Vue({
	el: '#app',
	template: `
		<div>
			<h2>{{ message }}</h2>
		</div>
	`
})
```
#### 	Vue的使用
```app.vue
<template>
	<div>
		<h2 class='title'>{{ message }}</h2>
		<button @click='btnClick'>按钮</button>
	</div>
</template>

<script>
	export default {
		name: 'App',
		data() {
			return {
				message: 'Hello Webpack'
			}
		},
		methods: {
			btnClick() {
			}
		}			
	}
</script>

<style scoped>
	.title {
		color: yellow;
	}
</style>
```

```main.js
import App from './vue/App.vue'

new Vue({
	el: '#app',
	template: '<App/>',
	components: {
		APP
	}
})
```
安装`vue-loader`和`vue-template-compiler`

修改webpack.config.js的配置文件
```
{
	test: /\.vue$/,
	use: ['vue-loader']
}
```
省略js、css、vue的后缀名

修改webpack.config.js的配置文件

```
module.exports = {
	resolve: {
		extensions: ['.js','.css','.vue']
	}
}
```

### 三、plugin
#### plugin是什么？

plugin是插件的意思，通常是用于对某个现有的架构进行扩展

webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等

#### loader和plugin区别

loader主要用于转换某些类型的模块，它是一个转换器

plugin是插件，它是对webpack本身的扩展，是一个扩展器

#### plugin的使用过程

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)

步骤二：在webpack.config.js中的plugins中配置插件

`BannerPlugin`：为打包的文件添加版权声明，属于webpack自带的插件

```
const webpack = require('webpack')

module.exports = {
	...
	plugins: [
		new webpack.BannerPlugin('最终版权归...所有')
	]
}
```





# [VueCLI](VueCLI.md)
