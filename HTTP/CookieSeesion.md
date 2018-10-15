cookie 和session 的区别：

1、cookie数据存放在客户的浏览器上，session数据放在服务器上。

2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session。

3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE。

4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。

5、所以个人建议：
   将登陆信息等重要信息存放为SESSION
   其他信息如果需要保留，可以放在COOKIE中





cookie、sessionStorage和localStorage的区别。
上面提到的技术名词，都是在客户端以键值对存储的存储机制，并且只能将值存储为字符串。

cookie	localStorage	sessionStorage
由谁初始化	客户端或服务器，服务器可以使用Set-Cookie请求头。	客户端	客户端
过期时间	手动设置	永不过期	当前页面关闭时
在当前浏览器会话（browser sessions）中是否保持不变	取决于是否设置了过期时间	是	否
是否随着每个 HTTP 请求发送给服务器	是，Cookies 会通过Cookie请求头，自动发送给服务器	否	否
容量（每个域名）	4kb	5MB	5MB
访问权限	任意窗口	任意窗口	当前页面窗口
