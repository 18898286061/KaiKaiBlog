## 前言
根据 React 官方定义，React 是一个构建使用者介面的 JavaScritp Library。<br/>
以 MVC 模式来说，ReactJS 主要是负责 View 的部份。<br/>
在过去，我们被灌输了许多前端分离的观念，在前端三兄弟中：HTML 掌管内容结构、CSS 负责外观样式，JavaScript 主管逻辑互动。<br/>
然而，在 React 世界里，所有事物都是 以 Component 为基础，将同一个 Component 相关的代码和资源都放在一起，<br/>
而在编写 React Component 时我们通常会使用 JSX 的方式来提升程式撰写效率。<br/>
事实上，JSX 并非一种全新的语言，而是一种语法糖（Syntatic Sugar），一种语法类似 XML 的 ECMAScript 语法扩充。<br/>
在 JSX 中 HTML 和组建这些元素标签的代码有紧密的关系。因此你可能要熟悉一下以 Component 为单位的思考方式（本文主要使用 ES6 语法）。

## 使用JSX的好处
 ### 1. 提供更加语意化且易懂的标签
 由于 JSX 类似 XML 的语法，让一些非开发人员也更容易看懂，且能精确定义包含属性的树状结构。<br/>
 一般来说我们想做一个回馈表单，使用 HTML 写法通常会长这样
 ```
  <form class="messageBox">
    <textarea></textarea>
    <button type="submit"></button>
  </form>
 ```
 使用 JSX，就像 XML 语法结构一样可以自行定义标签且有开始和关闭，容易理解：
 ```
 <MessageBox />
 ```
 
React 结合 JSX 的写法更易读易懂，当 Component 组成越来越复杂时，若使用 JSX 将可以让整个结构更加直观，可读性较高。

### 2.更加简洁
虽然最终 JSX 会转换成 JavaScript，但使用 JSX 可以让程式看起来更加简洁，以下为使用 JSX 和不使用 JSX 的范例：
```
<a href="https://facebook.github.io/react/">Hello!</a>
```
不使用 JSX（记得我们说过 JSX 是选用的）：
```
// React.createElement(组件/HTML标签, 组件属性，以物件表示, 子组件)
React.createElement('a', {href: 'https://facebook.github.io/react/'}, 'Hello!')
```

### 3. 结合原生 JavaScript 语法
JSX并没有改变 JavaScript 语意。透过结合 JavaScript ，可以释放 JavaScript 语言本身能力。<br/>
下面例子就是运用 map 方法，轻易把 result 值迭代出来，产生无序清单（ul）的内容，<br/>
不用再使用蹩脚的模版语言 (**这边有个小地方要留意：这边用 map function 迭代出的 index，每个 `<li>` 元素记得加上独特的 key ，不然会出现问题**)
```
// const 为常数
const lists = ['JavaScript', 'Java', 'Node', 'Python'];

class HelloMessage extends React.Component {
  render() {
    return (
    <ul>
      {lists.map((result, index) => {
        return (<li key={index}>{result}</li>);
      })}
    </ul>);
  }
}
```

## JSX 用法
### 1. 环境设定与使用方式
初步了解为何要使用 JSX 后，我们来聊聊 JSX 的用法。一般而言 JSX 通常有两种使用方式：
1. 使用 browserify 或 webpack 等 CommonJS bundler 并整合 babel 预处理
2. 于浏览器端做解析

在这边简单起见，我们先使用第二种方式：

一般载入 JSX 方式有：
1. 内嵌
```
<script type="text/babel">
  ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
  );
</script>
```
2. 从外部引入
`<script type="text/jsx" src="main.jsx"></script>`

### 2. 标签用法
JSX 标签非常类似 XML ，可以直接书写。<br/>
一般 `Component` 命名首字大写，HTML Tags（标签） 小写。以下是一个建立 Component 的 class：
```
class HelloMessage extends React.Component {
  render() {
    return (
      <div>
        <p>Hello React!</p>
        <MessageList />
      </div>
    );
  }
}
```
### 3. 转换成 JavaScript
JSX 最终会转换成浏览器可以读取的 JavaScript，以下为其规则：
```
React.createElement(
  string/ReactClass, // 表示 HTML 元素或是 React Component
  [object props], // 属性值，用对象表示
  [children] // 接下来参数都为该元素子元素
)
```

解析前：（特别注意在 JSX 中使用 JavaScript 表达式时使用 {} 括起，如下方范例的 text，里面对应的是变数。若需放置一般文字，请加上 ''）：
```
var text = 'Hello React';
<h1>{text}</h1>
<h1>{'text'}</h1>

```
解析完后：
```
var text = 'Hello React';
React.createElement("h1", null, "Hello React!");
```
另外要特别要注意的是由于 JSX 最终会转成 JavaScript ，<br/>
且每一个 JSX 节点都对应到一个 JavaScript 函数，<br/>
所以在 Component 的 render 方法中只能回传一个根节点（Root Nodes）。<br/>
例如：若有多个 <div> 要 render 请在外面包一个 Component 或 <div>、<span> 元素。

### 5. 属性
在 HTML 中，我们可以透过标签上的属性来改变标签外观样式，<br/>
在 JSX 中也可以，但要注意 `class` 和 `for` 由于为 JavaScript 保留关键字用法，<br/>
因此在 JSX 中使用 className 和 htmlFor 替代。
```
class HelloMessage extends React.Component {
  render() {
    return (
      <div className="message">
        <p>Hello React!</p>
      </div>
    );
  }
}
```
**Boolean 属性**
在 JSX 中预设只有属性名称但没设值为 true，例如以下第一个 input 标签 disabled 虽然没设值，但结果和下面的 input 为相同：

<input type="button" disabled />;
<input type="button" disabled={true} />;
反之，若是没有属性，则预设预设为 false：

<input type="button" />;
<input type="button" disabled={false} />;


### 6. 扩展属性
在 ES6 中使用 ... 是迭代物件的意思，可以把所有物件对应的值迭代出来设定属性，但要注意后面设定的属性会盖掉前面相同属性：
```
var props = {
  style: "width:20px",
  className: "main",
  value: "yo",  
}

<HelloMessage  {...props} value="yo" />

// 等于以下
React.createElement("h1", React._spread({}, props, {value: "yo"}), "Hello React!");
```

### 7. 自定义属性
若是希望使用自定义属性，可以使用 data-：

`<HelloMessage data-attr="xd" />`



### 8. 显示 HTML
通常为了避免资讯安全问题，我们会过滤掉 HTML，若需要显示的话可以使用：

`<div>{{_html: '<h1>Hello World!!</h1>'}}</div>`
### 9. 样式使用
在 JSX 中使用外观样式方法如下，第一个 {} 是 JSX 语法，第二个为 JavaScript 物件。与一般属性值用 - 分隔不同，为驼峰式命名写法：

`<HelloMessage style={{ color: '#FFFFFF', fontSize: '30px'}} />`
### 10. 事件处理
事件处理为前端开发的重头戏，在 JSX 中透过 inline 事件的绑定来监听并处理事件（注意也是驼峰式写法），更多事件处理方法请参考官网

`<HelloMessage onClick={this.onBtn} />`
