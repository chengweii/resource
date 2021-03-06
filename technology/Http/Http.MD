# Http
HTTP协议(Hyper Text Transfer Protocol[超文本传输协议]),是用于从万维网(WWW:World Wide Web)服务器传输超文本(HTML、图片、文件等)到本地浏览器的传送协议。  
HTTP基于TCP/IP通信协议；属于应用层的面向对象的协议。  
HTTP协议工作于客户端-服务端架构上，一般浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。Web服务器根据接收到的请求后，向客户端发送响应信息。
HTTP协议的主要特点是通信简单快速、传输内容灵活、无连接(请求响应完成立即断开)、无状态（任意两次通信无关联）、支持B/S及C/S模式。

## URL
HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。URL是一种特殊类型的URI，包含了用于查找某个资源的足够的信息URL,全称是Uniform Resource Locator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址。URL的格式如下：  
> http://{域名|IP}[:端口号][/虚拟目录][/文件名][?参数][#锚]  
例如:http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name

一个完整的URL包括协议部分(必选)、域名部分(必选)、端口部分(必选,不输默认80)、虚拟目录部分(可选)、文件名部分(可选)、参数部分(可选)、锚部分(可选)。以下为各个部分的说明：
>1.协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符。  
2.域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用。  
3.端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口80。  
4.虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”。  
5.文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名。  
6.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分。  
7.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。  

## URI、URN、URL
* URI，uniform resource identifier，统一资源标识符，用来唯一的标识一个资源。
* URN，uniform resource name，统一资源命名，是通过名字来标识资源，比如mailto:java-net@java.sun.com。
* URL，uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。

>URI是以一种抽象的，高层次概念定义统一资源标识，而URL和URN则是具体的资源标识的方式。URL和URN都是一种URI。笼统地说，每个 URL 都是 URI，但不一定每个 URI 都是 URL。这是因为 URI 还包括一个子类，即统一资源名称 (URN)，它命名资源但不指定如何定位资源。

## HTTP Headers
HTTP的头域包括通用头，请求头，响应头和实体头四个部分。
* 通用头(General header)  
通用头即可以包含在HTTP请求中，也可以包含在HTTP响应中。通用头的作用是描述HTTP协议本身。比如描述HTTP是否持久连接的Connection头，HTTP发送日期的Date头，描述HTTP所在TCP连接时间的Keep-Alive头,用于缓存控制的Cache-Control头等。
* 实体头(Entity header)  
实体头是那些描述HTTP信息的头。既可以出现在HTTP POST方法的请求中，也可以出现在HTTP响应中。比如图5和图6中的Content-Type和Content-length都是描述实体的类型和大小的头都属于实体头。其它还有用于描述实体的Content-Language,Content-MD5,Content-Encoding以及控制实体缓存的Expires和Last-Modifies头等。
* 请求头(HTTP Request Header)  
请求头是那些由客户端发往服务端以便帮助服务端更好的满足客户端请求的头。请求头只能出现在HTTP请求中。比如告诉服务器只接收某种响应内容的Accept头，发送Cookies的Cookie头，显示请求主机域的HOST头,用于缓存的If-Match，If-Match-Since,If-None-Match头，用于只取HTTP响应信息中部分信息的Range头，用于附属HTML相关请求引用的Referer头等。
* 响应头(HTTP Response Header)  
HTTP响应头是那些描述HTTP响应本身的头，这里面并不包含描述HTTP响应中第三部分也就是HTTP信息的头（这部分由实体头负责）。比如说定时刷新的Refresh头，当遇到503错误时自动重试的Retry-After头，显示服务器信息的Server头，设置COOKIE的Set-Cookie头，告诉客户端可以部分请求的Accept-Ranges头等。

## HTTP Request
客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头（header）、空行和请求实体四个部分组成。例如：

