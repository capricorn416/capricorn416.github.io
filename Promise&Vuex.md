# Promise
## 一、Promise的理解和使用

### 1. 理解

+ 抽象表达
  + Promise是异步编程的一种新解决方案
    + 旧方案是单纯使用回调函数
+ 具体表达
  + 从语法上来说：Promise是一个构造函数
  + 从功能上来说：promise对象用来封装一个异步操作并可以获取其成功/失败的结果值

### 2. 优点

+ 指定回调函数的方式更加灵活
  + 旧的：必须在启动异步任务前指定
  + promise：启动异步任务 => 返回promise对象 => 给promise对象绑定回调函数（甚至可以在异步任务结束后指定多个）
+ 支持链式调用，可以解决回调地狱问题
  + 回调地狱：
    + 回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件
    + 不便于阅读、不便于异常处理

### 3. Promise的三种状态
![图片10](图片10.jpg)

+ 当我们开发中有异步操作时, 就可以给异步操作包装一个Promise

+ Promise的状态

  + 实例对象中的一个属性  [PromiseState]

  + 异步操作之后会有三种状态：

    ①pending：等待状态，比如正在进行网络请求，或者定时器没有到时间

    ②fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()

    ③reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

+ 状态改变：

  + pending变为resolved
  + pending变为rejected
  + 说明：
    + 只有这2种，且一个promise对象只能改变一次
    + 无论变为成功还是失败，都会有一个结果数据

### 4. Promise对象的值

+ 实例对象中的另一个属性  [PromiseResult]
+ 保存着异步任务 成功/失败 的结果
+ 只有resolve和reject能对其进行修改

### 5. API

+ Promise构造函数：Promise(excutor) {}

  + excutor函数：执行器 (resolve, reject) => {}

  + resolve函数：内部定义成功时我们调用的函数 value => {}

  + reject函数：内部定义失败时我们调用的函数 reason => {}

  + 说明：executor会在Promise内部立即同步调用，异步操作在执行器中执行

    ```js
    new Promise((resolve, reject) => {
      // 同步调用的
      console.log(111)
    })
    console.log(222)	// 111 222
    ```

+ Promise.prototype.then 方法：(onResolved, onRejected) => {}

  + onResolved函数：成功的回调函数 (value) => {}
  + onRejected函数：失败的回调函数 (reason) => {}
  + 说明：
    + 指定用于得到成功value的成功回调和用于得到失败reason的失败回调
    + 返回一个新的promise对象

+ Promise.prototype.catch 方法：(onRejected) => {}

  + onRejected函数：失败的回调函数 (reason) => {}
  + 说明：then()的语法糖, 相当于：then(undefined, onRejected)

```vue
<script>
  //  参数 -> 函数(resolve, reject)
  //  resolve, reject本身它们又是函数
  //  链式编程
  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve()
    },1000)
  }).then(() => {
    console.log('Hello World');
    
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve()
      },1000)
    }).then(() => {
      console.log('Hello Vuejs');
      
      setTimeout(() => {
        console.log('Hello Python');
      },1000)
    })
  })
  //  什么情况下会用到Promise？一般情况下是有异步操作时，使用Promise对这个异步操作进行封装
  //  new -> 构造函数（1.保存一些状态信息 2.执行传入的函数）
  //  在执行传入的回调函数时，会传入两个参数，resolve，reject，本身又是函数
  new Promise((resolve,reject) => {
    setTimeout() => {
      //  成功的时候调用resolve
      resolve('Hello World')
      
      //  失败的时候调用reject
      reject(error message'')
    },1000)
  }).then((data) => {
    console.log(data);
  }).catch(err => {
    console.log(error);
  })
</script>
```

```js
//另外处理方式
new Promosie((resolve, reject) => {
  setTimeout(() => {
    //  resolve('Hello Vuejs')
    reject('error message')
  },1000)
}).then(data => {
  console.log(data)
}, err => {
  console.log(err)
})
```

