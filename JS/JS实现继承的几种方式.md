## 继承的概念

**继承：** 即通过一定的方式实现让某个类型A获取另外一个类型B的属性或方法。其中类型A称之为子类型，类型B称之为父类型或超类型。


## JavaScript中的继承
  
  Object是所有对象的 父级 | 父类型 | 超类型：js中所有的对象都直接或间接的继承自Object。
  
  继承有两种方式：接口继承和实现继承，在js中只支持实现继承，实现继承主要依赖原型链来完成(于javascript原型链的层层递进查找规则，以及原型对象(prototype)的共享特性，实现继承是非常简单的事情)。
  
  JavaScript中实现继承的几种方式：
  
  说明:其他语言中继承通常通过类来实现，js中没有类的概念，js中的继承是某个对象继承另外一个对象，是基于对象的。
  
  **一、把父类的实例对象赋给子类的原型对象**
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
  
 
  **二、把父类的原型对象(prototype)赋给子类的原型对象（prototype）,可以继承到父类的方法，但是继承不到父类的属性**
  ```
  function Person() {
    this.userName = 'KaiKai';
  }
  Person.prototype.getUserName = function() {
    return 'this.userName';
  }
  function Teacher() {}
  Teacher.prototype = Person.prototype;
  
  var t1 = new Teacher();
  console.log(t1.showUserName() ); // KaiKai
  console.log(t1.userName) //undefined, 没有继承到父类的userName
  ```
  
  因为Teacher.prototype的隐式原型(__proto__)只指向Person.prototype，所以获取不到Person实例的属性
  
  **三、发生继承关系后，实例与构造函数（类）的关系判断**
  
  还是通过instanceof和isPrototypeOf判断
  
  ```
    function Person() {
      this.userName = 'KaiKai';
    }
    Person.prototype.getUserName = function() {
      return this.userName;
    }
    function Teacher () {}
    Teacher.prototype = new Person();
    
    var t1 = new Teacher();
    
    console.log(t1 instance Teacher); //true
    console.log(t1 instance Person); //true
    console.log(t1 instanceof Object); //true
    console.log(Teacher.prototype.isPrototypeOf(t1)); //true
    console.log(Person.prototype.isPrototypeOf(t1)); //true
    console.log(Object.prototype.isPrototypeOf(t1)); //true
  ```
  
  **四、父类存在的方法和属性，子类可以覆盖（重写），子类没有的方法和属性，可以扩展**
  ```
  function Person() {}
    Person.prototype.getUserName= function() {
      console.log('Person :: getUsername');
    }
    
  function Teacher() {}
  Teacher.prototype = new Person();
  Teacher.prototype.showUserName = function() {
    console.log('Teacher :: showUsername');
  }
  Teacher.prototype.showAge = function(){
    console.log(21);
  }
  var t1 = new Teacher();
  t1.showUserName(); // Teacher :: showUsername
  t2.showAge(); // 21
  
  ```
  
  **五、重写原型对象之后，其实就是把原型对象的_proto__指向发生了改变**
  
  原型对象的prototype的__proto__的指向发生了改变, 会把原本的继承关系覆盖(切断);
  
  ```
  function Person() {}
  Person.prototype.showUserName = function() {
    console.log('Person :: showUserName');
  }
  function Teacher() {}
  Teacher.prototype = new Person();
  Teacher.prototype = {
    showAge : function() {
      console.log(21);
    }
  }
  
  var t1 = new Teacher();
  t1.showAge(); //22
  t1.showUserName()// Uncaught TypeError: t1.showUserName is not a function
  ```
  
  上例，第7行，Teacher.prototype重写了Teacher的原型对象（prototype），原来第6行的原型对象的隐式原型(__proto__)指向就没有作用了.

  所以在第14行，oT.showUserName() 就会发生调用错误，因为Teacher的原型对象(prototype)的隐式原型(__proto__)不再指向父类(Person)的实例，继承关系被破坏了;
  
  **六、在继承过程中，小心处理实例的属性上引用类型的数据**
  
  ```
  function Person() {
    this.skills = ['php', 'javascript'];
  }
  function Teacher (){}
  Teacher.prototype = new Person();
  
  var t1 = new Teacher();
  var t2 = new Teacher();
  t1.skills.push('c++');
  console.log(t1.skills); // php, javascript, c++
  ```
  t1的skills添加了一项c++的数据，**其他的实例都能访问到**，因为其他实例中共享了skills数据，skills是一个引用类型
  
  
  **七、借用构造函数**
  为了消除引用类型影响不同的实例，可以借用构造函数，把引用类型的数据复制到每个对象上，就不会相互影响了;
  
  ```
  function Person(uName) {
    this.skills = ['php', 'javascript'];
    this.userName = uName;
  }
  
  Person.prototype.getUserName = function(){
    return this.userName;
  }
  function Teacher(uName) {
    Person.call(this, uName);
  } 
  var t1 = new Teacher();
  t1.skills.push('c++');
  var t2 = new Teacher();
  console.log(t2.skills); // php, javascript
  console.log(t2.getUserName()); // Uncaught TypeError: oT2.showUserName is not a function
  ```
  
  虽然t1添加了一项"c++",但是不会影响t2.skills的数据，通过子类构造函数中call的方式，去借用父类的构造函数，把父类的属性复制过来，而且还能**传递参数**，如第8行 function Teacher(uName) , 但是第15行，方法调用错误，因为构造函数中只复制了属性，不会复制到父类原型对象上的方法。
  
  **组合继承（原型对象 + 借用构造函数）**
  
  **单一的原型继承缺点有：**
  
  1、不能传递参数，如：
  
  Teacher.prototype = new Person();
  
  有些人说，小括号后面可以跟参数啊，没错，但是只要跟了参数，子类所有的实例属性，都是跟这个一样，说白了，还是传递不了参数；
  
  2、把引用类型放在原型对象上，会在不同实例上产生相互影响
 

  **单一的借用构造函数缺点有：**
  
  1、不能复制到父类的方法
  
  刚好原型对象方式的缺点，借用构造函数可以弥补，借用构造函数的缺点，原型对象可以弥补，于是，就产生了一种组合继承方法：
  
  
 ```
  function Person(uName) {
    this skills = ['php', 'javascript'];
    this.userName = uName;
  }
  Person.prototype.getUserName = function() {
    return this.userName;
  }
  function Teacher(uName){
    Person.call(this, uName);
  }
  
  Teacher.prototype = new Person();
  
  var t1 = new Teacher('KaiKai');
  t1.skills.push('C++');
  var t2 = new Teacher('KaiKai');
  console.log(t2.skills); //php, javascript
  console.log(t2.getUserName()); // KaiKai
 ```
 
 子类实例oT2的skills不会受到oT1的影响，子类的实例也能调用到父类的方法.