```
// 请求行格式：Method Request-URI HTTP/Version
// Method 表示对于Request-URI完成的方法，这个字段是大小写敏感的，包括OPTIONS、GET、HEAD、POST、PUT、DELETE、 TRACE。
// 方法GET和HEAD应该被所有的通用WEB服务器支持，其他所有方法的实现是可选的。
// GET方法取回由Request-URI标识的信息。 HEAD方法也是取回由Request-URI标识的信息，只是可以在响应时，不返回消息体。
// POST方法可以请求服务器接收包含在请求中的实体信息，可 以用于提交表单，向新闻组、BBS、邮件群组和数据库发送消息。 
POST /tags/maven HTTP/1.1
// 请求头：紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息。
Host: mvnrepository.com
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.104 Safari/537.36 Core/1.53.3176.400 QQBrowser/9.6.11541.400
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding: gzip, deflate, sdch
Accept-Language: zh-CN,zh;q=0.8
Cookie: __qca=P0-252788103-1481513590432; mwLocalGuidCookieName=24311387-cafe-4281-829f-b599fcc23e44;
// 请求头部中的其他参数，例如：Content-Length、Content-Typed等，当然也可以自己定义参数，例如：My-Params:test-value。
...:...
// 空行

// 请求实体：也叫主体，可以添加任意的其他数据。
name=Professional%20Ajax&publisher=Wiley
```
* 请求行是请求方法，GET和POST是最常见的HTTP方法，除此以外还包括DELETE、HEAD、OPTIONS、PUT、TRACE。不过，当前的大多数浏览器只支持GET和POST，Spring 3.0提供了一个HiddenHttpMethodFilter，允许你通过“_method”的表单参数指定这些特殊的HTTP方法（实际上还是通过POST提交表单）。服务端配置了HiddenHttpMethodFilter后，Spring会根据_method参数指定的值模拟出相应的HTTP方法，这样，就可以使用这些HTTP方法对处理方法进行映射了。 
* 请求头是HTTP的报文头，报文头包含若干个属性，格式为“属性名:属性值”，服务端据此获取客户端的信息（附属信息、连接指令等）以便更好的满足客户端请求。
* 请求实体是报文体，它承载具体请求参数的数据（参数、文件等）。

### HTTP Request Headers
客户端向服务器端传递请求的附加信息以及客户端自身的信息存放在请求报头，以便服务端更好的满足客户端请求。
* Cache-Control：浏览器应遵循的缓存机制。
> no-cache：不要缓存的实体，要求现在从WEB服务器去取。  
max-age：只接受 Age 值小于 max-age 值，并且没有过期的对象。  
max-stale：可以接受过去的对象，但是过期时间必须小于 max-stale 值。  
min-fresh：接受其新鲜生命期大于其当前 Age 跟 min-fresh 值之和的缓存对象。

* Pramga：主要使用 Pramga: no-cache，相当于 Cache-Control： no-cache。
* Connection：表示是否需要持久连接。
> close：告诉WEB服务器或者代理服务器，在完成本次请求的响应后，断开连接，不要等待本次连接的后续请求了。  
keep-alive：告诉WEB服务器或者代理服务器，在完成本次请求的响应后，保持连接，等待本次连接的后续请求。