+ Promise.resolve 方法：(value) => {} 

  + value：成功的数据或 promise 对象 
  + 说明：返回一个成功/失败的 promise 对象

  ```js
  // 如果传入的参数为 非promise类型的对象，则返回的结果为成功promise对象
  let p1 = Promise.resolve(521)
  
  // 如果传入的参数为 promis对象，则参数的结果决定了resolve的结果
  let p2 = Promise.resolve(new Promise((resolve, reject) => {
    // resolve('OK')
    reject('Error')
  }))
  p2.catch(reason => {
    console.log(reason)
  })
  ```

+ Promise.reject 方法：(reason) => {}

  + reason：失败的原因
  + 说明：返回一个失败的 promise 对象

+ Promise.all 方法：(promises) => {}

  + promises：包含 n 个 promise 的数组
  + 说明：返回一个新的 promise, 只有所有的 promise 都成功才成功, 只要有一个失败了就直接失败

  ```js
  let p1 = new Promise((resolve, reject) => { resolve('OK') })
  let p2 = Promise.resolve('Success')
  const result = Promise.all([p1, p2])
  ```

+ Promise.race 方法：(promises) => {}

  + promises：包含 n 个 promise 的数组
  + 说明：返回一个新的 promise, 第一个完成的 promise 的结果状态就是最终的结果状态

### 6. promise的几个关键问题

+ 如何改变 promise 的状态？

  + resolve(value)：如果当前是 pending 就会变为 resolved
  + reject(reason)：如果当前是 pending 就会变为 rejected
  + throw '' 抛出异常：如果当前是 pending 就会变为 rejected

+ 一个 promise 指定多个成功/失败回调函数, 都会调用吗？

  + 当 promise 改变为对应状态时都会调用

+ 改变 promise 状态和指定回调函数谁先谁后？

  + 都有可能，正常情况下是先指定回调再改变状态，但也可以先改状态再指定回调
  + 如何先改状态再指定回调？
    + 在执行器中直接调用 resolve()/reject()
    + 延迟更长时间才调用 then()
  + 什么时候才能得到数据？
    + 如果先指定的回调，那当状态发生改变时，回调函数就会调用，得到数据
    + 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据

+ promise.then()返回的新 promise 的结果状态由什么决定？

  + 简单表达：由 then()指定的回调函数执行的结果决定
  + 详细表达：
    + 如果抛出异常，新 promise 变为 rejected，reason 为抛出的异常
    + 如果返回的是非 promise 的任意值，新 promise 变为 resolved，value 为返回的值
    + 如果返回的是另一个新 promise，此 promise 的结果就会成为新 promise 的结果

+ promise 如何串连多个操作任务？

  + promise 的then()返回一个新的 promise，可以开成then()的链式调用
  + 通过then的链式调用串连多个同步/异步任务

  ```js
  let p = new Promise((resolve, reject) => resolve('OK'))
  p.then(value => return new Promise((resolve, reject) => resolve('success')))
  .then(value => console.log(value))	// success
  .then(value => console.log(value))	// undefined
  ```

+ promise 异常传透？

  + 当使用 promise 的 then 链式调用时，可以在最后指定失败的回调
  + 前面任何操作出了异常，都会传到最后失败的回调中处理

+ 中断 promise 链？

  + 当使用 promise 的 then 链式调用时，在中间中断，不再调用后面的回调函数
  + 办法：在回调函数中返回一个 pendding 状态的 promise 对象
  
  ```js
  let p = new Promise((resolve, reject) => resolve('OK'))
  p.then(value => {
    console.log(111)
    return new Promise(() => {})
  }).then(value =>{
    console.log(222)
  }).then(value => {
    console.log(333)
  }).catch(reason => {
    console.warn(reason)
  })
  ```

## 二、Promise链式调用

```js
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  },1000)
}).then(data => {
  console.log(data);
  
  return new Promise((resolve) => {
    resolve(data+'111')
  })
}).then(data => {
  console.log(data);
  
  return new Promise((resolve) => {
    resolve(data+'222')
  })
}).then(data => {
  console.log(data);
})
```
+ `Promise.resovle()`：将数据包装成Promise对象，并且在内部回调resolve()函数

+ `Promise.reject()`：将数据包装成Promise对象，并且在内部回调reject()函数

