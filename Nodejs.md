# Node.js
## 一、day1
### (一) Node.js介绍
#### 1. Node.js是什么
+ Node.js不是一门语言，不是库、不是框架
+ Node.js是一个JavaScript运行时环境
+ Node.js可以解析和执行JavaScript代码
  - 现在的JS可以脱离浏览器来运行，归功于Node.js
#### 2. Node.js中的JavaScript
+ ECMAScript基本的JavaScript语言部分
  - 没有BOM、DOM
+ 核心模块
  - Node为JavaScript提供了很多服务器级别的API，这些API绝大多数都被包装到了一个具名的核心模块中了
    * 例如文件操作的`fs`核心模块，http服务构建的`http`模块，`path`路径操作模块，`os`操作系统信息模块
  - 要使用核心模块，就必须`var x = require('x')`
+ 用户自己编写的文件模块
  - 在Node中，没有全局作用域，只有**模块作用域**
  - 相对路径必须加./ ，可以省略后缀名
  - require方法有两个作用
    * 加载文件模块并执行里面的代码
    * 拿到被加载文件模块导出的接口对象
  - 在每个文件模块中都提供了一个对象：exports
    * exports默认是一个空对象
    * 所有需要被外部访问的成员挂载到这个exports对象中 `exports.add = function() {}`
#### 3. Node.js的特性
+ event-driven 事件驱动
+ non-blocking I/O model 非阻塞IO模型（异步）
+ lightweight and efficient 轻量和高效
#### 4. npm是世界上最大的开源库生态系统
+ 绝大多数JavaScript相关的包都存放在了npm上
#### 5. Node.js能做什么
+ Web服务器后台
+ 命令行工具
#### 6. ip地址和端口号
+ IP地址用来定位计算机
+ 端口号用来定位具体的应用程序（所有需要联网通信的软件都必须占用一个端口号）
  - 端口号的范围从0-65536之间
### (二) 使用Node执行js脚本文件
#### 1. 基本使用
+ 创建编写JavaScript脚本文件
+ 打开终端，定位到脚本文件所属目录
+ 在命令行输入`node xxx.js`执行对应文件
  - 注意：文件名不要使用node.js来命名
#### 2. 操作文件
+ 浏览器中的 JavaScript 是没有文件操作的能力的，但是 Node 中的 JavaScript 具有文件操作的能力
+ fs 是 file-system 的简写，就是文件系统的意思
+ 在 Node 中如果想要进行文件操作，就必须引入 fs 这个核心模块
+ 在 fs 这个核心模块中，就提供了所有的文件操作相关的 API
  - 读取文件 `fs.readFile`
    ```
    // 1. 使用 require 方法加载 fs 核心模块
    var fs = require('fs')

    // 2. 读取文件
    //    第一个参数就是要读取的文件路径
    //    第二个参数是一个回调函数         
    //        成功
    //          data 数据
    //          error null
    //        失败
    //          data undefined没有数据
    //          error 错误对象
    fs.readFile('./data/a.txt', function (error, data) {
      // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
      // 文件中存储的其实都是二进制数据 0 1
      // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
      // 但是无论是二进制01还是16进制，人类都不认识
      // 所以我们可以通过 toString 方法把其转为我们能认识的字符
      // console.log(data)

      // console.log(error)
      // console.log(data)

      // 在这里就可以通过判断 error 来确认是否有错误发生
      if (error) {
        console.log('读取文件失败了')
      } else {
        console.log(data.toString())
      }
    })
    ```
  - 写文件 `fs.writeFile`
    ```
    // 第一个参数：文件路径
    // 第二个参数：文件内容
    // 第三个参数：回调函数
    //    error
    //    
    //    成功：
    //      文件写入成功
    //      error 是 null
    //    失败：
    //      文件写入失败
    //      error 就是错误对象
    fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function (error) {
      // console.log('文件写入成功')
      // console.log(error)
      if (error) {
        console.log('写入失败')
      } else {
        console.log('写入成功了')
      }
    })
    ```
