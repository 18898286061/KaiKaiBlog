> 要成为一个前端选手，需要好好学习一下计算机网络，否则都不算合格的一枚开发者。记录一下一些学习到的入门级概念。

# WWW 的发明

度过了黑暗时期的网络时代，1990年左右，Tim Berners-Lee （李爵士）发明了WWW（World Wide Web）,一种使用与全世界的网络。

主要包含了三个概念：

1. URI ，俗称网址
2. HTTP， 两个电脑之间传输的协议
3. HTML， 超级文本，主要用来做页面的跳转

URL的作用是能让你访问一个页面，HTTP的作用是让你能下载这个页面，HTML的作用是让你能看懂这个页面。

这是一个简单而完美的系统。

### URI
英文全称Uniform Resource Identifier。   翻译过来是“统一资源标志符”。URI又分为URL和URN，而我们一般使用URL作为网址。

### URN
英文全称Uniform Resource Name。 翻译过来呢就是 “统一资源名称”。

ISBN:9787115275790 就是一个 URN，通过 URN 你可以确定一个「唯一的」资源，ISBN: 9787115275790 对应的资源的是《JavaScript 高级程序设计（第三版）》这本书。你去是介绍任何一个图书馆、书店，他们都知道是这本书。


### URL
英文全称：Uniform Resource Locator。 翻译：统一资源定位符。

https://www.baidu.com/s?wd=hello&rsv_spt=1#5 就是一个 URL，通过 URL 你可以确定一个「唯一的」地址（网址）。

1. `https://` -> 协议
2. `www.baidu.com` -> 域名
3. `/s` -> 路径
4. `wd=hello&rsv_spt=1` ->查询参数
5. `#5` -> 锚点

### DNS
英文全称：Domain Name System  翻译：域名系统

最简单来说，它可以：

- 输入域名
- 输出IP

可以试试在命令行下面两个命令可以输出对应网址IP：

`nslookup` baidu.com
`ping`  baidu.com


# 请求和响应

Server + Client + HTTP

**HTTP全称； HyperText Transfer Protocol   翻译：超文本传输协议**

- 电脑（Server服务器）80端口 ----请求---> 客户端（浏览器）
- 电脑（Server服务器）80端口 <----响应--- 客户端（浏览器）

服务器与浏览器的交互

1. 浏览器负责发起请求
2. 服务器在80端口接收请求（HTTP 的默认服务端口是：80）
3. 服务器负责返回内容（响应）
4. 浏览器负责下载相应内容

HTTP 的作用就是指导浏览器和服务器如何进行沟通。


## 请求

让我们试一下下面这行请求是里

`curl -s -v -H "Kaikai: ya" -- "https://www.baidu.com"`

我们来多试几个不同那个的请求，看看请求文的区别。

请求内容为： 这次是GET请求
```
curl -s -v -H "Kaikai: ya" -- "https://www.baidu.com"

> GET / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.61.1
> Accept: */*
> Kaikai: ya
>
```

请求内容为：这次换成了POST
```
curl -X POST -s -v -H "KaiKai: ya" -- "https://www.baidu.com"

> POST / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.61.1
> Accept: */*
> KaiKai: ya
>

```


请求的内容为：这次POST加了数据1234567890

```
curl -X POST -d "1234567890" -s -v -H "KaiKai: ya" -- "https://www.baidu.com"

> POST / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.61.1
> Accept: */*
> KaiKai: ya
> Content-Length: 10
> Content-Type: application/x-www-form-urlencoded
>
```

### 总结一哈 ， 请求的格式

```
第一部分： 动词 路径 协议/版本

第二部分： Key1: value1
          Key2: value2
          Key3: value3
          Content-Type: application/x-www-form-urlencoded
          Host: www.baidu.com
          User-Agent: curl/7.54.0
          
第三部分： ( 一个回车 )

第四部分： 要上传的数据
```
1. 请求最多包含四部分，最少包含三部分。（也就是说第四部分可以为空）
2. 第三部分永远都是一个回车（\n）
3. 动词有 GET POST PUT PATCH DELETE HEAD OPTIONS 等
4. 这里的路径包括「查询参数」，但不包括「锚点」
5. 如果你没有写路径，那么路径默认为 /
6. 第 2 部分中的 Content-Type 标注了第 4 部分的格式


### 用Chrome发请求
1. 打开 Network
2. 地址栏输入网址
3. 在 Network 点击，查看 request，点击「view source」
4.  **点击「view source」**
5. 如果有请求的第四部分，那么在 FormData 或 Payload 里面可以看到





## 响应

请求了之后，应该都能得到一个响应，除非断网了，或者服务器宕机了。

### 响应示例

上面三个请求示例，前两个请求对应的响应分别为：
```
< HTTP/1.1 200 OK
< Accept-Ranges: bytes
< Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
< Connection: Keep-Alive
< Content-Length: 2443
< Content-Type: text/html
< Date: Mon, 19 Nov 2018 13:57:24 GMT
< Etag: "58860429-98b"
< Last-Modified: Mon, 23 Jan 2017 13:24:57 GMT
< Pragma: no-cache
< Server: bfe/1.0.8.18
< Set-Cookie: BDORZ=27315; max-age=86400; domain=.baidu.com; path=/
<

<!DOCTYPE html>
<!--STATUS OK--><html> <head> 后面太长，省略
```