```js
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  },1000)
}).then(data => {
  console.log(data);
  
  return Promise.resolve('data'+'111')
}).then(data => {
  console.log(data);
  
  return Promise.resolve('data'+'222')
}).then(data => {
  console.log(data);
})
```
### 省略Promise.resolve
```js
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  },1000)
}).then(data => {
  console.log(data);
  
  return 'data'+'111'
}).then(data => {
  console.log(data);
  
  return 'data'+'222'
}).then(data => {
  console.log(data);
})
```
```js
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa')
  },1000)
}).then(data => {
  console.log(data);
  
  //return Promise.reject('error message');
  throw 'error message'
}).then(data => {
  console.log(data);
  
  return 'data'+'222'
}).then(data => {
  console.log(data);
}).catch(err => {
  console.log(err);
})
```

## 三、 自定义（手写）Promise

### 1. 定义整体结构

```js
// 声明构造函数
function Promise(executor) {}
// 添加then方法
Promise.prototype.then = function(onResolved, onRejected) {}
```

### 2. resolve与reject结构搭建

```js
function Promise(executor) {
  // resolve函数
  function resolve(data) {}
  // reject函数
  function reject(data) {}
  // 同步调用[执行器函数]
  executor(resolve, reject)
}
```

### 3. resolve与reject代码实现

```js
function Promise(executor) {
  // 添加属性
  this.PromiseState = 'pending'
  this.PromiseResult = null
  const self = this
  function resolve(data) {
    console.log(this)	// window
    // 1.修改对象的状态（PromiseState）
    self.PromiseState = 'fulfilled'
    // 2.设置对象结果值（PromiseResult）
    self.PromiseResult = data
  }
  function reject(data) {
    self.PromiseState = 'rejected'
    self.PromiseResult = data
  }
  executor(resolve, reject)
}
```

### 4. throw抛出异常改变状态

```js
function Promise(executor) {
  ...
  try{
    executor(resolve, reject)
  }catch(e) {
    reject(e)
  }
}
```

### 5. Promise对象状态只能修改一次

```js
function Promise(executor) {
  ...
  function resolve(data) {
    if(self.PromiseState !== 'pending') return
    ...
  }
  function reject(data) {
    if(self.PromiseState !== 'pending') return
    ...
  }
  ...
}
```

### 6. then方法执行回调

```js
Promise.prototype.then = function(onResolved, onRejected) {
  // 调用回调函数
  if(this.PromiseState === 'fulfilled') {
  	onResolved(this.PromiseResult)
  }
  if(this.PromiseState === 'rejected') {
  	onRejected(this.PromiseResult)
  }
}
```

### 7. 异步任务回调的执行

```js
function Promise(executor) {
  // 声明属性
  this.callback = {}
  function resolve(data) {
    self.PromiseState = 'fulfilled'
    self.PromiseResult = data
    // 调用成功的回调函数
    if(self.callback.onResolved) {
      self.callback.onResolved(data)
    }
  }
  function reject(data) {
    self.PromiseState = 'rejected'
    self.PromiseResult = data
    if(self.callback.onRejected) {
      self.callback.onRejected(data)
    }
  }
  executor(resolve, reject)
}

Promise.prototype.then = function(onResolved, onRejected) {
  // 判断pending状态
  if(this.PromiseState === 'pending') {
    // 保存回调函数
    this.callback = {
      onResolved,
      onRejected
    }
  }
}
```

### 8. 指定多个回调的实现

```js
function Promise(executor) {
  this.callbacks = []
  function resolve(data) {
    self.callbacks.forEach(item => {
      item.onResolved(data)
    })
  }
}

Promise.prototype.then = function(onResolved, onRejected) {
  if(this.PromiseState === 'pending') {
    this.callbacks.push({
      onResolved,
      onRejected
    })
  }
}
```

### 9. 同步修改状态then方法结果返回

```js
Promise.prototype.then = function(onResolved, onRejected) {
  return new Promise((resolve, reject) => {
    if(this.PromiseState === 'fulfilled') {
      try{
          // 获取回调函数的执行结果
          let result = onResolved(this.PromiseResult)
          // 判断
          if(result instanceof Promise) {
            result.then(v => {
              resolve(v)
            }, r => {
              reject(r)
            })
          }else {
            // 结果的对象状态为[成功]
            resolve(result)
          }
      }catch(e) {
          reject(e)
      }
    }
    if(this.PromiseState === 'rejected') {
  	  onRejected(this.PromiseResult)
    }
    if(this.PromiseState === 'pending') {}
  })
}
```

