> 写此文为记录之前学习了解到的服务端皮毛，了解服务端的大体工作原理也是前端攻城狮的必修课呀。

先简单的起个服务，不会没关系。网络上已经有人写好了一个不错的，取名叫做 http-server。

通过以下方法来使用：

1. `mkdir http-demo`
2. `touch xxx.html`
3. `npm install -g http-server`
4. `http-server . -c-1`

这时候你在浏览器上访问 http://localhost:8080/xxx.html就可以来预览你刚刚touch出来的 html 了。


Windows 上 http-server 的缓存「有可能」无法消除，如果你发现你改了页面，页面却无法更新，那么就这样做：

1. 打开 Chrome 开发者工具
2. 点击 Network
3. 勾选 Disable Cache
4. 这样缓存就不存在了~

# 网络与IP
> HTTP 协议的底层其实是由 TCP 协议和 IP 协议（简称 TCP/IP）构建的。

## TCP 传输控制协议（Transmission Control Protocol）

1. TCP 和 UDP 的区别是什么

简答：TCP 可靠、面向连接、相对 UDP 较慢；UDP 不可靠，不面向连接、相对 TCP 较快。

2. TCP 的三次握手指的是什么

简答：每次建立连接前，客户端和服务端之前都要先进行三次对话才开始正式传输内容，三次对话大概是这样的：
```
 1. 客户端：我要连接你了，可以吗
 2. 服务端：嗯，我准备好了，连接我吧
 3. 客户端：那我连接你咯。
 4. 开始后面步骤
```
如果你想更了解 TCP 可以看 http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html

## IP 网络协议（英语：Internet Protocol）

只要你在互联网中，那么你就会有一个 IP。通俗上理解，IP 分为「内网 IP」 和「外网 IP」

- 你从电信那里买来带宽，一年一千多。
- 电信为你提供 DNS 服务。
- 你买了一个路由器，然后用电脑和手机分别连接路由器广播出来的无线 WIFI。
- 只要路由器连上电信的服务器，那么路由器就会有一个「外网 IP」，比如「14.17.32.211」就是一个外网 IP。这就是你在互联网中的地址。
- 但是如果你重启路由器，那么你很有「可能」被重新分配一个「外网 IP」，也就是说 你的路由器没有「固定的外网 IP」
- 你可以花每年几千块钱租用一个「固定的外网 IP」，但是显然不会这么浪费钱。像腾讯、阿里这样的大公司租用了很多外网 IP，这样才能对我们提供稳定的服务。
- 但是有个问题，你的路由器的外网 IP 如果是14.17.32.211，那么你的手机和电脑的 IP 又是什么呢？答案是「内网 IP」
  - 路由器会在你家里创建一个内网，内网中的设备使用内网 IP，一般来说这个 IP 的格式都是 192.168.xxx.xxx。
  - 一般路由会给自己分配一个好记的内网 IP，如 192.168.1.1
  - 然后路由会给每一个内网中的设备分配一个不同的内网 IP，如电脑是 192.168.1.2，手机是 192.168.1.3，以此类推。
  - 现在路由器有两个 IP，一个外网 IP（14.17.32.211）和一个内网 IP（192.168.1.1）
    - 内网中的设备可以互相访问（比如你可以用电脑或手机进入 http://192.168.1.1 来查看你的路由器），但是不能直接访问外网，内网设备想要访问外网，就必须经过路由器中转。
    - 外网中的设备可以互相访问（比如 qq.com 可以把首页发送给你的路由器，你的路由器有外网 IP），但是外网中的设备无法访问你的内网设备（这很好理解，内网是一个封闭的网络，外人进不来，所以实际上 qq.com 无法直接把首页放送给你的电脑和手机）
      - 问题来了，那 qq.com 是怎么把首页发送到我的手机上的呢？答案是通过路由器来中转。
      - 路由器接收到 qq.com 的页面后，把页面发送给你的电脑或手机。路由器知道如何给这些信息指路，路由器就是一个指路人，这就是「路由」两个字的来历。
      - 路，就是「必由之路」中的路。由，就是「必由之路」中的由（由是经过、缘由的意思）。所有的信息都要经过路由器，然后被指向一条它该去的路。
      - 也就是说内网和外网就像两个隔绝的空间，无法互通，唯一的联通点就是路由器（因为路由器既有外网 IP 也有内网 IP），所以路由器有时候也被叫做「网关」，这个「关」是「一夫当关，万夫莫开」的「关」。如果路由器到电信的连接中断了，那么内网中所有的设备也就无法上网了。（这很好理解，相当于唯一一条出去的路断了）
- 除了内网 IP 和外网 IP，还有个特别特殊的 IP，就是本地 IP：127.0.0.1。本地 IP 永远表示设备自己。不信你可以 ping 127.0.0.1 一下，会发现只需要 0.01 ms 就得到了响应（你 ping qq.com 需要几十毫秒才得到响应）
- 默认情况下，hosts 文件里会有一行127.0.0.1 localhost，意思就是 localhost 指向 127.0.0.1，所以 localhost 也表示设备自己。不信你 ping localhost 试试，会发现实际上是在 ping 127.0.0.1

## 端口

想要访问一个设备（前提是你使用的是 TCP 或 UDP 协议。还记得吗，HTTP 就使用了 TCP），只指定 IP 是不够的，还必须指定端口（Port）。

端口其实就是一个编号，并不是一种硬件。

一个服务器（硬件）不一定只提供一种服务，比如一个服务器既提供 HTTP 服务，又提供 FTP 服务，还提供 SMTP 服务（邮件服务），那么只用一个 IP 是无法告诉服务器你想要使用哪种服务。

