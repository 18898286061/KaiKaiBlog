# Promise对象

> 因为做的项目需要到了Async Await，<br/>
所以决定回顾一下掌握不牢得Promise对象与没学过的Generator

## Promise的含义
Promise对象的两个特点：
1. 对象的状态不受外界影响。<br/>
`Promise`对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。<br/>
只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。<br/>
这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。<br/>
`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。<br/>
只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 `resolved`（已定型）。<br/>
如果改变已经发生了，你再对`Promise`对象添加回调函数，还是可以得到这个结果。<br/>
这与事件（Event）完全不同，事件的特点是：如果你错过了它，再去监听，是得不到结果的。


Promise的优缺点：
  - **优点**：<br/>
  有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。<br/>
  此外，Promise对象提供统一的接口，使得控制异步操作更加容易。
  - **缺点**：<br/>
    1. 首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。
    2. 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
    3. 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
    
## 用法

Promise对象是一个构造函数，用来生成Promise实例。

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise实例生成以后，可以用`then`方法分别指定resolved状态和rejected状态的回调函数。

```
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法可以接受两个回调函数作为参数。<br/>
第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。<br/>
其中，第二个函数是可选的，不一定要提供。<br/>
这两个函数都接受Promise对象传出的值作为参数。

下面是一个Promise对象的简单例子。

```
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```

上面代码中，timeout方法返回一个Promise实例，表示一段时间以后才会发生的结果。<br/>
过了指定的时间（ms参数）以后，Promise实例的状态变为resolved，就会触发then方法绑定的回调函数。