### 10. 异步修改状态then方法结果返回

```js
Promise.prototype.then = function(onResolved, onRejected) {
  const self = this
  return new Promise((resolve, reject) => {
    if(this.PromiseState === 'pending') {
      this.callbacks.push({
        onResolved: function() {
          // 执行成功的回调函数
          let result = onResolved(self.PromiseResult)
          if(...)...	// 如上
        },
        onRejected: function() {
          onRejected(self.PromiseResult)
        }
      })
    }
  })
}
```

### 11. then方法完善与优化

```js
Promise.prototype.then = function(onResolved, onRejected) {
  const self = this
  return new Promise((resolve, reject) => {
    if(this.PromiseState === 'rejected') {
  	  ... // 同上
      // 进一步封装
    }
  })
}
```

### 12. catch方法—异常穿透与值传递

```js
Promise.prototype.then = function(onResolved, onRejected) {
  // 判断回调函数参数
  if(typeof onRejected !== 'funtion') {
    onRejected = reason => {
      throw reason
    }
  }
  if(typeof onResolved !== 'function') {
    onResolved = value => value
  }
}

Promise.prototype.catch = function(onRejected) {
  return this.then(undefined, onRejected)
}
```

### 13. resolve方法封装

```js
Promise.resolve = function(value) {
  return new Promise((resolve, reject) => {
    if(value instanceof Promise){
      value.then(v => resolve(v), r=> reject(r))
    }else {
      resolve(value)
    }
  })
}
```

### 14. reject方法封装

```js
Promise.reject = function(value) {
  return new Promise((resolve, reject) => {
    reject(reason)
  })
}
```

### 15. all方法封装

```js
Promise.all = function(promises) {
  return new Promise((resolve, reject) => {
    let count = 0
    let arr = []
    for(let i=0; i<promises.length; i++) {
      promises[i].then(v => {
        // 得知对象的状态是成功
        // 每个promise对象都成功
        count++
        arr[i] = v
        if(count === promise.length) {
          resolve(arr)
        }
      }, r => {
        reject(r)
      })
    }
  })
}
```

### 16. race方法封装

```js
Promise.race = function(promises) {
  return new Promise((resolve, reject) => {
    for(let i=0; i<promises.length; i++) {
      promises[i].then(v => {
        resolve(v)
      }, r => {
        reject(r)
      })
    }
  })
}
```

### 17. then方法回调的异步执行

+ 包裹定时器

### 18. class版本的实现

```js
class Promise {
  // 构造方法
  constructor(executor) {}
  // then方法
  then(onResolved, onRejected) {}
  // catch方法
  catch(onRejected) {}
  static resolve(value) {}
  static reject(reason) {}
  static all(promises) {}
  static race(promises) {}
}

```



## 四、async与await

### 1. async函数

+ 函数的返回值为 promise 对象
+ promise 对象的结果由 async 函数执行的返回值决定

### 2. await表达式

+ await 右侧的表达式一般为 promise 对象，但也可以是其它的值
+ 如果表达式是 promise 对象，await 返回的是 promise 成功的值
+ 如果表达式是其它值，直接将此值作为 await 的返回值

### 3. 注意

+ await 必须写在 async 函数中，但 async 函数中可以没有 await
+ 如果 await 的 promise 失败了，就会抛出异常，需要通过 try...catch 捕获处理

### 4. async与await结合发送AJAX请求

```js
async function() {
  let data = await sendAJAX('http://xxx')
}
```






# Vuex

## 一、认识Vuex
+ Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式，用于在多个组件间共享状态
+ 专门在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信
+ 什么时候使用Vuex？
  + 多个组件依赖于同一状态
  + 来自不同组件的行为需要变更同一状态

### 1. 单界面的状态管理
![图片11](图片11.jpg)

+ State：状态，姑且可以当做是data中的属性

+ View：视图层，可以针对State的变化，显示不同的信息

+ Actions：这里主要是用户的各种操作：点击、输入等等，会导致状态的改变