```
< HTTP/1.1 302 Found
< Connection: Keep-Alive
< Content-Length: 17931
< Content-Type: text/html
< Date: Mon, 19 Nov 2018 13:59:21 GMT
< Etag: "54d9748e-460b"
< Server: bfe/1.0.8.18
<

<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8"> 后面太长，省略了……
```
### 总结一下响应的格式：

1. GET 请求和 POST 请求对应的响应可以一样，也可以不一样
2. 响应的第四部分可以很长很长很长

1 协议/版本号 状态码 状态解释
2 Key1: value1
2 Key2: value2
2 Content-Length: 17931
2 Content-Type: text/html
3
4 要下载的内容

- 状态码要背，是服务器对浏览器说的话
  - 1xx 不常用
  - 2xx 表示成功
  - 3xx 表示滚吧
  - 4xx 表示你丫错了
  - 5xx 表示好吧，我错了
- 状态解释没什么用
- 第 2 部分中的 Content-Type 标注了第 4 部分的格式
- 第 2 部分中的 Content-Type 遵循 MIME 规范

### 下面重点记一下几个比较常见的状态码：
（引用自维基百科）

**200 OK**：请求已成功，请求所希望的响应头或数据体将随此响应返回。实际的响应将取决于所使用的请求方法。在GET请求中，响应将包含与请求的资源相对应的实体。在POST请求中，响应将包含描述或操作结果的实体。

**301 Moved Permanently**：被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个URI之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。[19]除非额外指定，否则这个响应也是可缓存的。
新的永久性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。
如果这不是一个GET或者HEAD请求，那么浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。
注意：对于某些使用HTTP/1.0协议的浏览器，当它们发送的POST请求得到了一个301响应的话，接下来的重定向请求将会变成GET方式。

**302 Found**：要求客户端执行临时重定向（原始描述短语为“Moved Temporarily”）。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。
新的临时性的URI应当在响应的Location域中返回。除非这是一个HEAD请求，否则响应的实体中应当包含指向新的URI的超链接及简短说明。
如果这不是一个GET或者HEAD请求，那么浏览器禁止自动进行重定向，除非得到用户的确认，因为请求的条件可能因此发生变化。
注意：虽然RFC 1945和RFC 2068规范不允许客户端在重定向时改变请求的方法，但是很多现存的浏览器将302响应视作为303响应，并且使用GET方式访问在Location中规定的URI，而无视原先请求的方法。因此状态码303和307被添加了进来，用以明确服务器期待客户端进行何种反应。

**403 Forbidden**：服务器已经理解请求，但是拒绝执行它。与401响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。如果这不是一个HEAD请求，而且服务器希望能够讲清楚为何请求不能被执行，那么就应该在实体内描述拒绝的原因。当然服务器也可以返回一个404响应，假如它不希望让客户端获得任何信息。

**404 Not Found**：请求失败，请求所希望得到的资源未被在服务器上发现，但允许用户的后续请求。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。


**500 Internal Server Error**：通用错误消息，服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。没有给出具体错误信息。


**502 Bad Gateway**：作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。


维基百科HTTP状态码：https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81

### 用 Chrome 查看响应
1. 打开 Network
2. 输入网址
3. 选中第一个响应
4. 查看 Response Headers，**点击「view source」**。
5. 你会看到响应的前两部分
6. 查看 Response 或者 Preview，你会看到响应的第 4 部分





# HTML

英文全称：HyperText Markup Language ； 翻译： 超文本标记语言

当响应的第二部分有 Content-Type: text/html 而且响应的第四部分是 HTML 文本时，你就可以在浏览器看到网页了。

最初李爵士首次公布的关于 HTML 的想法：

只有如下几个标签：
```
<TITLE> ... </TITLE>
<NEXTID 27>
<A NAME=xxx HREF=XXX> ... </A>
<ISINDEX>
<PLAINTEXT>
<LISTING>
<P>
<H1>, <H2>, <H3>, <H4>, <H5>, <H6>
<ADDRESS> text ... </ADDRESS>
<HP1>...</HP1>   <HP2>... </HP2> 
<DL>
    <DT>Term<DD>definition pagagraph
    <DT>Term2<DD>Definition of term2
</DL>
<UL>
    <LI> list element
    <LI> another list element ...
</UL>
```
延续至今的标签有 title a p h1~h6 dl ul。

HTML 一开始的意图只是用来写文章和页面跳转，没想到现在的开发者已经用 HTML 做一切东西了。

以下软件都在使用 HTML 做界面：

手机微信、手机QQ、PC微信、PC QQ、钉钉、淘宝、支付宝、美团……

所有 App 都会内置一个浏览器（WebView）用来展示 HTML，而 HTML 都是通过 HTTP 下载的，而如果你要使用 HTTP 一般都会用到 URL。

这是一个简单而完美的系统。