#### 3. 简单的http服务
+ 可以使用 Node 构建一个 Web 服务器
+ 在 Node 中专门提供了一个核心模块：http
+ http 这个模块的职责就是帮你创建编写服务器的
  - 简单的http服务
    ```
    // 1. 加载 http 核心模块
    var http = require('http')

    // 2. 使用 http.createServer() 方法创建一个 Web 服务器
    //    返回一个 Server 实例
    var server = http.createServer()

    // 3. 服务器要干嘛？
    //    提供服务：对 数据的服务
    //    发请求
    //    接收请求
    //    处理请求
    //    给个反馈（发送响应）
    //    注册 request 请求事件
    //    当客户端请求过来，就会自动触发服务器的 request 请求事件，然后执行第二个参数：回调处理函数
    server.on('request', function () {
      console.log('收到客户端的请求了')
    })

    // 4. 绑定端口号，启动服务器
    server.listen(3000, function () {
      console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
    })
    ```
  - 发送响应
    ```
    // request 请求事件处理函数，需要接收两个参数：
    //    Request 请求对象
    //        请求对象可以用来获取客户端的一些请求信息，例如请求路径
    //    Response 响应对象
    //        响应对象可以用来给客户端发送响应消息
    server.on('request', function (request, response) {
      // http://127.0.0.1:3000/ /
      // http://127.0.0.1:3000/a /a
      // http://127.0.0.1:3000/foo/b /foo/b
      console.log('收到客户端的请求了，请求路径是：' + request.url)
      console.log('请求我的客户端的地址是：' + request.socket.remoteAddress, request.socket.remotePort)
      
      // response 对象有一个方法：write 可以用来给客户端发送响应数据
      // write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
      response.write('hello')
      response.write(' nodejs')
      // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
      // 响应内容只能是二进制数据或者字符串
      // res.end('hello nodejs')
      
      // 在服务端默认发送的数据，其实是 utf8 编码的内容
      // 但是浏览器不知道你是 utf8 编码的内容
      // 浏览器在不知道服务器响应内容的编码的情况下会按照当前操作系统的默认编码去解析
      // 中文操作系统默认是 gbk
      // 解决方法就是正确的告诉浏览器我给你发送的内容是什么编码的
      // 在 http 协议中，Content-Type 就是用来告知对方我给你发送的数据内容是什么类型
      // res.setHeader('Content-Type', 'text/plain; charset=utf-8')
      // res.end('hello 世界')
      
      // text/plain 就是普通文本
      // 如果你发送的是 html 格式的字符串，则也要告诉浏览器我给你发送是 text/html 格式的内容

      // 告诉客户端，我的话说完了，你可以呈递给用户了
      response.end()
    })
    ```