### 2. 多界面的状态管理
![图片12](图片12.jpg)

+ actions：对象，用于响应组件中的动作
+ mutations：对象，用于操作数据（state）
+ state：对象，用于存储数据
  + 如果actions中没有网络请求或其他复杂的业务逻辑，VC组件也可以越过actions，直接通过commit联系mutations

## 二、搭建Vuex环境

+ src/store/index.js

  ```js
  // 该文件用于创建Vuex中最为核心的store
  import Vue from 'vue'
  import Vuex from 'vuex'
  
  //  1.安装插件
  Vue.use(Vuex)
  
  //  2.创建对象
  const store = new Vuex.Store({
    state: {},
    mutations: {},
    actions: {},
    getters: {},
    modules: {}
  })
   
  //  3.导出store对象
  export default store
  ```

+ main.js

  ```js
  import store from './store'
  new Vue({
    ...
    store
  })
  ```

## 三、Vuex的基本使用

1. 提取出一个公共的store对象，用于保存在多个组件中共享的状态
2. 将store对象放置在new Vue对象中，这样可以保证在所有的组件中都可以使用到
3. 在其他组件中使用store对象中保存的状态即可
   + 通过`$store.state.`属性的方式来访问状态
   + 通过`$store.dispatch('action中方法', 数据)`或`$store.commit('mutation中方法', 数据)`来修改状态

+ store/index.js

  ```js
  const state = {
    sum: 0
  }
  const actions = {
    jiaOdd(context, value) {
      if(context.state.sum%2) context.commit('JIAODD', value)
    }
  }
  const mutations = {
    JIAODD(state, value) {
      state.sum += value
    },
    JIA(state, value) {
      state.sum += value
    }
  }
  ```

+ 组件

  ```vue
  <template>
    <div>{{$store.state.sum}}</div>
  </template>
  
  <script>
  export default {
    methods: {
      increment() {
        // 如果actions中没有复杂的业务逻辑，VC组件也可以直接通过commit联系mutations
        this.$store.commit('JIA', this.n)  
      },
      incrementOdd() {
        this.$store.dispatch('jiaOdd', this.n)
      }
    }
  }
  </script>
  ```

  

## 四、Vuex核心概念
### 1. State
#### State单一状态树
+ 如果你的状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难

  所以Vuex也使用了单一状态树来管理应用层级的全部状态

+ 单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便地管理和维护

### 2. Getters
+ 类似计算属性，用于将state中的数据进行加工

+ getters默认是不能传递参数的, 如果希望传递参数, 那么只能让getters本身返回另一个函数

```js
const store = new Vuex.Store({
  state: {
    counter: 1000,
    students: [
      {name: 'why', age: 18},
      {name: 'kobe', age: 24},
      {name: 'james', age: 30},
      {name: 'curry', age: 10}
    ]
  },
  getters: {
    powerCounter(state) {
      return state.counter * state.counter
    },
    more20stu(state) {
      return state.students.filter(s => s.age > 20)
    },
    more20stuLength(state, getters) {
      return getters.more20stu.length
    },
    moreAgeStu(state) {
      return function(age) {
        return state.students.filter(s => s.age > age)
      }
    }
  }
})
```
```vue
<template>
  <div>
    <h2>{{ $store.getters.powerCounter }}</h2>
    // <h2>{{ more20stu }}</h2>
    <h2>{{ $store.getters.more20stu }}</h2>
    <h2>{{ $store.getters.more20stu.length }}</h2>
    <h2>{{ $store.getters.more20stuLength }}</h2>
    <h2>{{ $store.getters.moreAgeStu(12) }}</h2>
  </div>
</template>
```
```vue
<script>
  export default {
    computed: {
     // more20stu() {
     //   return this.$store.state.students.filter(s => s.age > 20)
     // }
    }
  }
</script>
```
### 3. Mutation
+ Vuex的store状态的更新唯一方式：提交Mutation

+ Mutation主要包括两部分：
  + 字符串的事件类型（type）
  + 一个回调函数（handler），该回调函数的第一个参数就是state

