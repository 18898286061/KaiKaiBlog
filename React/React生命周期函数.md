# React生命周期函数
---
本文多处引用自：[图解ES6中的React生命周期 作者：扇子酱](https://juejin.im/post/5a062fb551882535cd4a4ce3)


![Alt text](https://user-gold-cdn.xitu.io/2017/11/11/88e11709488aeea3f9c6595ee4083bf3?imageslim)

  如图，React生命周期主要包括三个阶段：初始化阶段、运行中阶段和销毁阶段，在React不同的生命周期里，会依次触发不同的钩子函数，下面我们就来详细介绍一下React的生命周期函数;
  
## 一、初始化阶段
 #### 1、设置组件的默认属性
 
  ```
  static defaultProps = {
    name: 'sls,
    age: 23
  };
  // or
  Counter.defaltProps = {name: 'sls'}
  ```
  
  #### 2、设置组件的初始化状态
  
  ```
  constructor() {
    super();
    this.state = {number: 0}
  }
  ```
  
  #### 3、componentWillMount()
  
  ```
    组件即将被渲染到页面之前触发，此时可以进行开启定时器、向服务器发送请求等操作
  ```
  
  #### 4、render()
  
  ```
  组件渲染
  ```
  
  #### 5、componentDidMount()
  
  ```
    组件已经被渲染到页面中后触发：此时页面中有了真正的DOM的元素，可以进行DOM相关的操作
  ```
  
  ## 二、运行中阶段
  
  #### 1、componentWillReceiveProps()
  ```
    组件接收到属性时触发
  ```
  
  #### 2、shouldComponentUpdate()
  
  当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发
  
  
  ```
    当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发
    shouldComponentUpdate(newProps, newState) {
    if (newProps.number < 5) return true;
    return false
}
//该钩子函数可以接收到两个参数，新的属性和状态，返回true/false来控制组件是否需要更新。

  ```
  
  一般我们通过该函数来优化性能：

  一个React项目需要更新一个小组件时，很可能需要父组件更新自己的状态。而一个父组件的重新更新会造成它旗下所有的子组件重新执行render()方法，形成新的虚拟DOM，再用diff算法对新旧虚拟DOM进行结构和属性的比较，决定组件是否需要重新渲染

  无疑这样的操作会造成很多的性能浪费，所以我们开发者可以根据项目的业务逻辑，在shouldComponentUpdate()中加入条件判断，从而优化性能

  例如React中的就提供了一个PureComponent的类，当我们的组件继承于它时，组件更新时就会默认先比较新旧属性和状态，从而决定组件是否更新。值得注意的是，PureComponent进行的是浅比较，所以组件状态或属性改变时，都需要返回一个新的对象或数组

  
  #### 3、componentWillUpdate()
  ```
    组件即将被更新时触发
  ```
  
  #### 4、componentDidUpdate()
  ```
    组件被更新完成后触发。页面中产生了新的DOM的元素，可以进行DOM操作
  ```
  
  ## 三、销毁阶段
  
  
  1、componentWillUnmount()

    
  组件被销毁时触发。这里我们可以进行一些清理操作，例如清理定时器，取消Redux的订阅事件等等。
    

 

  
  
  
  
  
  
  
