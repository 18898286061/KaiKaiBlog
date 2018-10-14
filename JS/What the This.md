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
  
  直接不带任何引用形式去调用函数，则this会指向全局对象，因为没有其他影响去改变this，this默认就是指向全局对象（浏览器是window，Node中是global）的。这个结论是在非严格模式的情况下，严格模式下这个this其实是undefined的。
