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
    [img](https://image-static.segmentfault.com/152/073/1520733268-55dd228c67579_articlex)
  
  问题1: 容器元素wrap注册了事件，那么此事件的**作用范围**是什么？
  
  思考1:根据上面例子，当点击橘色块中(包括被子元素覆盖的部分)任何一部分时，都会弹出789，点击橘色块外面的部分并没有任何反应，那么我们是不是就可以得出这这样结论，元素注册事件的作用范围为元素自身在页面中所占的空间大小，但是真的就是这样吗？下面我们做个试验
  
  **实验1:**
  
  css代码修改如下,其他部分同上
  
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
  /*inner中的top被修改*/
  #inner {
    position: relative;
    top: 152px;
    left:25px;
    width: 50px;
    height: 50px;
    background: #44ddff;
  }
  ```
  
  **output**
  
    ![](http://image-static.segmentfault.com/210/695/2106953494-55dd4a220943b)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