所以这里有一个重要的原则：一个端口对应一个服务。

比如：

1. 要提供 HTTP 服务你最好使用 80 端口（能不能使用别的端口？可以，不过不建议你违反约定）
2. 要提供 HTTPS 服务你最好使用 443 端口（能不能使用别的端口？可以，不过不建议你违反约定）
3. 要提供 FTP 服务你最好使用 21 端口（能不能使用别的端口？可以，不过不建议你违反约定）



#### 问题1：我怎么知道应该使用什么端口？
我们可以借助搜索引擎去查找， 网络上把 0 到 1023 号端口对应的服务都告诉你了，去看看吧。



#### 问题2：一共由多少端口？
每个机器一共有 65535（2的16次方减1）个端口（这是协议规定的）。不过这些端口的使用由一些规定

1. 0 到 1023（2的10次方减1）号端口是留给系统使用的，你只有拥有了管理员权限后，才能使用这 1024 个端口。
2. 其他端口可以给普通用户使用
3. 如果一个端口正在提供服务，也就是被占用了，那么就不能再使用这个端口。除非你先停掉正在占用这个端口的服务。

### 总结

> 使用 HTTP 协议访问另一个 IP 时，必须同时提供 IP 和端口号，缺一不可。

那么问题来了

我访问 http://qq.com 时并没有提供端口号，为什么我依然可以访问

答：因为浏览器帮你加了默认端口号 80。



# 一个简易的Server

我们现在就来搞起一个服务器，然后提供 HTTP 服务。

服务器你已经有了，你使用的电脑就是服务器。
但是你还没有提供 HTTP 服务的「程序」

用脚本就可以提供 HTTP 服务，不管是 Bash 脚本还是 Node.js 脚本都可以。这里我使用Node.js脚本。



## Node.js服务器

#### 接收请求

我们的脚本只需要一个文件就可以搞定


1. mkdir node-demo;
2. cd node-demo
3. touch server.js
4. 编辑 server.js


```
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if(!port){
  console.log('请指定端口号好不啦？\n例如像: node server.js 8888 这样')
  process.exit(1)
}

var server = http.createServer(function(request, response){
  var parsedUrl = url.parse(request.url, true)
  var path = request.url 
  var query = ''
  if(path.indexOf('?') >= 0){ query = path.substring(path.indexOf('?')) }
  var pathNoQuery = parsedUrl.pathname
  var queryObject = parsedUrl.query
  var method = request.method

  /******** 从这里开始看，上面不要看 ************/

  console.log('HTTP 路径为\n' + path)
  if(path == '/style.css'){
    response.setHeader('Content-Type', 'text/css; charset=utf-8')
    response.write('body{background-color: #ccc;}h1{color: pink;}')
    response.end()
  }else if(path == '/script.js'){
    response.setHeader('Content-Type', 'text/javascript; charset=utf-8')
    response.write('alert("这是返回的JS文件执行的")')
    response.end()
  }else if(path == '/index.html'){
    response.setHeader('Content-Type', 'text/html; charset=utf-8')
    response.write('<!DOCTYPE>\n<html>'  + 
      '<head><link rel="stylesheet" href="/style.css">' +
      '</head><body>'  +
      '<h1>Hello Node</h1>' +
      '<script src="/script.js"></script>' +
      '</body></html>')
    response.end()
  }else{
    response.statusCode = 404
    response.end()
  }

  /******** 代码结束，下面不要看 ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功\n请打开 http://localhost:' + port)
```
5. 运行 node server 或者 node server.js，然后我们看到报错
6. 根据报错提示调整你的命令
7. 成功之后，这个 server 会保持运行，无法退出
8. 如果你想「中断」这个 server，按 <kb>Ctrl</kbd> + <kbd>C</kbd> 即可（C 就是 Cancel 的意思）
9. 中断后你才能输入其他命令
10. 我建议你把这个 server 放在那里别动，新开一个 Bash 窗口，完成下面的教程：
11. 在新的 Bash 窗口运行 curl http://localhost:你的指定的端口/xxx 或者 curl http://127.0.0.1:你指定的端口/xxx。
12. 你会马上发现 server 打印出了路径！
13. 这说明我们的 server 收到了我们用 curl 发出的请求
14. 由于 server 迟迟没有发出响应，所以 curl 就一直等在那里，无法退出（用 <kb>Ctrl</kbd> + <kbd>C</kbd> 中断这个傻 curl）

好了服务器完成。but！

这个服务器目前只有一个功能，那就是打印出路径和查询字符串，多没意思啊。

我们还缺少一个重要的功能，那就是发出 HTTP 响应。





#### 发送响应：

接下来我们让我们 server 发出响应

1. 编辑 server.js
2. 在中间我标注的区域添加两行代码
```
 response.write('Hi')
 response.end()
```
3. 中断之前的 server，重新运行 node server 8888
4. curl http://127.0.0.1:8888/xxx，结果如下：
  
  `Hi\n`
  
5. 好了，响应添加成功


6. 使用 curl -s -v -- "http://localhost:8888/xxx" 可以查看完整的请求和响应。


**我们根据if语句的判断PATH的路径，可以对应的响应出我们想要响应出来的结果。**

例如：
```
if(path == '/){
response.setHeader('Content-Type', 'text/html;charset=utf-8')
response.write('Hello Node')
response.end()
} else {
response.setHeader('Content-Type', 'text/javascript; charset=utf-8')
response.write('404')
response.end()
}
```
    
    
    
    
    
    
    
    
    
    
    
    
    






































































