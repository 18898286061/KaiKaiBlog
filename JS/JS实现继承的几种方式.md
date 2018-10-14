## 基于原型的继承

  ```
  function father(){
    this.faName = 'father';
    this.names = ['11', '22']
  }
  father.prototype.getFaName = function() {
    console.log(this.faName);
  };
  function Child() {
    this.chName = 'child';
  }
  child.prototype = new father();
  child.prototype = child;
  child.prototype.getchName = function() {
    console.log(this.chName);
  };
  
  var c1 = new child();
  c1.names.push("sasa");
  var c2 = new child();
  console.log(c2.names)   //原型上的names属性被共享了 不是我们所需要的
  
  ```
  
  这种继承会有如下的缺点：
  
  1. 如果父类包含有引用类型的属性 所有的子类就会共享这个属性。
  2. 在创建子类的实例时 不能向父类的构造函数传递参数
