# HTTP 1.1新功能
HTTP 1.0规定浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接。


## 新增请求头字段
- Host
- Connection:Keep-Alive
- Accept-Language:en-gb,zh-cn
- Vary
- Content-Range 返回的状态码 206(Partial Content),请求资源的某一部分
- Content-Encoding,Accept-Encoding,Transfer-Encoding

## 长连接

- **长连接 Connection:Keep-Alive** 每个单独的网页文件的请求和应答还是需要使用各自的连接
HTTP 1.1支持持久连接，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟。

1. 如何关闭长连接？？
Nginx的 keepalive_timeout 参数配置，工作在Keep-Alive模式保持HTTP的连接

2. 客户端如何判断服务器的数据已经发生完成？
Keep-Alive模式发送玩数据HTTP服务器不会自动断开连接，所有不能再使用返回EOF（-1）来判断。
使用头部字段  Content-Length ,或者是 Transfer-Encoding字段来标识


- **流水线 Pipelining**
HTTP 1.1还允许客户端不用等待上一次请求结果返回，就可以发出下一次请求，但服务器端必须按照接收到客户端请求的先后顺序依次回送响应结果，以保证客户端能够区分出每次请求的响应内容，这样也显著地减少了整个下载过程所需要的时间。

在HTTP1.1中支持请求流水线这样的请求模式。为了说明请求流水线，我们先说下没有请求流水线的持续TCP。在一个网页中有很多其他资源，使用持续TCP连接的非流水线数据传输时，当客户端没有收到上一个http请求的响应报文之前是不会发送下一个http请求的。也就是说客户只在收到前一个请求的响应后才发出新的请求，这样在等到应答的这段时间内线路处于空闲状态会造成资源浪费。

## 参考文章
https://blog.csdn.net/ForgotAboutGirl/article/details/6936982 HTTP1.1 

http://www.nowamagic.net/academy/detail/23350305  HTTP的Keep-Alive 和 TCP的Keep-Alive的理解 如果保持TCP连接