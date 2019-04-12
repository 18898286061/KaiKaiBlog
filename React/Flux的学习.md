# Flux的学习
![Flux](https://i.loli.net/2019/04/12/5cb07507e8b6e.jpeg)
> 记录React的学习过程
## 前言
随着 React App 复杂度提升，我们会发现常常需要从父组件（Parent Component）透过 props 传递方法到子组件（Child Component）去改变 state tree，<br/>
不但不方便也难以管理，因此我们需要更好的数据架构来建置更复杂的应用程式。<br/>
**Flux** 是 Facebook 推出的 client-side 应用程式架构（Architecture），<br/>
主要想解决 MVC 架构的一些问题。事实上，Flux 并非一个完整的前端框架 Framework，<br/>
其特色在于实现了 Unidirectional Data Flow（单向数据流）的设计模式，<br/>
在开发复杂的大型应用程式时可以更容易地管理 state（状态）。<br/>
由于 React 主要是负责 View 的部份，所以透过搭配 Flux-like 的数据处理架构，可以更好的去管理我们的 state（状态），处理复杂的使用者互动<br/>
（例如：Facebook 同时要维护使用者是否按赞、点击相片，是否有新讯息等状态）。

由于原始的 Flux 架构在实现上有些部分可以精简和改善，在实务上我们通常会使用开发者社群开发的 `Flux-like` 相关的架构实现（例如：Redux、Alt、Reflux 等）。<br/>
不过这边我们主要会使用 Facebook 本身提供 Dispatcher API 函数库<br/>
（可以想成是一个 pub/sub 处理器，透过 broadcast（广播） 将 payloads 传给注册的 callback function）<br/>
并搭配 NodeJS 的 EventEmitter 模组去完成 Flux 架构的实现。


## Flux概念介绍
在 Flux Unidirectional Data Flow（单向数据流）世界里有四大主角，分别负责不同对应的工作：
### 1. actions / Action Creator

action 负责定义所有改变 state（状态）的**行为**，可以让开发者快速了解 App 的各种功能，若你想改变 state 你只能发 action。<br/>
注意 action 可以是同步或是异步。例如：新增代办事项，调用异步 API 获取资料。

在实际的使用上`actions`我们会分成 `action` 和 `Action Creator`。<br/>
action 为描述行为的 object（对象），<br/>
Action Creator 将 action 送给 `dispatcher`。<br/>
一般来说符合 Flux Standard Action 的 action 会像以下示例代码，<br/>
具备 `type` 来区别所触发的行为。而 `payload` 则是所夹带的资料：
```
// action
const addTodo = {
  type: 'ADD_TODO',
  payload: {
    text: 'Do something.'  
  }
}

AppDispatcher.dispatch(addTodo);
```

当发生 rejected Promise 情况：
```
{
  type: 'ADD_TODO',
  payload: new Error(),
  error: true
}
```

### 2. Dispatcher
Dispatcher 是 Flux 架构的核心，每个 App 只有一个 Dispatcher，<br/>
提供 API 让 store 可以注册回调函数（callback function），<br/>
并负责向所有 store 发送 action 事件。<br/>
在本范例中我们使用 Facebook 提供的 Dispatcher API，其内建有 `dispatch` 和 `subscribe` 方法。

### 3. Stores
一个 App 通常会有多个 store 负责存放业务逻辑，根据不同业务会有不同 store，例如：TodoStore、RecipeStore。<br/>
store 负责操作和储存资料并提供 view 使用 listener（监听器），若有资料更新即会触发更新。<br/>
值得注意的是 store 只提供 getter API 读取资料，若想改变 state 一律发送 action。

### 4.Views（Controller Views）
这部份是 React 负责的范畴，负责提供监听事件的回调函数（callback function） ，当事件发生时重新取得资料并重绘 View。


## Flux流程：
![Flux-流程](https://i.loli.net/2019/04/12/5cb07507abab4.png)

Flux 架构前置作业：

1.Stores 向 Dispatcher 注册 callback，当资料改变时告知 Stores
2. Controller Views 向 Stores 取得初始资料
3. Controller Views 将资料给 Views 去渲染 UI
4. Controller Views 向 store 注册 listener，当资料改变时告知 Controller Views

Flux 与使用者互动运作流程：
1. 使用者和 App 互动，触发事件，Action Creator 发送 actions 给 Dispatcher
2. Dispatcher 依序将 action 传给 store 并由 action type 判断合适的处理方式
3. 若有资料更新则会触发 Controller Views 向 store 注册的 listener 并向 store 取得更新资料
4. View 根据 Controller Views 的新资料重新绘制 UI

## Flux做一个TodoList
1. 我这里觉得搭Webpack太麻烦，选择了直接用React的脚手架来写。
`$ create-react-app flux-demo`
2. 然后安装flux依赖
`$ yarn add flux`

安装好后我们可以设计一下我们的资料夹结构。<br/>
![目录](https://i.loli.net/2019/04/12/5cb07507c98ea.gif)

src/index.js：
```
import React from 'react';
import ReactDOM from 'react-dom';
import TodoHeader from './components/TodoHeader';
import TodoList from './components/TodoList';

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
      <div>
        <TodoHeader />
        <TodoList />
      </div>
    );
  }
}

ReactDOM.render(<App />, document.getElementById('app'));

```

通常项目上我们会开一个 `constants` 资料夹存放 `config` 或是 `actionTypes` 常数。<br/>
以下是 `src/constants/actionTypes.js`：
```
export const ADD_TODO = 'ADD_TODO';
```

在这个示例中我们继承了 Facebook 提供的 `Dispatcher API`（主要是继承了 `dispatch`、`register` 和 `subscribe` 的方法），<br/>
打造自己的 DispatcherClass，当使用者触发 handleAction() 会 dispatch 出事件。<br/>
以下是 `src/dispatch/AppDispatcher.js`：
```
// Todo app dispatcher with actions responding to both
// view and server actions
import { Dispatcher } from 'flux';

class DispatcherClass extends Dispatcher {
  handleAction(action) {
    this.dispatch({
      type: action.type,
      payload: action.payload,
    });
  }
}

const AppDispatcher = new DispatcherClass();

export default AppDispatcher;
```

以下是我们利用 AppDispatcher 打造的 `ActionCreator` 由 `handleAction` 负责发出传入的 action ，<br/>
完整程式码如 `src/actions/todoActions.js`：
```
import AppDispatcher from '../dispatcher/AppDispatcher';
import { ADD_TODO } from '../constants/actionTypes';

export const TodoActions = {
  addTodo(text) {
    AppDispatcher.handleAction({
      type: ADD_TODO,
      payload: {
        text,
      },
    });
  },
};
```

`Store` 主要是负责资料以及业务逻辑处理，我们继承了 `events` 模组的 `EventEmitter`，<br/>
当 action 传入 `AppDispatcher.register` 的处理范围后，根据 `action type` 选择适合处理的 `store` 进行处理，<br/>
处理完后透过 `emit` 方法发出事件让监听的 `Views Controller` 知道。<br/>
以下是 `src/stores/TodoStore.js`：
```
import AppDispatcher from '../dispatcher/AppDispatcher';
import { ADD_TODO } from '../constants/actionTypes';
import { EventEmitter } from 'events';

const store = {
  todos: [],
  editing: false,
};

class TodoStoreClass extends EventEmitter {
  addChangeListener(callback) {
    this.on(ADD_TODO, callback);
  }
  removeChangeListener(callback) {
    this.removeListener(ADD_TODO, callback);
  }
  getTodos() {
    return store.todos;
  }
}

const TodoStore = new TodoStoreClass();

AppDispatcher.register((action) => {
  switch (action.type) {
    case ADD_TODO:
      store.todos.push(action.payload.text);
      TodoStore.emit(ADD_TODO);
      break;
    default:
      return true;
  }
  return true;
});

export default TodoStore;
```

在这个 React Flux 范例中我们把 View 和 Views Controller 整合在一起。<br/>
在 TodoHeader 中，我们主要任务是让使用者可以透过 input 新增代办事项。<br/>
使用者输入文字在 input 时会触发 onChange 事件，进而更新内部的 state，当使用者按了送出钮就会触发 onAdd 事件，dispatch 出 addTodo event。<br/>
以下是 `src/components/TodoHeader.js` 完整代码：
```
import React, { Component } from 'react';
import { TodoActions } from '../../actions/todoActions';

class TodoHeader extends Component {
  constructor(props) {
    super(props);
    this.onChange = this.onChange.bind(this);
    this.onAdd = this.onAdd.bind(this);
    this.state = {
      text: '',
      editing: false,
    };
  }
  onChange(event) {
    this.setState({
      text: event.target.value,
    });
  }
  onAdd() {
    TodoActions.addTodo(this.state.text);
    this.setState({
      text: '',
    });
  }
  render() {
    return (
      <div>
        <h1>TodoFlux</h1>
        <div>
          <input
            value={this.state.text}
            type="text"
            placeholder="请输入代办事项"
            onChange={this.onChange}
          />
          <button
            onClick={this.onAdd}
          >
            送出
          </button>
        </div>
      </div>
    );
  }
}

export default TodoHeader;
```

在上面的 Component 中我们让使用者可以新增代办事项，接下来我们要让新增的代办事项可以显示。<br/>
我们在 componentDidMount 设了一个监听器 TodoStore 资料改变时会去把资料重新再更新，这样当使用者新增代办事项时 TodoList 就会保持同步。<br/>
当以下是 `src/components/TodoList.js` 完整程式码：
```
import React, { Component } from 'react';
import TodoStore from '../../stores/TodoStore';

function getAppState() {
  return {
    todos: TodoStore.getTodos(),
  };
}
class TodoList extends Component {
  constructor(props) {
    super(props);
    this.onChange = this.onChange.bind(this);
    this.state = {
      todos: [],
    };
  }
  componentDidMount() {
    TodoStore.addChangeListener(this.onChange);
  }
  onChange() {
    this.setState(getAppState());
  }
  render() {
    return (
      <div>
        <ul>
          {
            this.state.todos.map((todo, key) => (
              <li key={key}>{todo}</li>
            ))
          }
        </ul>
      </div>
    );
  }
}

export default TodoList;
```

## 总结
Flux 优势：
1. 让开发者可以快速了解整个 App 中的行为
2. 资料和业务逻辑统一存放好管理
3. 让 View 单纯化只负责 UI 的排版不需负责 state 管理
4. 清楚的架构和分工对于复杂中大型应用程式易于维护和管理代码

Flux 劣势：
1. 代码上不够简洁
2. 对于简单小应用来说稍微复杂

以上就是 Flux 的实战入门，我知道一开始接触 Flux 一定会觉得很抽象，<br/>
有些读者甚至会觉得这个架构到底有什么好处（明明感觉没比 MVC 高明到哪去或是一点都不简洁），<br/>
但如同上述优点所说 Flux 设计模式的优势在于清楚的架构和分工对于复杂中大型应用程式易于维护和管理程式码。<br/>
若还是不熟悉可以跟着范例多动手，相信慢慢就可以体会 Flux 的特色。<br/>
事实上，在开发社群中为了让 Flux 架构更加简洁，产生了许多 Flux-like 的架构和函式库，<br/>
其中最有人气的，就是 `Redux`。