+ 在通过mutation更新数据的时候, 有可能我们希望携带一些额外的参数：参数被称为是mutation的载荷(Payload)
+ 当有很多参数需要传递时，通常以对象的形式传递, 也就是payload是一个对象


```js
mutations: {
  incrementCount(state, count) {
    state.counter += count
  }
}
```
```js
methods: {
  addCount(count) {
    this.$store.commit('incrementCount', count)
  }
}
```
#### Mutation的提交风格
+ 通过commit进行提交是一种普通的方式

+ Vue还提供了另外一种风格, 它是一个包含type属性的对象

+ Mutation中的处理方式是将整个commit的对象作为payload使用

```js
mutations: {
  incrementCount(state, payload) {
    state.counter += payload.count
  }
}
```
```js
methods: {
  addCount(count) {
    this.$store.commit({
      type: 'incrementCount',
      count
    })
  }
}
```
#### Mutation的响应规则
Vuex的store中的state是响应式的, 当state中的数据发生改变时, Vue组件会自动更新

这就要求我们必须遵守一些Vuex对应的规则：

（1）提前在store中**初始化**好所需的属性

（2）当给state中的对象添加新属性时, 使用下面的方式：

&ensp; &ensp; ①使用Vue.set(obj, 'newProp', 123)

&ensp; &ensp; ②用新对象给旧对象重新赋值
```
state: {
  info: {
    name: 'kobe',
    age: 40,
    height: 1.98
  }
},
mutations: {
  updateInfo(state) {
    //非响应式 state.info['address'] = '洛杉矶'
    Vue.set(state.info, 'address', '洛杉矶')
    //非响应式 delete state.info.age
    Vue.delete(state.info, 'age')
  }
}
```
#### Mutation的类型常量
在各种Flux实现中, 一种很常见的方案就是使用常量替代Mutation事件的类型

我们可以将这些常量放在一个单独的文件中, 方便管理以及让整个app所有的事件类型一目了然

我们可以创建一个文件: mutation-types.js, 并且在其中定义我们的常量

定义常量时, 我们可以使用ES2015中的风格, 使用一个常量来作为函数的名称
```
export INCREMENT = 'increment'
```
```
mutations: {
  [INCREMENT](state) {
    state.counter++
  }
}
```
```
methods: {
  addition() {
    this.$store.commit(INCREMENT)
  }
}
```
#### Mutation的同步函数
通常情况下, Vuex要求我们Mutation中的方法必须是**同步方法**

主要的原因是当我们使用devtools时, devtools可以帮助我们捕捉mutation的快照

但是如果是异步操作, 那么devtools将不能很好地追踪这个操作什么时候会被完成

### 4. Action
Action类似于Mutation, 但是是用来代替Mutation进行**异步操作**的
```
actions: {
  aUpdateInfo(context, payload) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        context.commit('updateInfo');
        console.log(payload);
        resolve('111')
      }, 1000)
    })
  }
}
```
在Vue组件中, 如果我们调用action中的方法, 那么就需要使用dispatch

同样的, 也是支持传递payload

```
methods: {
  updateInfo() {
    this.$store
    .dispatch('aUpdateInfo', '我是payload')
    .then(res => {
      console.log('里面完成了提交');
      console.log(res)
    })
  }
}
```
### 5. Module
+ Vue使用单一状态树,那么也意味着很多状态都会交给Vuex来管理;当应用变得非常复杂时,store对象就有可能变得相当臃肿

+ 为了解决这个问题, Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutations、actions、getters等

```js
const moduleA = {
  namespaced: true,	// 开启命名空间
  state: {
    name: 'zhangsan'
  },
  mutations: {
    updateName(state, payload) {
      state.name = payload
    }
  },
  getters: {
    fullname(state) {
      return state.name + '111'
    },
    fullname2(state, getters) {
      return getters.fullname + '222'
    },
    fullname3(state, getters, rootState) {
      return getters.fullname2 + rootState.counter
    }
  }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA
  }
})
```
```js
$store.state.a.name

...mapState('a', ['name'])

this.$store.commit('a/updateName', 'lisi')

$store.getters['a/fullname']
```

## 五、 map

### 1. mapState

+ 用于帮助我们映射`state`中的数据为计算属性

