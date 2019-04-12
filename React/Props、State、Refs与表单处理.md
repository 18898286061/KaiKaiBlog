> React Component （React组件）事实上可以视为显示 UI 的一个状态机（state machine）。<br/>
这个状态机根据不同的 state（通过 setState() 修改）和 props（由父元素传入），Component 会出现对应的显示结果。

## Props
我们使用 一个简单的组件（A Simple Component）  来说明 props 的使用方式。<br/>
传入组件的 name 属性为 Mark，所以以下代码将会在浏览器显示 Hello, Mark。<br/>
针对传入的 props 我们也有`验证`和`预设值`的设计，让我们撰写的组件可以更加稳定健壮（robust）。

HTML：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>A Component Using External Plugins</title>
</head>
<body>
<!-- 这边方便使用 CDN 方式引入 react 、 react-dom 进行讲解  -->
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
  <div id="app"></div>
	<script src="./app.js"></script>
</body>
</html>
```

引入的 app.js，使用 `ES6 Class Component` 写法：
```
class HelloMessage extends React.Component {
	// 若是需要绑定 this.方法或是需要在 constructor 使用 props，定义 state，就需要 constructor。
  // 若是在其他方法（如 render）使用 this.props 则不一定非要定义 constructor
	constructor(props) {
		// 对于 OOP 物件导向程式设计熟悉的读者应该对于 constructor 构造函数的使用不陌生，
    // 事实上它是 ES6 的语法糖，骨子里还是 prototype based 物件导向程式语言。
    // 透过 extends 可以继承 React.Component 父类别。super方法 可以继承父类别的建构子
		super(props);
		this.state = {}
	}
	// render 是唯一必须的方法，但如果是单纯 render UI 建议使用 Functional Component 写法，性能更好而且比较简洁
	render() {
		return (
			<div>Hello {this.props.name}</div>
		)
	}
}

// PropTypes 验证，若传入的 props type 不是 string 将会显示错误
HelloMessage.propTypes = {
  name: React.PropTypes.string,
}

// Prop 预设值，若对应 props 没传入值将会使用 default 值 Zuck
HelloMessage.defaultProps = {
 name: 'Zuck',
}

ReactDOM.render(<HelloMessage name="Mark" />, document.getElementById('app'));
```


我们来对比一下 使用 `Functional Component` 写法：
```
// Functional Component 可以视为 f(d) => UI，根据传进去的 props 绘出对应的 UI。
// 注意这边 props 是传入函式的参数，因此取用 props 不用加 this
const HelloMessage = (props) => (
	<div>Hello {props.name}</div>
);

// PropTypes 验证，若传入的 props type 不是 string 将会显示错误
HelloMessage.propTypes = {
  name: React.PropTypes.string,
}

// Prop 预设值，若对应 props 没传入值将会使用 default 值 Zuck。用法等于 ES5 的 getDefaultProps
HelloMessage.defaultProps = {
 name: 'Zuck',
}

ReactDOM.render(<HelloMessage name="Mark" />, document.getElementById('app'));
```

## State
接下来我们将使用有State的组件（A Stateful Component）这个范例来讲解 State 的用法。<br/>
在 React Component 可以自己管理自己的内部 state，并用 this.state 来存取 state。<br/>
当 setState() 方法更新了 state 后将重新呼叫 render() 方法，重新绘制 component 内容。<br/>
以下范例是一个每 1000 毫秒（等于1秒）就会加一的累加器。<br/>
由于这个范例是 Stateful Component 因此仅使用 ES6 Class Component，而不使用 Functional Component。

HTML：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>A Component Using External Plugins</title>
</head>
<body>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
  <div id="app"></div>
	<script src="./app.js"></script>
</body>
</html>
```

app.js：
```
class Timer extends React.Component {
	constructor(props) {
		super(props);
		// 与 ES5 React.createClass({}) 不同的是 
    // component 内自定义的方法需要自行绑定 this context，或是使用 arrow function（箭头函数）
        this.tick = this.tick.bind(this);
        
		// 初始 state，等于 ES5 中的 getInitialState
		this.state = {
			secondsElapsed: 0,
		}
	}
  
	// 累加器方法（ tick() ），每一秒被呼叫后就会使用 setState() 更新内部 state，让 Component 重新 render
	tick() {
	    this.setState({secondsElapsed: this.state.secondsElapsed + 1});
	}
  
	// componentDidMount 为 component 生命周期中阶段 component 已插入节点的阶段，
  // 通常一些异步操作都会放置在这个阶段。这便是每1秒钟会去呼叫 tick 方法
	componentDidMount() {
	    this.interval = setInterval(this.tick, 1000);
	}
  
	// componentWillUnmount 为 component 生命周期中 component 即将移出插入的节点的阶段。
  // 这边移除了 setInterval 
	componentWillUnmount() {
		clearInterval(this.interval);
	}
  
	// render 为 class Component 中唯一需要定义的方法，其渲染 component 想要显示的内容
	render() {
	    return (
	      <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
	    );
	}
}

ReactDOM.render(<Timer />, document.getElementById('app'));
```

