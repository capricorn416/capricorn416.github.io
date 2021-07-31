# Node.js
## 一、day1
### (一) Node.js介绍
#### 1. Node.js是什么
+ Node.js不是一门语言，不是库、不是框架
+ Node.js是一个JavaScript运行时环境
+ Node.js可以解析和执行JavaScript代码
#### 2. Node.js中的JavaScript
+ 没有BOM、DOM
+ ECMAScript基本的JavaScript语言部分
+ 在Node中为JavaScript提供了一些服务器级别的API
  - 文件操作的能力
  - http服务的能力
#### 3. Node.js的特性
+ event-driven 事件驱动
+ non-blocking I/O model 非阻塞IO模型（异步）
+ lightweight and efficient 轻量和高效
#### 4. npm是世界上最大的开源库生态系统
+ 绝大多数JavaScript相关的包都存放在了npm上
#### 5. Node.js能做什么
+ Web服务器后台
+ 命令行工具

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
  - `fs.readFile`读取文件
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

