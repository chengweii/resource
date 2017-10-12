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

## HTTP Request
客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成。例如：

```
// 请求行：以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本。
POST /tags/maven HTTP/1.1
// 请求头部：紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息。
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

// 请求数据：也叫主体，可以添加任意的其他数据。
name=Professional%20Ajax&publisher=Wiley
```

###  
# 参考文献
[关于HTTP协议，一篇就够了](http://www.cnblogs.com/ranyonsue/p/5984001.html)  
[为什么所有主要浏览器的 User-Agent 都是 Mozilla/x.0 开头？](https://www.zhihu.com/question/19553117)  