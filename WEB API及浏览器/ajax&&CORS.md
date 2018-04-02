# AJAX通信及跨域

## ajax工作原理
- 1.使用new XMLHttpRequest/open() 来创建网络请求

```javascript
//对于get请求（或凡涉及到url传递参数的），被传递的参数都要先经encodeURIComponent方法处理.
var url = "update.php?username=" +encodeURIComponent(username) + "&content=" +encodeURIComponent(content)+"&id=1" ;
```
- 2.onreadystatechange 绑定状态变化的处理函数

```javascript
  xhr.readyState == 4  //包括状态码0、1、2、3、4
  xhr.status == 200 //请求成功
```
- 3.setRequestHeader("Content-Type","application/x-www-form-urlencoded") 设置请求头

它用来设置请求头的信息，post请求通常会设置前面的Content-Type的请求头，比如说我想跨域发送请求，那你可能就要设置xhr.setRequestHeader("Origin","url")了(要真的跨域还需要在服务器端设置Access-Control-Allow-Origin)。但是并不能设置所有的请求头信息，
你可以设置除了下面这些头之外的信息：
Accept-Charset,Accept-Encoding,Connection,Content-Length,Cookie,Cookie2,Content-Transfer-Encoding,Date,Expect,Host,Keep-Alive,Referrer,User-Agent,Trailer,Transfer-Encoding,Upgrade,Via.

用来查询响应头的信息
xhr.getResponseHeader()
xhr.getAllResponseHeader()

- 4.xmlHttpRequest.send("v1=" + v1 + "&v2=" + v2); 
  发送HTTP请求 注意这里是post方式，发生请求参数。get的方式直接是send(NULL)

- 5.xhr常用属性及事件
  xhr.overrideMimeType('text/plain;charset=x-user-defined') //指定服务器返回的数据的Mime类型
 
  xhr2才支持的属性，在这个属性不支持的时候，使用上面的方法替代
  xhr.responseType  //指定服务器返回的数据的类型
  xhr.responseText
  xhr.responseXML

  新增事件：
  onloadstart
  onprogress
  onabort
  onerror
  onload  请求成功完成时触发
  ontimeout 自定义超时
  onloadend

  ## 跨域

> 可以发送请求，但浏览器会拒绝接受响应

同源政策：协议、域名、端口

CORS 是跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，属于跨源 AJAX 请求的根本解决方法。相比 JSONP 只能发GET请求，CORS 允许任何类型的请求。

1. 简单请求：
Origin:http://api.bob.com
响应：
Access-Control-Allow-Origin: * 表示接收任意域名的请求
Access-Control-Allow-Credentials:true  表示服务器明确许可，Cookie可以包含在请求中，一起发送给服务器端
此时的客户端 **需要设置xhr.withCredentials = true** ,则在CROS的机制下 才发发送cookies

2. 复杂请求：

OPTIONS：告诉WEB服务器 这是一个 "预检"(preflight) 请求
