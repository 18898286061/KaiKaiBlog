# HTML5的新增特性

## 语义标签

语义化标签使得页面的内容结构化，见名知义：

| 标签 | 描述 |
| -------- | -------- |
| &lt;header&gt;&lt;/header&gt; | 定义了文档的头部区域 |
| &lt;footer&gt;&lt;/footer&gt; | 定义了文档的尾部区域 |
| &lt;nav&gt;&lt;/nav&gt; | 定义文档的导航 |
| &lt;section&gt;&lt;/section&gt; | 定义文档中的节（section、区段） |
| &lt;article&gt;&lt;/article&gt; | 定义页面独立的内容区域 |
| &lt;aside&gt;&lt;/aside&gt; | 定义页面的侧边栏内容 |
| &lt;detailes&gt;&lt;/detailes&gt; | 用于描述文档或文档某个部分的细节 |
| &lt;summary&gt;&lt;/summary&gt; | 标签包含 details 元素的标题 |
| &lt;dialog&gt;&lt;/dialog&gt; | 定义对话框，比如提示框 |

## 增强型表单

  HTML5 拥有多个新的表单 Input 输入类型。这些新特性提供了更好的输入控制和验证。
  
## 视频和音频

  HTML5 提供了播放音频文件的标准，即使用 `<audio>` 元素
  HTML5 规定了一种通过 video 元素来包含视频的标准方法。
  
## Canvas绘图

## SVG绘图

## 拖放API

## Web Worker

`Web Worker` 的作用，就是为 `JavaScript` 创造多线程环境，允许主线程创建 `Worker` 线程，将一些任务分配给后者运行。在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算`密集型`或`高延迟`的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

`Worker` 线程一旦新建成功，就会`始终运行`，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。这样有利于随时响应主线程的通信。但是，这也造成了 Worker 比较耗费资源，不应该过度使用，而且一旦使用完毕，就应该关闭。

## Web Storage

使用HTML5可以在本地存储用户的浏览数据。早些时候,本地存储使用的是`cookies`。但是Web 存储需要更加的安全与快速. 这些数据`不会被保存在服务器上`，但是这些数据只用于用户请求网站数据上.它也可以存储大量的数据，而不影响网站的性能。数据以 `键/值` 对存在, web网页的数据`只允许该网页访问`使用。

客户端存储数据的两个对象为：
  
- localStorage - 没有时间限制的数据存储
- sessionStorage - 针对一个 session 的数据存储, 当用户关闭浏览器窗口后，数据会被删除。
  
在使用 web 存储前,应检查浏览器是否支持 `localStorage` 和 `sessionStorage`
  
``` javascript
if(typeof(Storage)!=="undefined")
{
// 是的! 支持 localStorage  sessionStorage 对象!
// 一些代码.....
}
else
{
// 抱歉! 不支持 web 存储。
}
```

  `localStorage` 与 `sessionStorage` 可使用的API都相同，常用的有如下几个（以localStorage为例）：

- 保存数据：localStorage.setItem(key,value);
- 读取数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

## WebSocket

`WebSocket` 是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。在 `WebSocket API` 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。浏览器通过 `JavaScript` 向服务器发出建立 `WebSocket` 连接的请求，连接建立以后，客户端和服务器端就可以通过 `TCP 连接` 直接交换数据。当你获取 `WebSocket`连接后，你可以通过 `send()` 方法来向服务器发送数据，并通过 `onmessage` 事件来接收服务器返回的数据。

``` html
<!DOCTYPE HTML>
<html>
   <head>
   <meta charset="utf-8">
   <title>WebSocket</title>
      <script type="text/javascript">
         function WebSocketTest()
         {
            if ("WebSocket" in window)
            {
               alert("您的浏览器支持 WebSocket!");
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
               ws.onopen = function()
               {
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
               ws.onmessage = function (evt)
               {
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
               ws.onclose = function()
               {
                  // 关闭 websocket
                  alert("连接已关闭...");
               };
            }
            else
            {
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
      </script>
   </head>
   <body>
      <div id="sse">
         <a href="javascript:WebSocketTest()">运行 WebSocket</a>
      </div>
   </body>
</html>
```
