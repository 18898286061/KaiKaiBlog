# Redux和React+Redux的学习过程

> 之前学习Redux是通过视频来学习的，总感觉对Redux的理解不太深入。比如说用Redux在我做的项目中能够做什么，在什么地方用。怎么去做。所以今天我打算重头学一下Redux

本文主要分为两个部分：
  1. Redux
  2. React-Redux 

## Redux
要知道Redux和React并没有什么的关系，Redux甚至可以和jQuery一起用。
React-Redux才是React的用于便捷操作Redux的第三方插件。
所以呢，学习React-redux之前我们要比较熟悉的了解Redux的思想。

下面附上我学习的代码:
  1. 脚手架创建完成项目之后，进入文件夹，把我们用不到的全删掉
  2. 在在src/index.js里引入Redux并创建Ation Reducer和Store

src/index.js：
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { createStore } from 'redux' 
 
//这是redux的原始state
const tiger = 10000

//这是action
const increase = {
    type:'涨工资'
}
const decrease = {
    type:'扣工资'
}

//这是reducer
const reducer = (state = tiger, action) => {
    switch (action.type){
      case '涨工资': 
        return state += 100;
      case '扣工资': 
        return state -= 100;
      default: 
        return state;
    }
}

//创建store
const store = createStore(reducer);

console.log(store.getState())

ReactDOM.render(<App />, document.getElementById('root'));
```

- `action`：行为，它是一个对象。里面必有 `type` 来指定其类型，这个类型可以理解为你要做什么，
而下面提到的`reducer` 要根据 `action` 的 `type` 来返回不同的 `state` 。而每个项目有且可以有多个`action`
- `reducer`： 可以理解为一个专门处理`state`的工厂，
给他一个旧数据它会根据不同`action.type`返回新的数据，也就是：`旧state + action = 新state` 每个项目有且可以有多个`reducer`
- `store`： `store`本质上是一个状态树，保存了所有对象的状态。任何UI组件都能直接的从`store`访问特定对象的状态。每个项目有且只能有一个`store`

好，现在脑子里有了这些基本的概念后我们就可以把`reducer`放到`createStore`里并创建好`store`了。

可以看到 代码的最后我们打印了`store.getState()`， 这段代码简单理解就是打印下`store`里的数据
因为我们只是写了`action`并没有给它`dispatch`，
所以`reducer`只会走默认的`default` ，所以仅仅是返回 `state=tiger`，那么`tiger`就是我们之前定义好的`state `

此时我们执行 `npm start` 在浏览器打开该项目 `F12` 打开控制台 可以看到打印的数据为：`10000`

这里我们只是简单的回顾下`redux`的基础知识 并没有在`app.js`里面写任何东西

好，你可以看到在上面我们虽然定义了`action` 但是好像并没有什么用啊。
接下来我们要做的事情就是让数值变化了。
刚才仅仅是获得到初始的数据，这并不是我们想要的结果，在实际项目中我们肯定有很多的需求。
不同需求对应不同功能，不同功能获得不同数据。
所以接下来我们要用到`dispatch`给它派发不同的`action`。


src/index.js:  
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { createStore } from 'redux' 
 
const tiger = 10000

//这是action
const increase = {
    type:'涨工资'
}
const decrease = {
    type:'扣工资'
}
//这是reducer
const reducer = (state = tiger, action) => {
    switch (action.type){
      case '涨工资': 
        return state += 100;
      case '扣工资': 
        return state -= 100;
      default: 
        return state;
    }
}

//创建store
const store = createStore(reducer);

//订阅事件
store.subscribe(() =>
  console.log(store.getState())
);

//派发事件
store.dispatch(increase)

ReactDOM.render(<App />, document.getElementById('root'));
```

可以看到，打印的数据变成了 11000 也就是说`reducer`根据 `dispatch` 派发的 `action` 的 `type` ，
`return`了新的 `state` 。
当然我们也可以派发 `store.dispatch(decrease)` 那打印的结果就是 10000，原因想必大家都知道。


其实我们仅仅是多写了`store.dispatch(increase)` 和 `store.subscribe(() =>{}）` 
他们的每个作用大概解释下：
1. `dispatch`的作用就是告诉`reducer `我给你 `action` , 你要根据我的 `action.type` 返回新的 `state` 。 
2. 然后 `reducer` 就会根据 `action` 的 `type`，返回新的 `state` 。
3. 我们给它 `dispatch` 了 `action` ，`reducer` 也做出相应，最后也返回新的`state`。但是其实这时候`console.log()`不会打印新的数据 因为`state`虽然变化了 但是还是打印 `store.getState()` 还是原始的数据
4. 这时候我们就需要 `store.subscribe(() =>{}）` 了，它的作用就是每当 `reducer` 返回新的数据，它就会自动更新页面。把UI组件的`state`更新下，否则，虽然`state`变化了，页面仍不会更新（虽然现在还没有其他组件）