```js
import { mapState } from 'vuex'
export default {
  computed: {
    // 借助mapState生成计算属性，从state中读取数据（对象写法）
    ...mapState({he: 'sum', xuexiao: 'school'})
    // 借助mapState生成计算属性，从state中读取数据（数组写法）
    ...mapState(['sum', 'school'])
  }
}
```

### 2. mapGetters

+ 用于帮助我们映射`getters`中的数据为计算属性

```js
import { mapGetters } from 'vuex'
export default {
  computed: {
    // 借助mapGetters生成计算属性，从getters中读取数据（对象写法）
    ...mapGetters({bigSum: 'bigSum'})
    // 借助mapGetters生成计算属性，从getters中读取数据（数组写法）
    ...mapState(['bigSum'])
  }
}
```

### 3. mapMutations

+ 用于帮助我们生成与`mutations`对话的方法，即：包含`$store.commit(xxx)`的函数

+ 借助mapMutations生成对应的方法，方法中会调用commit去联系mutations
  + 对象写法
  + 数组写法

### 4. mapActions

+ 用于帮助我们生成与`actions`对话的方法，即：包含`$store.dispatch(xxx)`的函数

+ 借助mapActions生成对应的方法，方法中会调用dispatch去联系actions
  + 对象写法
  + 数组写法



## 六、Vuex的项目结构

![图片13](图片13.jpg)



# axios

## 一、axios的理解和使用

### 1. 特点

+ 基于promise的异步ajax请求库
+ 浏览器端/node端都可以使用
+ 支持请求/响应拦截器
+ 支持请求取消
+ 请求/响应数据转换
+ 批量发送多个请求

### 2. 常用语法

+ axios(config)  通用/最本质的发任意类型请求的方式
+ axios(url[, config])  可以只指定url发get请求
+ axios.request(config)  等同于axios(config)
+ axios.get(url[, config])  发get请求
+ axios.delete(url[, config])  发delete请求
+ axios.post(url[, data, config])  发post请求
+ axios.put(url[, data, config])  发put请求



+ axios.defaults.xxx  请求的默认全局配置
+ axios.interceptors.request.use()  添加请求拦截器
+ axios.interceptors.response.use()  添加响应拦截器



+ axios.create([config])  创建一个新的axios（它没有下面的功能）



+ axios.Cancel()  用于创建取消请求的错误对象
+ axios.CancelToken  用于创建取消请求的token对象
+ axios.isCancel()  是否是一个取消请求的错误
+ axios.all(promises)  用于批量执行多个异步请求
+ axios.spread()  用来指定接收所有成功数据的回调函数的方法

### 3. 基本使用
```js
import axios from 'axios'

axios({
  url: 'http://123.207.32.32:8000/home/multidata',
  method: 'get'
}).then(res => {
  console.log(res);
})

axios({
  url: 'http://123.207.32.32:8000/home/data',
  // 专门针对get请求的query参数拼接
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})
```
## 二、axios发送并发请求
+ `axios.all`，可以放入多个请求的数组

+ `axios.all([])` 返回的结果是一个数组，使用 `axios.spread` 可将数组 [res1,res2] 展开为 res1, res2

```js
axios.all([axios(), axios()])
  .then(results => {
  })
```
```js
axios.all([axios(), axios()])
  .then(axios.spread((res1, res2) => {
    console.log(res1),
    console.log(res2)
  })
```
## 三、axios的配置
+ 在开发中可能很多参数都是固定的

  这个时候我们可以进行一些抽取, 也可以利用axiox的**全局配置**：

```js
axios.defaults.baseURL = 'http://123.207.32.32:8000'
axios.defaults.timeout = 5000
```
## 四、axios的实例和模块封装
+ 某些请求需要使用特定的baseURL或者timeout或者content-Type等

  这个时候, 我们就可以创建新的实例, 并且传入属于该实例的配置信息

### 1. 创建axios实例

+ `axios.create(config)`
  + 根据指定配置创建一个新的axios，也就是每个新axios都有自己的配置
  + 新axios只是没有 取消请求 和 批量发请求 的方法，其他所有语法都是一致的
  + 为什么要设计这个语法？
    + 需求：项目中有部分接口需要的配置与另一部分接口需要的配置不太一样
    + 解决：创建2个新axios，每个都有自己特有的配置，分别应用到不同要求的接口请求中

