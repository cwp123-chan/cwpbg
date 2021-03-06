---
title: http与https的区别
date: 2019-08-27 12:46:49
---
# http与https的区别

## 1.  Http与Https的基本概念和他们的区别

```
http的中文叫做超文本传输协议,它负责完成客户端到服务端的一系列操作,是专门用来传输注入HTML的超媒体文档等web内容的协议,它是基于传输层的TCP协议的应用层协议

https:https是基于安全套接字的http协议,也可以理解为是http+ssl/tls(数字证书)的组合

http和https的区别:

HTTP 的 URL 以 http:// 开头，而 HTTPS 的 URL 以 https:// 开头
HTTP 是不安全的，而 HTTPS 是安全的
HTTP 标准端口是 80 ，而 HTTPS 的标准端口是 443
在 OSI 网络模型中，HTTPS的加密是在传输层完成的,因为SSL是位于传输层的,TLS的前身是SSL,所以同理
HTTP无需认证证书,而https需要认证证书 

```

## 2. HTTPS工作原理

```
首先服务端给客户端传输证书,这个证书就是公钥,只是包含了很多的信息,比如说证书的办法机构,证书的过期时间
客户端进行证书的解析,比如说验证办法机构,过期时间,如果发现没有任何问题,就生成一个随机值(私钥),然后用证书对这个私钥进行加密,并发送给服务端
服务端使用私钥将这个信息进行解密,得到客户端的私钥,然后客户端和服务端就可以通过这个私钥进行通信了
服务端将消息进行对称加密(简单来说就是讲消息和私钥进行混合,除非知道私钥否则服务进行解密),私钥正好只有客户端和服务端知道,所以信息就比较安全了
服务端将进行对称加密后的消息进行传送
客户端使用私钥进行信息的解密

```

## 3. GET方法与POST方法的区别,什么时候应该使用GET什么时候应该使用POST

```
GET：一般用于信息获取，使用URL传递参数，对所发送信息的数量也有限制，一般在2000个字符
POST：一般用于修改服务器上的资源，对所发送的信息数量没有限制
GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值，也就是说Get是通过地址栏来传值，而Post是通过提交表单来传值。

```

## 4. HTTP的缺点与HTTPS有哪些改进

### HTTP优化:

```
资源内敛 : 资源内联 : 既然每个资源的首次访问都会存在握手等rtt损耗,那么越少数量的资源请求就越好,例如咋一个html中src访问css,不如直接将其这个css集成到html中
图片懒加载 ; 用到的时候在加载,这个已经很普遍了,就不细说了
服务器渲染 : 让服务端先将页面渲染好,在发送给客户端,也可以减少rtt的次数

```

## 5. 一次完整的HTTP请求与响应都发生了什么

```
1.在浏览器端输入网站的url地址
只有知道了一个网站的url地址才能访问到这个网站

2.浏览器查找缓存
浏览器会查找浏览器缓存,系统缓存,路由缓存,如果没有的话 继续下一步,如果有的话,直接显示

注意:浏览器会把访问过得web网站资源(html 图片)缓存起来,而判断是否使用缓存的条件有以下几种:

是否有这个网站的缓存
这个网站的缓存是否过期,具体看Cache-Control 中缓存的有效时间
跟服务器进行协商是否使用缓存,如果上次缓存的时候有Last-modified 和 Etag 字段,本次请求就会加上If-Modified-Since(上次请求资源的时间)和If-None-Match(上次资源的修改时间)
3.通过DNS获取url对应的ip地址
现在本机的host文件中查找是否有这个url对应的ip,如果没有的话,就请求DNS进行ip地址的获取

4.建立TCP链接
http在工作之前,需要客户端和服务端建立链接,这个链接的建立是通过tcp(三次握手)来完成的,因为http是比tcp更高层的协议,在网络协议的建立中,不谈底层谈高层都是在耍流氓,所以想要让http进行工作,需要tcp首先建立链接

 

5.浏览器向web服务器发送请求
一旦链接已经建立,浏览器就可以给web服务器发送请求命令,比如 : GET/deom/hello.jsp HTTP/1.1

 

6.浏览器给web服务器发送请求头信息
浏览器在发送了请求后,还要给web服务器请求头信息,比如accept-charset(浏览器端指定的字符集),最后发送一个空的请求头代表请求发送完毕,注意:如果是post提交,则会继续提交请求体

 

7.web服务器进行应答
应答的第一部分是http版本号,第二部分是协议的状态码,比如:HTTP/1.1 200 OK

 

8.web服务器发送应答头消息
web服务器给浏览器发送应答头消息,也就是关于web服务器自己的信息,最后发送一个空白行代表应答结束

 

9.web服务器发送数据
以应答头里面的content-type所描述的格式发送数据

 

10.web服务器关闭链接
web服务器向浏览器发送了应答数据之后,就要关闭tcp链接(tcp四次握手关闭链接),如果添加了connection:keep-alive,那么就还会保持链接状态

```

