# React生命周期函数

![Alt text](https://user-gold-cdn.xitu.io/2017/11/11/88e11709488aeea3f9c6595ee4083bf3?imageslim)

  如上图所示，React 组件就像人会有生老病死一样有生命周期。<br/>
  一般而言 Component 有以下三种生命周期的状态：React生命周期主要包括三个阶段：
  1. 初始化阶段
  2. 运行中阶段
  3. 销毁阶段
  
  在React不同的生命周期里，会依次触发不同的钩子函数，下面我们就来详细介绍一下React的生命周期函数。
  
## 初始化阶段

 ### 1. 设置组件的默认属性
  ```
  static defaultProps = {
    name: 'sls,
    age: 23
  };
  // or
  Counter.defaltProps = {name: 'sls'}
  ```
  
  ### 2. 设置组件的初始化状态
  ```
  constructor() {
    super();
    this.state = {number: 0}
  }
  ```
  
  ### 3. componentWillMount()
  组件即将被渲染到页面之前触发.
  
  ### 4. render()
  组件渲染.
  
  ### 5. componentDidMount()
  组件已经被渲染到页面中后触发：此时页面中有了真正的DOM的元素，可以进行DOM相关的操作.
  
  
## 运行中阶段
  ### 1. componentWillReceiveProps(object nextProps)
  组件接收到新的`props(属性)`时触发.
  
  ### 2. shouldComponentUpdate(object nextProps, object nextState)
  当组件接收到新属性，或者组件的状态发生改变时触发。组件首次渲染时并不会触发。（ 除非主动触发 `forceUpdate()` ）
  ```
    shouldComponentUpdate(newProps, newState) {
    if (newProps.number < 5) return true;
    return false
}
  ```
  该钩子函数可以接收到两个参数，新的`props属性`和`state状态`，返回**true/false**来控制组件是否需要更新。
  
  一般我们通过该函数来优化性能：<br/>
  一个React项目需要更新一个小组件时，很可能需要父组件更新自己的状态。<br/>
  而一个父组件的重新更新会造成它旗下所有的子组件重新执行render()方法，形成新的虚拟DOM，<br/>
  再用diff算法对新旧虚拟DOM进行结构和属性的比较，决定组件是否需要重新渲染。<br/>
  无疑这样的操作会造成很多的性能浪费，<br/>
  所以我们开发者可以根据项目的业务逻辑，在shouldComponentUpdate()中加入条件判断，从而优化性能
  
  ### 3. componentWillUpdate()
  组件即将被更新时触发
  
  ### 4. componentDidUpdate()
  组件被更新完成后触发。页面中产生了新的DOM的元素，可以进行DOM操作
  
  
## 销毁阶段
  ### 1、componentWillUnmount()
  组件被销毁时触发。这里我们可以进行一些清理操作，例如：清理定时器，取消Redux的订阅事件等等。
    

 
一开始学习 Component 生命周期时会觉得很抽象，接下来用一个简单的实例感受一下 Component 的生命周期。<br/>

1. 可以发现当一开始载入组件时第一个会触发 console.log('constructor');，<br/>
接着依序执行 ：
2. componentWillMount
3. componentDidMount
4. 而当点击文字触发 handleClick() 更新 state 时则会依序执行 -> 
5. componentWillUpdate
6. componentDidUpdate：

HTML：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <script src="https://fb.me/react-15.1.0.js"></script>
  <script src="https://fb.me/react-dom-15.1.0.js"></script>
  <title>Component LifeCycle</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

Component 生命周期展示：
```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    console.log('constructor');
    this.handleClick = this.handleClick.bind(this);
    this.state = {
      name: 'Mark',
    }
  }
  handleClick() {
    this.setState({'name': 'Zuck'});
  }
  componentWillMount() {
    console.log('componentWillMount');
  }
  componentDidMount() {
    console.log('componentDidMount');
  }
  componentWillReceiveProps() {
    console.log('componentWillReceiveProps');
  }
  componentWillUpdate() {
    console.log('componentWillUpdate');
  }
  componentDidUpdate() {
    console.log('componentDidUpdate');
  }
  componentWillUnmount() {
    console.log('componentWillUnmount');
  }
  render() {
    return (
      <div onClick={this.handleClick}>Hi, {this.state.name}</div>
    );
  }
}

ReactDOM.render(<MyComponent />, document.getElementById('app'));
```
其中特殊处理的函数 shouldComponentUpdate，<br/>
目前预设 `return true`。<br/>
若你想要优化效能可以自己编写判断方式。<br/>


若采用 immutable 可以使用 nextProps === this.props 比对是否有变动：
```
shouldComponentUpdate(nextProps, nextState) {
  return nextProps.id !== this.props.id; 
}
```

## Ajax 异步处理
若有需要进行 Ajax 非同步处理，请在 componentDidMount 进行处理。以下透过 jQuery 执行 Ajax 取得 Github API　资料当做范例：

HTML：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <script src="https://fb.me/react-15.1.0.js"></script>
  <script src="https://fb.me/react-dom-15.1.0.js"></script>
  <script src="https://code.jquery.com/jquery-3.1.0.js"></script>
  <title>GitHub User</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

app.js：
```
class UserGithub extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
          username: '',
          githubtUrl: '',
          avatarUrl: '',
        }
    }
    componentDidMount() {
        $.get(this.props.source, (result) => {
            console.log(result);
            const data = result;
            if (data) {
              this.setState({
                    username: data.name,
                    githubtUrl: data.html_url,
                    avatarUrl: data.avatar_url
              });
            }
        });
    }
    render() {
        return (
          <div>
            <h3>{this.state.username}</h3>
            <img src={this.state.avatarUrl} />
            <a href={this.state.githubtUrl}>Github Link</a>.
          </div>
        );
    }
}

ReactDOM.render(
  <UserGithub source="https://api.github.com/users/torvalds" />,
  document.getElementById('app')
);
```
  
  
