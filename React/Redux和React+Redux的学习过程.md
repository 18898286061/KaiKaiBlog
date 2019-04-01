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
3. 我们给它 `dispatch` 了 `action` ，`reducer` 也做出相应，最后也返回新的`state`。但是其实这时候`console.log()`不会打印新的数据。因为`state`虽然变化了，但是还是打印 `store.getState()` 还是原始的数据
4. 这时候我们就需要 `store.subscribe(() =>{}）` 了，它的作用就是每当 `reducer` 返回新的数据，它就会自动更新页面。
把UI组件的`state`更新下，否则，虽然`state`变化了，页面仍不会更新（虽然现在还没有其他组件）。



## React-Redux
好了，刚刚我们所说的这些就是基本的 `Redux` 的知识。
我们不难发现，这样其实挺麻烦的。
你需要写好多好多东西，而且我们并没有把 `reducer` ，`action` 的各种给分离出去，
不然的话我还要往很多组件里面传很多东西。
这对我们实际开发是很不友好的。
所以， 这时候就十分迫切需要 `React-Redux` 了。
它的作用是帮助我们操作 `Redux` 。
有了它我们可以很方便的写`Redux`。

```
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { createStore } from 'redux';
import { Provider, connect } from 'react-redux';

class App extends Component {
    render() {
        const { PayIncrease, PayDecrease } = this.props;
        return (
            <div className="App">
                <div className="App">
                    <h2>当月工资为{this.props.tiger}</h2>
                    <button onClick={PayIncrease}>升职加薪</button>
                    <button onClick={PayDecrease}>迟到罚款</button>
                </div>
            </div>
        );
    }
}

const tiger = 10000

//这是action
const increase = {
    type: '涨工资'
}
const decrease = {
    type: '扣工资'
}
//这是reducer
const reducer = (state = tiger, action) => {
    switch (action.type) {
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

//需要渲染什么数据
function mapStateToProps(state) {
    return {
        tiger: state
    }
}
//需要触发什么行为
function mapDispatchToProps(dispatch) {
    return {
        PayIncrease: () => dispatch({ type: '涨工资' }),
        PayDecrease: () => dispatch({ type: '扣工资' })
    }
}

//连接组件
App = connect(mapStateToProps, mapDispatchToProps)(App)

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
)

```

我们可以看到，我们仅仅对代码进行了稍微的改造，在index.js里面写个APP组件以方便我们观察，APP组件里面的 `button` 分别触发不同的事件。
`action`、 `reducer`、 `store` 想必大家都已经了解过了 这里不再做过多介绍。我们主要关注下哪里有变化：

> 首先最明显的是demo中引入 `react-redux` 的 `Provider` 和 `connect` ，它们非常重要！这里先大概解释下它们的作用：
  
- `Provider`：它是 `react-redux` 提供的一个 `React` 组件。作用是把 `state` 传给它的所有子组件，也就是说 当你用 `Provider` 传入数据后 ，下面的所有子组件都可以共享数据，十分的方便。 
- `Provider` 的使用方法：把 `Provider` 组件包裹在最外层的组件。如代码所示，把整个APP组件给包裹住，然后在Provider里面把store传过去。**注意：** 一定是在Provider中传store，不能在APP组件中传store。
- `connect`：它是一个高阶组件。所谓高阶组件就是你给它传入一个组件，它会给你返回新的加工后的组件，不过用法倒简单，深究其原理就有点难度。这里不做connect的深究，主要是学会它的用法，毕竟想要深究必须先会使用它。  首先它有 `四个参数` (`[mapStateToProps]`, `[mapDispatchToProps]`, `[mergeProps]`, `[options]`)，后面两个参数可以不写，不写的话它是有默认值的。我们主要关注下前两个参数`mapStateToProps`和`mapDispatchToProps`。
- `connect` 的使用方法是：把指定的`state`和指定的`action`与`React`组件连接起来，后面括号里面写UI组件名。


除此之外demo中还多出了 `mapStateToProps` 、 `mapDispatchToProps` 他们又有什么作用呢？

通俗一点讲的话就是：
  1. 比如你在一个很深的UI组件里 当你想要获得`store`的数据就很麻烦。
  2. `mapStateToProps` 就是告诉 `store` 你需要哪个 `state` ，需要什么数据就直接在 `mapStateToProps` 中写出来，然后store就会返回给你。
  3. 同理，如果你想要 `dispatch` 派发一些行为怎么办呢， `mapDispatchToProps` 就是告诉 `store` 你要派发什么行为，需要派发什么行为就在 `mapDispatchToProps` 中写出来，然后 `store` 就会把你想要派发的行为告诉 `reducer` 。
  4. 接下来大家都应该知道了 `reducer`就会根据旧的`state`和`action`返回新的 `state` 。


去试下我们写的代码吧。果不其然，当我们点击'升职加薪'按钮时候 '当月工资'也相应的增加了。我们捋一下整个事件流程：

1. 点击加薪按钮
2. 触发`PayDecrease()`方法
3. 该方法派发相应`action`
4. `reducer`根据`action`的`type`响应得到新的`state`
5. 通过`{this.props.tiger}`拿到新的 `state` ，渲染到页面


Ok，这个简单的demo我们就实现了。
但是现在还有一个问题：我们把所有的 `action`、`reducer`、`store` 、`Provider` 、`connect`等等都写在了一个页面，这在我们实际开发中肯定是不合理的，所以，我们最后就给这个小demo再优化下：

首先，我们要把`action`，`reducer`什么的抽离出去，作为一个单独的文件，然后再导出：

src/index.reducer.js:
```
const tiger = 10000

//这是action
const increase = {
    type: '涨工资'
}
const decrease = {
    type: '扣工资'
}
//这是reducer
const reducer = (state = tiger, action) => {
    switch (action.type) {
        case '涨工资':
            return state += 100;
        case '扣工资':
            return state -= 100;
        default:
            return state;
    }
}
export default reducer
```

其次，我们也要把APP组件写在外面 **(此处一定要注意： 导出的不是APP组件，而是connect后的APP组件)**

src/APP.js:
```
import React, { Component } from 'react';
import { connect } from 'react-redux';

class App extends Component {

  componentDidMount() {
    console.log(this.props)
  } 
  render() {
    const { PayIncrease, PayDecrease } = this.props;
    return (
      <div className="App">
        <h2>当月工资为{this.props.tiger}</h2>
        <button onClick={PayIncrease}>升职加薪</button>
        <button onClick={PayDecrease}>迟到罚款</button>
      </div>
    );
  }
}
//需要渲染什么数据
function mapStateToProps(state) {
  return {
    tiger: state
  }
}
//需要触发什么行为
function mapDispatchToProps(dispatch) {
  return {
    PayIncrease: () => dispatch({ type: '涨工资' }),
    PayDecrease: () => dispatch({ type: '扣工资' })
  }
}

export default App = connect(mapStateToProps, mapDispatchToProps)(App)

```

把这些东西分离出去之后，此时的index.js看起来明显就简洁了许多：

src/index.js:

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import reducer from './index.reducer'

//创建store
const store = createStore(reducer);


ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>, 
    document.getElementById('root'));
```

在最后，比较基础的Redux和React-Redux例子到这就结束了。 加油吧少年~