## 事件处理（Event Handle）
下列将使用 React 官网的 An Application 当做例子，实作出一个简单的 TodoApp。

HTML：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>A Component Using External Plugins</title>
</head>
<body>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
  <div id="app"></div>
	<script src="./app.js"></script>
</body>
</html>
```

app.js：
```
// TodoApp 组件中包含了显示 Todo 的 TodoList 组件，Todo 的内容透过 props 传入 TodoList 中。
// 由于 TodoList 仅单纯 Render UI 不涉及内部 state 操作是 stateless component，
// 所以使用 Functional Component 写法。
// 需要特别注意的是这边我们用 map function 来迭代 Todos，
// 而每个迭代的元素必须要有 unique key 不然会发生错误（可以用自定义 id，或是使用 map function 的第二个参数 index）
const TodoList = (props) => (
	<ul>
		{
			props.items.map((item) => (
				<li key={item.id}>{item.text}</li>
			))
		}
	</ul>
)

// 整个 App 的主要组件，这边比较重要的是事件处理的部份，内部有
class TodoApp extends React.Component {
	constructor(props) {
		super(props);
		this.onChange = this.onChange.bind(this);
		this.handleSubmit = this.handleSubmit.bind(this);
		this.state = {
			items: [],
			text: '',
		}
	}
	onChange(e) {
    	this.setState({text: e.target.value});
	}
	handleSubmit(e) {
    	e.preventDefault();
    	const nextItems = this.state.items.concat([{text: this.state.text, id: Date.now()}]);
    	const nextText = '';
    	this.setState({items: nextItems, text: nextText}); // 将写入的item插入items，然后把text清空
	}
	render() {
	    return (
	      <div>
	        <h3>TodoList</h3>
	        <TodoList items={this.state.items} />
	        <form onSubmit={this.handleSubmit}>
	          <input onChange={this.onChange} value={this.state.text} />
	          <button>{'Add #' + (this.state.items.length + 1)}</button>
	        </form>
	      </div>
	    );
	}
}

ReactDOM.render(<TodoApp />, document.getElementById('app'));
```
以上介绍了 React 事件处理的部份，除了 `onChange` 和 `onSubmit` 外，React 也封装了常用的事件处理，如 `onClick` 等。
若想更进一步了解有哪些可以使用的事件处理方法可以参考 官网的 [Event System](https://reactjs.org/docs/events.html)。


## Refs 与表单处理
上面介绍了 props（传入后就不能修改）、state（随着使用者互动而改变）和事件处理机制（Event System）后，<br/>
我们介绍如何在 React 中进行表单处理。<br/>
同样我们使用 React 官网范例 A Component Using External Plugins （使用外部插件）进行介绍。<br/>
由于 React 可以容易整合外部的 libraries（例如：jQuery），<br/>
本范例将使用 remarkable 结合 ref 属性取出 DOM Value 值（另外比较常用的作法是使用 onChange 事件处理方式处理表单内容），<br/>
让使用者可以使用 Markdown 语法的所见即所得编辑器（editor）。

HTML Markup
（除了引入 react 、 react-dom 还要用 CDN 方式引入 remarkable 这个 Markdown 语法 parser 套件，<br/>
  记得如果没有使用 Webpack 或是 browserify + babelify 等工具，<br/>
  那你就需要引入 babel-standalone 浏览器解析 ES6 语法并于引入 script 加上 type="text/babel"）：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>A Component Using External Plugins</title>
</head>
<body>
<script src="https://fb.me/react-15.1.0.js"></script>
<script src="https://fb.me/react-dom-15.1.0.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/6.18.1/babel.min.js"></script>
<script src="https://cdn.jsdelivr.net/remarkable/1.6.2/remarkable.min.js"></script>
  <div id="app"></div>
	<script type="text/babel" src="./app.js"></script>
</body>
</html>
```

app.js
```
class MarkdownEditor extends React.Component {
	constructor(props) {
		super(props);
		this.handleChange = this.handleChange.bind(this);
		this.rawMarkup = this.rawMarkup.bind(this);
		this.state = {
			value: 'Type some *markdown* here!',
		}
	}
	handleChange() {
	    this.setState({value: this.refs.textarea.value});
	}
	// 将使用者输入的 Markdown 语法 parse 成 HTML 放入 DOM 中，<br/>
  // React 通常使用 virtual DOM 作为和 DOM 沟通的中介，不建议直接由操作 DOM。<br/>
  // 故使用时的属性为 dangerouslySetInnerHTML
	rawMarkup() {
	    const md = new Remarkable();
	    return { __html: md.render(this.state.value) };
	}
	render() {
	    return (
	      <div className="MarkdownEditor">
	        <h3>Input</h3>
	        <textarea
	          onChange={this.handleChange}
	          ref="textarea"
	          defaultValue={this.state.value} />
	        <h3>Output</h3>
	        <div
	          className="content"
	          dangerouslySetInnerHTML={this.rawMarkup()}
	        />
	      </div>
	    );
	}
}

ReactDOM.render(<MarkdownEditor />, document.getElementById('app'));
```

















