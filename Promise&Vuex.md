# Promise
## 一、Promise的介绍
Promise是异步编程的一种解决方案
```script
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
## 二、Promise的三种状态
![图片10](图片10.jpg)

当我们开发中有异步操作时, 就可以给异步操作包装一个Promise

异步操作之后会有三种状态：

①pending：等待状态，比如正在进行网络请求，或者定时器没有到时间

②fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()

③reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

```
//另外处理方式
new Promosie((resolve, reject) => {
  setTimeout(() => {
    //  resolve('Hello Vuejs')
    reject('error message')
  },1000)
}).then(data => {
  console.log(data)
}, err => {
  console.log(err);
})
```

## 三、Promise链式调用
```
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
### `Promise.resovle()`：将数据包装成Promise对象，并且在内部回调resolve()函数

### `Promise.reject()`：将数据包装成Promise对象，并且在内部回调reject()函数

```
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
```
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
```
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
## Promise的all方法使用
```
Promise.all([
  new Promise((resolve, reject) => {
   setTimeout(() => {
    resolve('result1');
   }, 2000)
  }),
  new Promise((resolve, reject) => {
   setTimeout(() => {
    resolve('result2');
   }, 1000) 
  })
]).then(results => {
  console.log(results);
})
```

# Vuex
## 一、认识Vuex
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式，用于在多个组件间共享状态
### 单界面的状态管理
![图片11](图片11.jpg)

State：状态，姑且可以当做是data中的属性

View：视图层，可以针对State的变化，显示不同的信息

Actions：这里主要是用户的各种操作：点击、输入等等，会导致状态的改变

### 多界面的状态管理

## 二、Vuex的基本使用
```src/store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

//  1.安装插件
Vue.use(Vuex)

//  2.创建对象
const store = new Vuex.Store({
  state: {
    counter: 0
  },
  mutations: {
  },
  actions: {
  },
  getters: {
  },
  modules: {
  }
})
 
//  3.导出store对象
export default store

```


```
<template>
  <div>
    <h2>{{ $store.state.counter }}</h2>
  </div>
</template>
```