+ 可以同时开启多个服务，但一定要确保不同服务占用的端口号不一致。在一台计算机中，同一个端口号同一时间只能被一个程序占用  
+ [Content-Type](https://tool.oschina.net/commons)
  - 对于文本类型的数据，最好都加上编码，防止中文解析乱码问题
  - 图片就不需要指定编码了，因为我们常说的编码一般指的是：字符编码
#### 4. 初步实现Apache功能
+ 得到wwwDir目录列表中的文件名和目录名
  - `fs.readdir`
+ 将得到的文件名和目录名替换到template.html中
+ 模板引擎
  - ```npm install art-template```
  - 该命令在哪执行就会把包下载到哪里，默认会下载到node_modules目录中
  - 在html中引用
    ```
    <script src="node_modules/art-template/lib/template-web.js"></script>
    <script type="text/template" id="tpl">
      hello {{ name }}
    </script>
    <script>
      var ret = template('tpl', {
        name: 'Jack'
      })
      console.log(ret);
    </script>
    ```
    ```
    <script type="text/template" id="tpl">
      <div>
        <label for="">用户名</label>
        <input type="text" value="{{ user.username }}">
      </div>
      <div>
        <label for="">职业</label>
        <select name="" id="">
          {{ each jobs }} {{ if user.job === $value.id }}
          <option value="{{ $value.id }}" selected>{{ $value.name }}</option>
          {{ else }}
          <option value="{{ $value.id }}">{{ $value.name }}</option>
          {{ /if }} {{ /each }}
        </select>
      </div>
    </script>
    ```
  - 模板引擎不关心字符串内容，只关心自己能认识的模板标记语法，例如{{}}
+ 服务端渲染和客户端渲染
  - 客户端渲染不利于SEO搜索引擎优化
  - 服务端渲染是可以被爬虫抓取到的，客户端异步渲染是很难被爬虫抓取到的
+ 通过服务器让客户端重定向
  1. 状态码设置为302，临时重定向
  2. 在响应头中通过Location告诉客户端往哪儿重定向
  - 如果客户端发现收到服务器的响应的状态码是302，就会自动去响应头中找Location
  - 所以你就能看到客户端自动跳转了
### （三）Node中的模块系统
+ CommonJS模块规范
  - 加载`require`
  
    ```var 自定义变量名 = require('模块')```
    * 执行被加载模块中的代码
    * 得到被加载模块中的exports导出接口对象
  - 导出`exports`
    * Node中是模块作用域，默认文件中所有的成员只在当前文件模块有效
    * 对于希望可以被其它模块访问的成员，我们就需要把这些公开的成员都挂载到`exports`接口对象中就可以了
    * 导出多个成员（必须在对象中）：
      ```
      exports.a = 123
      exports.b = 'hello'
      exports.c = function() {}
      exports.d = {}
      ```
      也可以：```module.exports = {}```
    * 导出单个成员（拿到的就是函数、字符串）
      * 如果一个模块需要直接导出某个成员，而非挂载的方式，必须使用：
      ```module.exports = 'hello'```
      
      ```
      module.exports = {
        add : function() {},
        str: 'hello'
      }
+ 模块原理
  - exports是module.exports的一个引用
    ```exports.foo = 'bar' ```等价于``module.exports.foo = 'bar'
    ```console.log(exports === module.exports) //true```
  - 在 Node 中，每个模块内部都有一个自己的 module 对象
    - 该 module 对象中，有一个成员叫：exports 也是一个对象
    ```js
    // var module = {
    //   exports: {
    //     foo: 'bar',
    //     add: function
    //   }
    // }

    module.exports.foo = 'bar'
    module.exports.add = function (x, y) {
      return x+y
    }
    ```
    - 谁来 require 我，谁就得到 module.exports
    - 默认在代码的最后有一句：
    - return module.exports
  - 也就是说如果你需要对外导出成员，只需要把导出的成员挂载到 module.exports 中
    - 我们发现，每次导出接口成员的时候都通过 module.exports.xxx = xxx 的方式很麻烦，点儿的太多了
    - 所以，Node 为了简化你的操作，专门提供了一个变量：exports 等于 module.exports，也就是说在模块中还有这么一句代码```var exports = module.exports```
    - 两者一致，那就说明，我可以使用任意一方来导出内部成员
  - 当一个模块需要导出单个成员的时候，直接给 exports 赋值是不管用的
    - 记住：最后 return 的是 module.exports 而不是 exports，所以你给 exports 重新赋值不管用
    - 给 exports 赋值会断开和 module.exports 之间的引用，同理，给 module.exports 重新赋值也会断开，导致 exports !== module.exports
  - ```
    // 这里导致exports !== module.exports
    module.exports = {
      foo: 'bar'
    }
    
    // 但是这里又重新建立两者的引用关系
    exports = module.exports
    exports.foo = 'hello'  // hello
    ```
+ require方法加载规则
  - 优先从缓存加载
    * 可以拿到里面的接口对象，但是不会重复执行里面的代码
    * 避免重复加载，提高模块加载效率
  - 判断模块标识
    * require('模块标识')
    * 核心模块
      * 核心模块的本质也是文件，核心模块文件已经被编译到了二进制文件中了，我们只需要按照名字来加载就可以了
    * 第三方模块
      * 凡是第三方模块都必须通过 npm 来下载
      * 使用的时候就可以通过require('包名')的方式来进行加载才可以使用
        * 先找到当前文件所处目录中的node_modules目录
        * node_modules/art-template/package.json文件
        * node_modules/art-template/package.json文件中的main属性
        * main属性中就记录了art-template的入口模块
        * 如果package.json文件不存在或者main指定的入口模块也没有
        * 则node会自动找该目录下的index.js，也就是说index.js会作为一个默认备选项
        * 如果以上所有任何一个条件都不成立，则会进入上一级目录中的node_modules目录查找
        * 如果上一级还没有，则继续往上上一级查找
        * 如果直到当前磁盘根目录还找不到，最后报错
    * 自己写的模块
+ npm
  - node package manager
  - [npmjs](https://www.npmjs.com/)
  - 命令行工具，只要你安装了node就已经安装了npm
  - 版本：`npm --version`
  - 升级npm（自己升级自己）：`npm install --global npm`
  - 常用命令
    * npm init 
      * npm init -y 可以跳过向导，快速生成
    * npm install 
      * npm install 包名
    * npm uninstall 包名
    * npm help 查看使用帮助
      * npm 命令 --help 查看指定命令的使用帮助
+ package.json
  - 包描述文件
  - 这个文件可以通过`npm init`的方式自动初始化出来
  - `dependencies`选项可以用来帮我们保存第三方包的依赖信息
+ package-lock.json（npm5以后的版本）
  - 当你安装包的时候，会自动创建或者更新这个文件
  - 这个文件会保存`node_modules`中所有包的信息（版本、下载地址）
    - 重新`npm install`的时候速度就可以提升 
    - 'lock'可以锁定版本号，防止自动升级新版
### （四）Express
#### 1. Hello World
+ ```
  const express = require("express")
  const app = express()
  app.get("/", function (req, res) {
    res.send('hello express')  
  })
  app.listen(3000, function () {
    console.log("running...")
  })
  ```
#### 2. 基本路由
+ get
  ```
  app.get("/about", function (req, res) {
    // 在Express中可以直接通过req.query来获取查询字符串参数
    console.log(req.query)
    res.send('你好')  
  })
  ```
+ post
  ```
  app.post("/", function (req, res) {
    res.send('hello express')  
  })
  ```
+ 静态资源
  ```
  // 公开指定目录
  // 当以 /public/ 开头的时候，去 ./public/ 目录中找对应的资源
  app.use('/public/', express.static('./public/'))
  
  // 当省略第一个参数的时候，则需要通过省略/public的方式来访问
  app.use(express.static('./public/'))
  
  // 必须是/a/...
  app.use('/a/', express.static('./public/'))
  ```
### 3. 在express中配置使用art-template
+ 安装
  ```
  npm install art-template
  npm install express-art-template
  ```
+ 配置
  ```
  // 配置使用art-template模板引擎
  // 第一个参数表示当渲染以.art结尾的文件的时候，使用art-template模板引擎
  // express-art-template是专门用来在express中把art-template整合到express中
  // 虽然这里不需要加载art-template，但是也必须安装
  // 原因就在于express-art-template依赖了art-template
  app.engine('html', require('express-art-template'))
  ```
+ 使用
  ```
  // express为response响应对象提供了一个方法：render
  // render方法默认是不可以使用，但是如果配置了模板引擎就可以使用了
  // res.render('html模板名', {模板数据})
  // 第一个参数不能写路径，默认会去项目中的views目录查找该模板文件
  // 也就是说express有一个约定：开发人员把所有的视图文件都放到views目录中

  // 如果想要修改默认的views目录，可以
  // app.set('views', render函数的默认路径)

  app.get('/', (req, res) => {
    res.render('index.html', {
      title: '管理系统'
    })
  })
  ```
#### 3. 路由设计
+ router.js路由模块
  ```
  // Express 提供了一种更好的方式专门用来包装路由
  var express = require('express')
  // 1. 创建一个路由容器
  var router = express.Router()
  // 2. 把路由都挂载到 router 路由容器中
  router.get()
  // 3. 把 router 导出
  module.exports = router
  ```
+ ```app.js
  var router = require('./router')
  // 配置模板引擎和 body-parser 一定要在 app.use(router) 挂载路由之前
  
  // 把路由容器挂载到 app 服务中
  app.use(router)
  ```
+ 封装异步API
  - 回调函数：获取异步操作的结果
  ```js
  function fn(callback) {
   // var callback = function (data) { console.log(data) }

    setTimeout(() => {
      var data = 'hello'
      callback(data)
    }, 1000)
  }
  
  // 如果需要获取一个函数中异步操作的结果，则必须通过回调函数来获取
  fn(function(data) {
    console.log(data)
  })
  ```
### （五）Promise
#### 1. 回调地狱
+ 通过回调嵌套的方式来保证顺序 -> 回调地狱
  ```
  fs.readFile('./data/a.txt', 'utf8', function (err, data) {
  if (err) {
    // return console.log('读取失败')
    // 抛出异常
    //    1. 阻止程序的执行
    //    2. 把错误消息打印到控制台
    throw err
  }
  console.log(data)
  fs.readFile('./data/b.txt', 'utf8', function (err, data) {
    if (err) {
      throw err
    }
    console.log(data)
    fs.readFile('./data/c.txt', 'utf8', function (err, data) {
      if (err) {
        throw err
      }
      console.log(data)
    })
  })
  ```
#### 2. Promise
+ Promise是一个构造函数
+ 基础用法
  ```
  // 创建 Promise 容器
  // Promise 容器一旦创建，就开始执行里面的代码
  var p1 = new Promise(function (resolve, reject) {
    fs.readFile('./data/a.txt', 'utf8', function (err, data) {
      if (err) {
        // 把容器的 Pending 状态变为 Rejected
        // 调用了 reject 就相当于调用了 then 方法的第二个参数函数
        reject(err)
      } else {
        // 把容器的 Pending 状态改为成功 Resolved
        // 也就是说这里调用的 resolve 方法实际上就是 then 方法传递的那个function
        resolve(data)
      }
    })
  })
  
  // 当p1成功了，然后（then）做指定的操作
  // then方法接收的 function 就是容器中的 resolve 函数
  p1
  .then(function (data) {
    console.log(data)
  }, function (err) {
    console.log('读取文件失败了', err)
  })
  ```
+ 链式调用
  ```
  p1
  .then(function (data) {
    console.log(data)
    // 当 p1 读取成功的时候，当前函数中 return 的结果就可以在后面的 then 中 function 接收到
    // 当你 return 123 后面就接收到 123，没有 return 后面收到的就是 undefined
    // 重要的是：我们可以 return 一个 Promise 对象
    // 当 return 一个 Promise 对象的时候，后续的 then 中的 方法的第一个参数会作为 p2 的 resolve
    return p2
  }, function (err) {
    console.log('读取文件失败了', err)
  })
  .then(function (data) {
    console.log(data)
    return p3
  })
  .then(function (data) {
    console.log(data)
    console.log('end')
  })
  ```
+ 封装Promise
  ```
  function pReadFile(filePath) {
    return new Promise(function (resolve, reject) {
      fs.readFile(filePath, 'utf8', function (err, data) {
        if (err) {
          reject(err)
        } else {
          resolve(data)
        }
      })
    })
  }

  pReadFile('./data/a.txt')
    .then(function (data) {
      console.log(data)
      return pReadFile('./data/b.txt')
    })
    .then(function (data) {
      console.log(data)
      return pReadFile('./data/c.txt')
    })
    .then(function (data) {
      console.log(data)
    })
  ```
+ Promise-ajax封装
  ```
    function pGet(url, callback) {
      return new Promise(function (resolve, reject) {
        var oReq = new XMLHttpRequest()
        oReq.onload = function () {
          callback && callback(JSON.parse(oReq.responseText))
          resolve(JSON.parse(oReq.responseText))
        }
        oReq.onerror = function (err) {
          reject(err)
        }
        oReq.open("get", url, true)
        oReq.send()
      })
    }
    
    // pGet(' ', function (data) {
    //   console.log(data)
    // })

    pGet(' ')
      .then(function (data) {
        console.log(data)
      })
  ```
