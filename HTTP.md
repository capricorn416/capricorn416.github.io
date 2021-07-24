# HTTP协议
## 一、原理
### 1. HTTP协议概念及工作流程
+ HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写
+ HTTP协议即按一定规则，向服务器要数据，或发送数据，而服务器按一定规则，回应数据
### 2. HTTP三个要点
+ 无连接
  - 限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间
+ 媒体独立
  - 只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型
+ 无状态
  - 协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快
### 3. 网页请求过程
+ 先请求网页文本，也就是HTML
+ 根据HTML中的指定的地址，请求其他内容，最常见的是：样式表，JavaScript，图片等
### 4. http请求信息和响应信息的格式
#### 请求
  ```
  POST /0.1.php HTTP/1.1
  Host: localhost
  Content-type: application/x-www-form-urlencode
  Content-length: 5
  
  Age=3
  ```
  - 请求行
    * 请求方法
      * GET POST HEAD PUT DELETE TRACE OPTIONS
        * GET：请求指定的页面信息，并返回实体主体
        * POST：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改
        * HEAD：类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头；比如用于确认一个内容还正常存在，不需要返回该内容
        * PUT：从客户端向服务器传送的数据取代指定的文档的内容
	* DELETE：请求服务器删除指定的页面
	* CONNECT：HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器
	* OPTIONS：允许客户端查看服务器的性能；返回服务器可用的请求方法
	* TRACE：回显服务器收到的请求，主要用于测试或诊断；用了代理上网，看看代理有没有修改你的HTTP请求
	* PATCH：是对 PUT 方法的补充，用来对已知资源进行局部更新
      * 这些请求方法虽然HTTP协议里规定的，但WEB SERVER未必允许或支持这些方法
      * POST比GET多了主体信息，且头信息里要标明`content-type: `和`content-length: `
    * 请求路径
      * 结合下面的 Host 就可以拼接出完整的地址
    * 协议版本
  - 请求头信息
    * User-Agent：包含发出请求的用户信息，代表发起访问是什么浏览器
    * Cookie：记录了登录信息，或者上次请求服务端设置的信息
    * 头信息结束后要有一个空行
  - 请求主体信息（可以没有）

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
        * 307 重定向中保持原有的请求数据 
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
## 二、实战
### 1. PHP+socket编程发送http请求
```
<?php

// http请求类的接口
interface Proto {
	// 连接url
	function conn($url);

	// 发送get查询
	function get();

	// 发送post查询
	function post();

	// 关闭连接
	function close();
}


class Http implements Proto {
	const CRLF = "\r\n";
	
	protected $errno = -1;
	protected $errstr = '';
	protected $response = '';

	protected $url = null;
	protected $version = 'HTTP/1.1';
	protected $fh = null;

	protected $line = array();
	protected $header = array();
	protected $body = array();

	public function __construct($url) {
		$this->conn($url);
		$this->setHeader('Host: ' . $this->url['host']);
	}

	// 此方法负责写请求行
	protected function setLine($method) {
		$this->line[0] = $method . ' ' . $this->url['path'] . ' ' . $this->version;
	}
	// 此方法负责写头信息
	protected function setHeader($headerline) {
		$this->header[] = $headerline;
	}
	// 此方法负责写主体信息
	protected function setBody() {
	}

	// 连接url
	public function conn($url) {
		$this->url = parse_url($url);
		// 判断端口
		if(!isset($this->url['port'])) {
			$this->url['port'] = 80;
		}
		$this->fh = fsockopen($this->url['host'], $this->url['port'], $this->errno, $this->errstr, 3);
	}
	// 构造get请求的数据
	public function get() {
		$this->setLine('GET');
		$this->request();
		return $this->response;
	}

	// 构造post查询的数据
	public function post() {
	}

	// 真正请求
	public function request() {
		// 把请求行，头信息，实体信息，放在一个数组里，便于拼接
		$req = array_merge($this->line, $this->header, array(''), $this->body, array(''));
		$req = implode(self::CRLF, $req);
		fwrite($this->fh, $req);
		
		while(!feof($this->fh)) {
			$this->response .= fread($this->fh, 1024);
		}

		$this->close(); // 关闭连接
	}

	// 关闭连接
	public function close() {
	}	

}

$url = 'https://www.baidu.com';
$http = new Http($url);
$http->get();

?>
```
### 
