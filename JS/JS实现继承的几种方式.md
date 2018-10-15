## 继承的概念

**继承：** 即通过一定的方式实现让某个类型A获取另外一个类型B的属性或方法。其中类型A称之为子类型，类型B称之为父类型或超类型。


## JavaScript中的继承
  
  Object是所有对象的 父级 | 父类型 | 超类型：js中所有的对象都直接或间接的继承自Object。
  
  继承有两种方式：接口继承和实现继承，在js中只支持实现继承，实现继承主要依赖原型链来完成(于javascript原型链的层层递进查找规则，以及原型对象(prototype)的共享特性，实现继承是非常简单的事情)。
  
  JavaScript中实现继承的几种方式：
  
  说明:其他语言中继承通常通过类来实现，js中没有类的概念，js中的继承是某个对象继承另外一个对象，是基于对象的。
  
  **一、把父类的实例对象给子类的原型对象**
  ```
  function Person() {
    this.userName = 'KaiKai';
  }
  Person.prototype.getUserName = function() {
    return this.userName;
  }
  function Teacher() {
  
  }
  Teacher.prototype = new Person();
  
  var t1 = new Teacher();
  console.log(t1.userName); // KaiKai
  console.log(t1.getUserName() ); // KaiKai
  ```
  
  这是通过把父类(Person)的一个**实例**赋给子类Teacher的**原型对象**，就可以实现继承，子类的实例就可以访问到父类的属性和方法;
  
  
  ![Alt text](https://images2017.cnblogs.com/blog/253192/201708/253192-20170827112655168-1905643207.png)
  
 