```js
const instance1 = axios.create({	// instance1是函数类型
  baseURL: '',
  timeout: 
})

instance1({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
})

instance1({
  url: '/home/data',
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})
```
### 2. axios封装
#### ①方式一

```js
import axios from 'axios'

export function request(config, success, failure) {
  const instance = axios.create({
    baseURL: '',
    timeout: 
  })
  
  instance(config)
    .then(res => {
      success(res);
    })
    .catch(err => {
      failure(err)
    })
}
```
```js
import {request} from './network/request'

request({
  url: '/home/multidata'
}, res => {
  console.log(res);
}, err => {
  console.log(err);
})
```
#### ②方式二

```js
import axios from 'axios'

export function request(config) {
  const instance = axios.create({
    baseURL: '',
    timeout: 
  })
  
  instance(config.baseConfig)
    .then(res => {
      config.success(res);
    })
    .catch(err => {
      config.failure(err)
    })
}
```
```js
import {request} from './network/request'

request({
  baseConfig: {
  },
  success: function(res) {
  },
  failure: function(err) {
  }
})
```
#### ③**方式三**

```js
export function request(config) {
  return new Promise((resolve, reject) => {
    const instance = axios.create({
      baseURL: '',
      timeout: 
    })

    instance(config)
      .then(res => {
        resovle(res)
      })
      .catch(err => {
        reject(err)
      })  
  })
}
```
```js
import {request} from './network/request'

request({
  url: ''
}).then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```
#### ④**方法四**

```js
export function request(config) {
  const instance = axios.create({
    baseURL: '',
    timeout: 
  })
  
  return instance(config)
}
```
## 五、axios的拦截器

+ axios提供了拦截器，用于我们在发送每次请求或者得到相应后，进行对应的处理
  + `instance.interceptors.request`：拦截请求
  + `instance.interceptors.response`：拦截响应
+ 请求拦截器是后添加先执行，响应拦截器是先添加先执行

```js
export function request(config) {
  // 1.创建axios的实例
  const instance = axios.create({
    baseURL: '',
    timeout: 
  })
  
  // 2.axios的拦截器
  // 2.1.请求拦截
  instance.interceptors.request.use(config => {
    console.log(config)
    // 1.比如config中的一些信息不符合服务器的要求
    // 2.比如每次发送网络请求时，都希望在界面中显示一个请求的图标
    // 3.某些网络请求（比如登录（token）），必须携带一些特殊的信息
    return config
  }, err => {
    console.log(err)
    return Promise.reject(error)
  })
  
  // 2.2.响应拦截
  instance.interceptors.response.use(res => {
    console.log(res)
    return res.data;
  }, err => {
    console.log(err)
    return Promise.reject(error);
  })
  
  // 3.发送真正的网络请求
  return instance(config)
}
```

## 六、 项目

+ src/utils/request.js

  ```js
  import axios from 'axios'
  
  const request = axios.create({
      baseURL: '/api/welcome'
  })
  /*
  request.interceptors.request.use(
    // 所有请求会经过这里
    // config是当前请求相关的配置信息对象
    function(config) {
      const user = window.localStorage.getItem('user')
      // 如果有用户登录信息，则统一设置token
      if(user) {
        config.headers.Authorization = `Bearer ${user}`
      }
      // 当这里return config之后，请求才会真正地发出去
      return config;
  }, 
    function (error) {
      // 请求失败，会经过这里
      return Promise.reject(error)
  })
  */
  export default request
  ```

+ src/api/xxx.js

  ```js
  import request from "../utils/request"
  
  export const sendForm = (data) => {
    return request({
      method: 'POST',
      url: '/resume',
      data
    })
  }
  
  export const getUploadToken = async ()=>{
      return (await request.get('/uploadToken')).data
  }
  ```

## 七、取消请求

### 1. 基本流程

+ 配置cancelToken对象
+ 缓存用于取消请求的cancel函数
+ 在后面特定时机调用cancel函数取消请求
+ 在错误回调中判断如果error是cancel，做相应处理

### 2. 实现功能
