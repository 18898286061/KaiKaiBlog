# HTML5的新增特性

**（1）语义标签**

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
  | &lt;dialog&gt;&lt;/dialog&gt; |	定义对话框，比如提示框 |
  
  
  
**(2)增强型表单**
  HTML5 拥有多个新的表单 Input 输入类型。这些新特性提供了更好的输入控制和验证。
  
**(3)视频和音频**
  HTML5 提供了播放音频文件的标准，即使用 <audio> 元素
  HTML5 规定了一种通过 video 元素来包含视频的标准方法。
  
**(4)Canvas绘图**


**(5)SVG绘图**

**(7)拖放API**

**(8)Web Worker**

**(9)Web Storage**

**(10)WebSocket**
WebSocket是HTML5开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。在WebSocket API中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage 事件来接收服务器返回的数据。
```
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
