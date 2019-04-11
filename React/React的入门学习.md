
# React的入门学习

> 因为打算好好重头学一下ReactJS 所以 阅读了 Github 上的 《从零开始学 ReactJS》，做一下笔记，以便后来的复习，也记录自己学习的路程。本笔记多处出自本书。。

## ReactJS 的特别之处
1. 基于组件（Component）化思考
2. 用 JSX 进行声明式（Declarative）UI 设计
3. 使用 Virtual DOM
4. Component PropType 防呆机制
5. Component 就像个状态机（State Machine），而且有生命周期（Life Cycle）
6. 一律重绘（Always Redraw）和单向数据流（Unidirectional Data Flow）
7. 在 JavaScript 里写 CSS：Inline Style

## 基于组件（Component）化思考
在 React 的世界中最基本的单位为组件（Component），<br/>
每个组件也可以包含一个以上的子组件，并依照需求组装成一个组合式的（Composable）组件，<br/>
因此具有`封装（encapsulation）`、`关注点分离 (Separation of Concerns)`、`复用 (Reuse)` 、`组合 (Compose)` 等特性。

在 React 中组件是一切的基础，<br/>
让开发应用程式就好像在堆积木一样。对于过去模版式（template）的开发。React组件化是另一种开发模式，<br/>
过去我们往往习惯于将 HTML、CSS 和 JavaScript 分离，<br/>
**现在却要把它们都封装在一起。**

以下是写 React Component 的主要两种方式：
  1. 使用 ES6 的 Class（可以进行比较复杂的操作和组件生命周期的控制，但相对于 无状态组件（Stateless Components） 比较耗费资源）
  ```
  //  注意组件开头第一个字母都要大写
  class MyComponent extends React.Component {
  // render 是 Class based 组件唯一必须的方法（method）
  render() {
    return (
      <div>Hello, World!</div>
      );
    }
  }

  // 将 <MyComponent /> 组件插入 id 为 app 的 DOM 元素中
  ReactDOM.render(<MyComponent/>, document.getElementById('app'));
  ```
  2. 使用 Functional Component 写法（单纯的 render UI  无状态组件（stateless components），<br/>
     没有内部状态、没有实作物件和 ref，没有生命周期函数。<br/>
     建议：若是不需要控制生命周期的话建议多使用 无状态组件（stateless components） 获得比较好的效能
   ```
    // 使用 arrow function 来设计 Functional Component 让 UI 设计更单纯（f(D) => UI），减少副作用（side effect）
    const MyComponent = () => (
      <div>Hello, World!</div>
    );

    // 将 <MyComponent /> 组件插入 id 为 app 的 DOM 元素中
    ReactDOM.render(<MyComponent/>, document.getElementById('app'));
   ```
   
 ## 用 JSX 进行声明式（Declarative）UI 设计
 React 在设计上的思路认为使用 Component 比起模版（Template）和显示逻辑（Display Logic）更能实现`关注点分离`的概念，<br/>
 而搭配 JSX 可以实现声明式 Declarative（注重 『做什么』（ what to）），而非命令式 Imperative（注重怎么做『how to』 ）的程式书写方式。<br/>
 像下述的声明式（Declarative）UI 设计就比单纯用（Template）式的方式更易懂：
 ```
  // 使用宣告式（Declarative）UI 设计很容易可以看出这个组件的功能
  <MailForm />
 ```
 
 ```
  // <MailForm /> 内部长相
  <form>
  <input type="text" name="email" />
  <button type="submit"></button>
  </form>
 ```

## 使用 Virtual DOM
在传统 Web 中一般是使用 jQuery 进行 DOM 的直接操作。<br/>
然而更改 DOM 往往是 Web 效能的瓶颈，<br/>
因此在 React 世界设计有 Virtual DOM 的机制，让 App 和 DOM 之间用 Virtual DOM 进行沟通。<br/>
当更改 DOM 时，会透过 React 自身的 diff 演算法去计算出最小更新，进而去最小化更新真实的 DOM。


## Component PropType 防呆机制
在 React 设计时提供两个机制，让整个 Component 设计更加稳健：
 1. `props 预设值设定（Default Prop Values）`机制
 2. `Prop 的验证（Validation）`机制
 
 ```
 //  注意组件开头第一个字母都要大写
class MyComponent extends React.Component {
	// render 是 Class based 组件唯一必须的方法（method）
	render() {
		return (
			<div>Hello, World!</div>
		);
	}
}

// PropTypes 验证，若传入的 props type 不符合将会显示错误
MyComponent.propTypes = {
  todo: React.PropTypes.object,
  name: React.PropTypes.string,
}

// Prop 预设值，若对应 props 没传入值将会使用 default 值
MyComponent.defaultProps = {
 todo: {}, 
 name: '', 
}
 ```

## Component 就像个状态机（State Machine），还有生命周期（Life Cycle）
Component 就像个状态机（State Machine），<br/>
根据不同的 state（透过 setState() 修改）和 props（由父元素传入），Component 会出现对应的显示结果。<br/>
`而人有生老病死，组件也有生命周期。`（听起来像人生哲理）<br/>
我们通过操作生命周期处理函数，可以在对应的时间点进行 Component 需要的处理。


## 一律重绘（Always Redraw）和单向数据流（Unidirectional Data Flow）
在 React 世界中，props 和 state 是影响 React Component "长相"的重要要素。<br/>
其中 props 都是由父元素所传进来，`不能更改`，若要更改 props 则必须由父元素进行更改。<br/>
而 state 则是根据使用者互动而产生的不同状态，主要是透过 `setState()` 方法进行修改。<br/>
当 React 发现 props 或是 state 更新时，就会重绘整个 UI。<br/>
当然你也可以使用 `forceUpdate()` 去强制重绘 Component。<br/>
而 React 通过整合 Flux 或 Flux-like（例如：Redux）可以更具体实现`单向数据流（Unidirectional Data Flow）`，让资料流的管理更为清晰。


## 在 JavaScript 里写 CSS：Inline Style
在 React Component 中 CSS 使用 Inline Style 写法，全都封装在 JavaScript 当中：
```
const divStyle = {
  color: 'red',
  backgroundImage: 'url(' + imgUrl + ')',
};

ReactDOM.render(<div style={divStyle}>Hello World!</div>, document.getElementById('app'));
```
