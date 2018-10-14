# What the This...

---

首先，分析this的指向共有四种类型，在分析之前，我们首先要记清楚一下两件事:
  1.函数被调用时（即运行时）才会确定该函数内this的指向;
  2.要确定函数中this的指向，必须先找到该函数被调用的位置。
  
### 第一种“test()”形式
  ```
  var a = 1
  function test () {
      console.log(this.a)
  }
  test()
  ```
  
  直接不带任何引用形式去调用函数，则this会指向全局对象，因为没有其他影响去改变this，this默认就是指向**全局对象**（浏览器是window，Node中是global）的。这个结论是在非严格模式的情况下，**严格模式**下这个this其实是undefined。
  
  
### 认准第二种“xxx.test()”形式

```
  var a = 1
  function test () {
      console.log(this.a)
  }
  var obj = {
      a: 2,
      test
  }
  obj.test()
```

这种形式对比起第一种，此时, test()被obj调用了; 谁去调用这个函数的，这个函数中的this就绑定到谁身上。

```
var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var obj0 = {
    a: 3,
    obj 
}
obj0.obj.test()
```

串串烧模式: 结果也是一样的，test()中的this只对直属上司（直接调用者obj）负责。


```
  var a = 1
  function test () {
      console.log(this.a)
  }
  var obj = {
      a: 2,
      test
  }
  var testCopy = obj.test
  testCopy() //1
```
这里并不需要去思考什么，按照我们的套路，我们就认函数调时的样子，有没有看到最后调用的时候跟第一种情况一毛一样;


## 认准第三种“test.call(xxx) / test.apply(xxx) / test.bind()”形式
```
  var a = 1
  function test () {
      console.log(this.a)
  }
  var obj = {
      a: 2,
      test
  }
  var testCopy = obj.test
  testCopy.call(obj) //2
```
可以看到，我们通过call来调用testCopy，并且传入了你想要this指向的上下文，那么this就会乖乖按照你的指示行事啦。


## 认准第四种“new test()”形式
```
  var a = 1
  function test (a) {
      this.a = a
  }
  var b = new test(2)
  console.log(b.a) //2
```
  new这个操作符其实是new了一个新对象出来，而被new的test我们称为构造函数，我们可以在这个构造函数里定义一下将要到来的新对象的一些属性。那么在构造函数里，我们怎样去描述这个还未出生的新对象呢？没错，就是用this。所以构造函数里的this指的就是将要被new出来的新对象(新生成的实例)。
  
## 箭头函数
```
var a = 1
var test = () => {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
obj.test() //1
```
开头我们有说过，“函数被调用时（即运行时）才会确定该函数内this的指向。”现在我们要改变一下说法，普通函数（非箭头函数）才能区别于箭头函数。箭头函数中的this在函数**定义的时候**就已经确定，它this指向的是它的外层作用域this的指向。
