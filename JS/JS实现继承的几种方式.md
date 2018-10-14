## 继承的概念

**继承：**即通过一定的方式实现让某个类型A获取另外一个类型B的属性或方法。其中类型A称之为子类型，类型B称之为父类型或超类型。


## JavaScript中的继承
  
  Object是所有对象的 父级 | 父类型 | 超类型：js中所有的对象都直接或间接的继承自Object。
  
  继承有两种方式：接口继承和实现继承，在js中只支持实现继承，实现继承主要依赖原型链来完成。
  
  JavaScript中实现继承的几种方式：
  
  说明:其他语言中继承通常通过类来实现，js中没有类的概念，js中的继承是某个对象继承另外一个对象，是基于对象的。
  
  父类代码如下：
    
  ```
  function Animal(name){
    // 属性
    this.name = name || 'Animal';
    // 实例方法
    thi.sleep = function() {
      console.log(this.name + 'Sleeping');
    }
  }
  // 原型方法:
  Animal.prtotype.eat = function(food) {
    console.log(this.name + 'Eating：' + food);
  }

  ```
  
  ## 1、原型链继承
  **核心:** 将父类的实例作为子类的原型; 这样的话创建出来的子类都有父类的
  ```
  function Cat() {
  
  }
  
  Cat.prototype = new Animal();
  Cat.prototype.name = 'Cat';

  //　Test Code
  var cat = new Cat();
  console.log(cat.name); // Cat
  console.log(cat.eat('fish')); // CatEating：fish
  console.log(cat.sleep()); // undefined
  console.log(cat instanceof Animal); //true 
  console.log(cat instanceof Cat); //true
  ```
  
  特点：

  1. 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
  2. 父类新增原型方法/原型属性，子类都能访问到
  3. 简单，易于实现
  
  缺点：

  1. 要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行，不能放到构造器中
  2. 无法实现多继承
  3. 来自原型对象的所有属性被所有实例共享
  4. 创建子类实例时，无法向父类构造函数传参
  