* Accept：  告诉WEB服务器自己接受什么介质类型，*/* 表示任何类型，type/* 表示该类型下的所有子类型；
* Accept-Charset：  浏览器申明自己接收的字符集
* Accept-Encoding：浏览器申明自己接收的编码方法，通常指定压缩方法，是否支持压缩，支持什么压缩方法  （gzip，deflate）
* Accept-Language：  浏览器申明自己接收的语言。语言跟字符集的区别：中文是语言，中文有多种字符集，比如big5，gb2312，gbk等等。
* Authorization：  当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，该头部来回应自己的身份验证信息给WEB服务器。

* Referer：发送请求页面URL。浏览器向 WEB 服务器表明自己是从哪个 网页/URL 获得/点击 当前请求中的网址/URL。
* User-Agent: 浏览器表明自己的身份（是哪种浏览器）。
* Host： 发送请求页面所在域。
* Range：浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。
* Form：一种请求头标，给定控制用户代理的人工用户的电子邮件地址。
* Cookie：这是最重要的请求头信息之一

## HTTP Response
在服务端接收和解释请求消息后，会返回一个HTTP响应消息。HTTP响应也是由三个部分组成，分别是：状态行、响应头、响应实体。例如：

```
// 状态行格式：HTTP-Version Status-Code Reason-Phrase
// HTTP -Version表示支持的HTTP版本，例如为HTTP/1.1。Status- Code是一个三个// 数字的结果代码。Reason-Phrase给Status-Code提供一个简单的文本描述。
// Status-Code主要用于机器自动识别，Reason-Phrase主要用于帮助用户理解。
HTTP/1.0 200 OK 
Date:Mon,31Dec200104:25:57GMT 
Server:Apache/1.3.14(Unix) 
Content-type:text/html 
Last-modified:Tue,17Apr200106:46:28GMT 
Etag:"a030f020ac7c01:1e9f" 
Content-length:39725426 
Content-range:bytes554554-40279979/40279980 
```
### HTTP Response Status-Code
Status-Code的第一个数字定义响应的类别，后两个数字没有分类的作用。第一个数字可能取5个不同的值：  
* 1xx：信息响应类，表示接收到请求并且继续处理  
    * 100	Continue	继续。客户端应继续其请求
    * 101	Switching Protocols	切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议
* 2xx：处理成功响应类，表示动作被成功接收、理解和接受
    * 200	OK	请求成功。一般用于GET与POST请求
    * 201	Created	已创建。成功请求并创建了新的资源
    * 202	Accepted	已接受。已经接受请求，但未处理完成
    * 203	Non-Authoritative Information	非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本
    * 204	No Content	无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档
    * 205	Reset Content	重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域
206	Partial Content	部分内容。服务器成功处理了部分GET请求
* 3xx：重定向响应类，为了完成指定的动作，必须接受进一步处理 
    * 300	Multiple Choices	多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择
    * 301	Moved Permanently	永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替
    * 302	Found	临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI
    * 303	See Other	查看其它地址。与301类似。使用GET和POST请求查看
    * 304	Not Modified	未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源
    * 305	Use Proxy	使用代理。所请求的资源必须通过代理访问
    * 306	Unused	已经被废弃的HTTP状态码
    * 307	Temporary Redirect	临时重定向。与302类似。使用GET请求重定向
* 4xx：客户端错误，客户请求包含语法错误或者是不能正确执行
    * 400	Bad Request	客户端请求的语法错误，服务器无法理解
    * 401	Unauthorized	请求要求用户的身份认证
    * 402	Payment Required	保留，将来使用
    * 403	Forbidden	服务器理解请求客户端的请求，但是拒绝执行此请求
    * 404	Not Found	服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面
    * 405	Method Not Allowed	客户端请求中的方法被禁止
    * 406	Not Acceptable	服务器无法根据客户端请求的内容特性完成请求
    * 407	Proxy Authentication Required	请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权
    * 408	Request Time-out	服务器等待客户端发送的请求时间过长，超时
    * 409	Conflict	服务器完成客户端的PUT请求是可能返回此代码，服务器处理请求时发生了冲突
    * 410	Gone	客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置
    * 411	Length Required	服务器无法处理客户端发送的不带Content-Length的请求信息
    * 412	Precondition Failed	客户端请求信息的先决条件错误
    * 413	Request Entity Too Large	由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息
    * 414	Request-URI Too Large	请求的URI过长（URI通常为网址），服务器无法处理
    * 415	Unsupported Media Type	服务器无法处理请求附带的媒体格式
    * 416	Requested range not satisfiable	客户端请求的范围无效
    * 417	Expectation Failed	服务器无法满足Expect的请求头信息
* 5xx：服务端错误，服务器不能正确执行一个正确的请求
    * 500	Internal Server Error	服务器内部错误，无法完成请求
    * 501	Not Implemented	服务器不支持请求的功能，无法完成请求
    * 502	Bad Gateway	充当网关或代理的服务器，从远端服务器接收到了一个无效的请求
    * 503	Service Unavailable	由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中
    * 504	Gateway Time-out	充当网关或代理的服务器，未及时从远端服务器获取请求
    * 505	HTTP Version not supported	服务器不支持请求的HTTP协议的版本，无法完成处理
### HTTP Response Headers
响应报头允许服务器传递不能放在状态行中的附加响应信息，以及关于服务器的信息和对Request-URI所标识的资源进行下一步访问的信息。
* Allow：服务器支持哪些请求方法（如GET、POST等）。
* Content-Encoding：文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。
* Content-Length：表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容。
* Content-Type：表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。
* Date：当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。
* Expires：应该在什么时候认为文档已经过期，从而不再缓存它？
* Last-Modified：文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。
* Location：表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。
* Refresh：表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 
注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。 
注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。 
注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。
* Server：服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。
* Set-Cookie：设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。
* WWW-Authenticate：客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 
注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。

## 会话保持
## 缓存
## 持久连接
## 安全(XSS、CSRF)
## 分块传输(断点续传)
# 参考文献
[关于HTTP协议，一篇就够了](http://www.cnblogs.com/ranyonsue/p/5984001.html)  
[为什么所有主要浏览器的 User-Agent 都是 Mozilla/x.0 开头？](https://www.zhihu.com/question/19553117)  
[HTTP请求行、请求头、请求体详解](http://blog.csdn.net/u010256388/article/details/68491509)  
[https比http更安全吗？为什么大多数网站还是使用http？](https://www.zhihu.com/question/25119364)  
[Web攻防之XSS,CSRF,SQL注入](http://www.cnblogs.com/drawwindows/archive/2013/03/11/2954259.html)  
[HTTP断点续传（分块传输）（HTTP头格式非常清楚）](http://www.cnblogs.com/findumars/p/5745345.html)  