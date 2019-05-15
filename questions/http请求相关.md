## http请求全过程

* 打开浏览器，输入网址 
* 开始dns域名解析
* 浏览器会的域名对应的ip地址后 发起http的三次握手
* 浏览器给web服务器发送一个http请求
* 请求过程
> #### 请求行
> 分为请求方法、url、http协议三个字段 如 GET /index.html HTTP/1.1  
get：一般用于查询/获取数据 如网页上的链接、地址栏中的网址
url？后是携带的数据，一般大小不超过1024个字符
post：一般用于form表单提交数据 传输的数据量大，键值对类型
head、put、delete等不常用
> #### 请求头
> 键值对出现 典型的请求头：
User-Agent  浏览器类型
Accept 客户端可以接受的内容类型   (text/html,application/xml,*/*)
	Host 主机名
	Accept-Language 客户端可接受的自然语言
	Accept-Encoding 客户端可接受的编码压缩格式
	Cookie 存储在客户端的扩展字段
空行 结束消息 
请求体 form data一般出现在post请求中
b>响应过程
服务器接收到数据后解析数据信息，然后发送相应的数据给客户端
状态码  1xx 信息 2xx 成功 3xx 重定向 4xx 客户端错误 5xx 服务器错误
   响应头
	   Server 服务器信息
		Vary  指示不可缓存的请求头列表
       Connection  连接方式 close keep-alive
       Tansfer-Encoding: chunked  分块传输
       Content-Type 返回的数据类型格式
text/html,text/plain,image/png,image/jpeg,application/json,
application/x-www-form-urlencoded,application/form-data

      空行 结束消息
响应体 响应的数据
          若connection为close 服务器主动关闭tcp连接
如keep-alive 连接会保持一段时间 期间可以继续发送数据
5.客户端解析服务器返回的数据 进行渲染
浏览器对HTML文档进行解析，生成DOM树
加载网页css img js文件等外部资源（也要经过上述过程加载）
css，img 属于异步请求，不阻塞文档加载，
但是js会阻塞文档加载 当js加载 解析执行完毕 文档继续加载
接着css解析 渲染dom 样式
