# Redux的学习笔记

## Redux 核心概念介绍

Redux 资料流的模型大致上可以简化成： `View -> Action -> (Middleware) -> Reducer`。<br/>
当使用者和 View 互动时会触发事件发出 Action，若有使用 Middleware 的话会在进入 Reducer 进行一些处理，<br/>
当 Action 进到 Reducer 时，Reducer 会根据 `action type` 去 mapping 对应处理的动作，然后回传回新的 state。<br/>
View 则因为侦测到 state 更新而重绘页面。<br/>
在这里我们讨论的是 synchronous（同步）的情形，asynchronous（异步）会在实战中讨论。<br/>
以下就用官方网站上的简单范例来让大家感受一下 Redux 的整个使用流程：
```
import { createStore } from 'redux';

/** 
  下面是一个简单的 reducers ，主要功能是针对传进来的 action type 判断并回传新的 state
  reducer 规格：(state, action) => newState 
  一般而言 state 可以是 primitive、array 或 object 甚至是 ImmutableJS Data。
  但要留意的是不能修改到原来的 state ，回传的是新的 state。
  由于使用在 Redux 中使用 ImmutableJS 有许多好处，所以我们的范例 App 也会使用 ImmutableJS 
*/
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1;
  case 'DECREMENT':
    return state - 1;
  default:
    return state;
  }
}

// 创建 Redux store 去存放 App 的所有 state
// store 的可用 API { subscribe, dispatch, getState } 
let store = createStore(counter);

// 可以使用 subscribe() 来订阅 state 是否更新。但实务通常会使用 react-redux 来串连 React 和 Redux
store.subscribe(() =>
  console.log(store.getState());
);

// 若想改变 state ，一律发 action
store.dispatch({ type: 'INCREMENT' });
// 1
store.dispatch({ type: 'INCREMENT' });
// 2
store.dispatch({ type: 'DECREMENT' });
// 1
```

## Redux API 入门

### 1. createStore：`createStore(reducer, [preloadedState], [enhancer])`
我们知道在 Redux 中只会有一个 store。<br/>
在产生 store 时我们会使用 createStore 这个 API 来创建 store：
  1. 第一个参数放入我们的 `reducer` 或是有多个 reducers combine（使用 combineReducers）在一起的 rootReducers。
  2. 第二个参数我们会放入希望预先载入的 `state` 例如：user session 等。
  3. 第三个参数通常会放入我们想要使用用来增强 Redux 功能的`中间件（middlewares）` ，若有多个 middlewares 的话，通常会使用 applyMiddleware 来整合。

### 2. Store
属于 Store 的四个方法：
  - getState()
  - dispatch(action)
  - subscribe(listener)
  - replaceReducer(nextReducer)
关于 Store 重点是要知道 Redux 只有一个 Store 负责存放整个 App 的 State，<br/>
而唯一能改变 State 的方法只有发送 action。

### 3. combineReducers：`combineReducers(reducers)`

combineReducers 可以将多个 reducers 进行整合并回传一个 Function，让我们可以将 reducer 适度分割

### 4. applyMiddleware：`applyMiddleware(...middlewares)`
若有 NodeJS 的经验，对于 middleware 概念应该不陌生，让开发者可以在 req 和 res 之间进行一些操作。<br/>
在 Redux 中 Middleware 则是扮演 action 到达 reducer 前的第三方扩充。<br/>
而 applyMiddleware 可以将多个 middlewares 整合并回传一个 Function，便于使用。

### bindActionCreators：`bindActionCreators(actionCreators, dispatch)`

`bindActionCreators` 可以将 actionCreators 和 dispatch 绑定，并回传一个 Function 或 Object，<br/>
让程式更简洁。但若是使用 `react-redux` 可以用 `connect` 让 `dispatch` 行为更容易管理







