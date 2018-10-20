# 事件流
  ```
  定义:
  1.事件流 描述的是从页面中接收事件的顺序，也可理解为事件在页面中传播的顺序。
  
  2.事件 就是用户或浏览器自身执行的某种动作。诸如click(点击)、load(加载)、mouseover(鼠标悬停)。
  
  3.事件处理程序 响应某个事件的函数就叫事件处理程序(或事件侦听器)。
  ```
  
# 事件的作用范围讨论

  **示例1**

  HTML
  ```
    <div id="wrap">
      <div id="outer>
        <div id="inner></div>
      </div>
    </div>
  ```
  CSS
  ```
  #wrap {
    width: 200px;
    height: 200px;
    background: orange;
  }
  
  #outer {
    position: relative;
    top: 50px;
    left: 50px;
    width: 100px;
    height: 100px;
    background: #eeddff;
}
  #inner {
    position: relative;
    top: 25px;
    left:25px;
    width: 50px;
    height: 50px;
    background: #44ddff;
  }
  ```
  
  js
 
  ```
  var wrap = document.getElementById('wrap');
  wrap.addEventListener('click',function(){
    alert('789');
  },false);
  ```
  
  **output**
  ![img](https://image-static.segmentfault.com/152/073/1520733268-55dd228c67579_articlex)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
