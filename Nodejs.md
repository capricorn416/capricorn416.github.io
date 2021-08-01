# Node.js
## 一、day1
### (一) Node.js介绍
#### 1. Node.js是什么
+ Node.js不是一门语言，不是库、不是框架
+ Node.js是一个JavaScript运行时环境
+ Node.js可以解析和执行JavaScript代码
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
#### 3. http服务
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
      * 
+ 可以同时开启多个服务，但一定要确保不同服务占用的端口号不一致。在一台计算机中，同一个端口号同一时间只能被一个程序占用  
