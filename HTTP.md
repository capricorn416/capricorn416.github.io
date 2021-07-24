# HTTP协议
## 一、HTTP协议概念及工作流程
### 1. 什么是http协议
+ http协议即按一定规则，向服务器要数据，或发送数据，而服务器按一定规则，回应数据
### 2. http请求信息和响应信息的格式
#### 请求
  - 请求行
    * 请求方法
      * GET POST HEAD PUT DELETE TRACE OPTIONS
      * POST比GET多了主体信息，且头信息里要标明`content-type: `和`content-length: `
      * HEAD和GET基本一致，只是不返回内容，比如用于确认一个内容还正常存在，不需要返回该内容
      * TRACE：用了代理上网，看看代理有没有修改你的HTTP请求
      * OPTIONS：返回服务器可用的请求方法
      * 这些请求方法虽然HTTP协议里规定的，但WEB SERVER未必允许或支持这些方法
    * 请求路径
    * 所用的协议
  - 请求头信息
    * 头信息结束后要有一个空行
  - 请求主体信息（可以没有）
  ```
  POST /0.1.php HTTP/1.1
  Host: localhost
  Content-type: application/x-www-form-urlencode
  Content-length: 5
  
  Age=3
  ```
#### 响应
  - 响应行
    * 协议版本
    * 状态码 
      * 用来反应服务器响应情况的
      * 1XX 信息 接收到请求，继续处理
      * 2XX 成功 操作成功地收到，理解和接受
        * 200 服务器成功返回网页
      * 3XX 重定向 为了完成请求，必须采取进一步措施
        * 301/2 永久/临时重定向
        * 304 未修改
      * 4XX 客户端错误 请求的语法有错误或不能完全被满足
        * 404 请求的网页不存在
      * 5XX 服务端错误 服务器无法完成明显有效的请求
        * 503 服务器暂时不可用
        * 500 服务器内部错误
    * 状态文字
      * 用来描述状态码，便于人观察 
  - 响应头信息
    * 格式为 key: value
  - 响应主体信息（可以没有）
  ```
  HTTP/1.1 200 OK
  Content-type: text/html
  Content-length: 5
  
  hello
  ```
