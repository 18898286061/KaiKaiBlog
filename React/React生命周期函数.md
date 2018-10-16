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
  
