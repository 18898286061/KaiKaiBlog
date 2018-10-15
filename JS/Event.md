# 总结一下JS中的事件绑定、时间委托、事件监听

**事件绑定**

  要想让JavaScript对用户的操作作出响应，首先要对DOM元素绑定事件处理函数。所谓事件处理函数，就是处理用户操作的函数，就是处理用户操作的函数，不同的操作对应不同的名称。

  在JavaScript中，有三种常用的绑定事件的方法：

  - 在DOM元素中直接绑定；
  - 在JavaScript代码中绑定；
  - 绑定事件监听函数。
  
  **在DOM中直接绑定事件**
  
  我们可以在DOM元素上绑定onclick、onmouseover、onmouseout、onmousedown、onmouseup、ondblclick、onkeydown、onkeypress、onkeyup等。

  **在JavaScript代码中绑定事件**
  
  在JavaScript代码中（即script标签内）绑定事件可以使JavaScript代码与HTML标签分离，文档结构清晰，便于管理和开发。
  
  **使用事件监听绑定事件**
  绑定事件的另一种方法是用 addEventListener() 或 attachEvent() 来绑定事件监听函数。下面详细介绍，事件监听。


















