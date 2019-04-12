# Immutable 是什么?
关于Immutable的定义， 简而言之， Immutable数据就是一旦创建，就不能更改的数据。<br/>
每当对Immutable对象进行修改的时候，就会返回一个新的Immutable对象，<br/>
以此来保证数据的不可变。
 
JavaScript 中的对象一般是可变的（Mutable），因为使用了引用赋值，新的对象简单的引用了原始对象，改变新的对象将影响到原始对象。如：
```
var map1 = { a: 1 }; 
var map2 = map1; 
map2.a = 2

console.log(map1.a) // = { a: 2}
```

我们修改 `map2.a=2` 你会发现此时 `map1.a` 也被改成了2。虽然这样做可以节约内存，但当应用复杂后，这就造成了非常大的隐患，
Mutable 带来的优点变得得不偿失。

怎么解决？ <br/>
为了解决这个问题，一般的做法是使用` deepCopy（深拷贝）`来避免被修改，<br/>
但这样做造成了 CPU 和内存的浪费。

而 `Immutable Data` 可以很好地解决这些问题，所谓的 `Immutable Data` 就是一旦创建，就不能再被更改的数据。

在 2013 年时 Facebook 工程师 Lee Byron 打造了 ImmutableJS，<br/>
ImmutableJS 的出现解决了 React 甚至 Redux 所遇到的一些问题。

以下范例即是引入了 ImmutableJS 的效果，可以发现，虽然我们操作了 map1 的值，<br/>
但会发现原本的 map1 并未受到影响（因为任何修改都不会影响到原始资料），<br/>
虽然使用 深拷贝（deepCopy） 也可以模拟类似的效果但会浪费过多性能，<br/>
ImmutableJS 则可以容易地共享没有被修改到的资料（例如下面的资料 b 即为 map1 所 map2 共享），因而有更好的效能表现。
```
import Immutable from 'immutable';

var map1 = Immutable.Map({ a: 1, b: 3 });
var map2 = map1.set('a', 2);

map1.get('a'); // 1
map2.get('a'); // 2
```

## Immutable Data

Immutable Data 就是一旦创建，就不能再被更改的数据。<br/>
对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。

Immutable 实现的原理是 **Persistent Data Structure**（持久化数据结构），<br/>
简而言之：`就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。`

同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 **Structural Sharing**（结构共享），<br/>
`即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。`

## ImmutableJS 特性介绍
ImmutableJS 提供了 7 种不可修改的资料类型：`List`、`Map`、`Stack`、`OrderedMap`、`Set`、`OrderedSet`、`Record`。<br/>
若是对 Immutable 物件操作都会回传一个新值。其中比较常用的有 `List`、`Map` 和 `Set`：
### 1. Map：类似于 key/value 的 object，在 ES6 也有原生 Map 对应
```
const Map= Immutable.Map;

// 1. Map 大小
const map1 = Map({ a: 1 });
map1.size
// => 1

// 2. 新增或取代 Map 元素
// set(key: K, value: V)
const map2 = map1.set('a', 7);
// => Map { "a": 7 }

// 3. 删除元素
// delete(key: K)
const map3 = map1.delete('a');
// => Map {}

// 4. 清除 Map 内容
const map4 = map1.clear();
// => Map {}

// 5. 更新 Map 元素
// update(updater: (value: Map<K, V>) => Map<K, V>)
// update(key: K, updater: (value: V) => V)
// update(key: K, notSetValue: V, updater: (value: V) => V)
const map5 = map1.update('a', () => (7))
// => Map { "a": 7 }

// 6. 合并 Map 
const map6 = Map({ b: 3 });
map1.merge(map6);
// => Map { "a": 1, "b": 3 }
```
### 2. List：有序且可以重复值，对应于一般的 Array
```
const List= Immutable.List;

// 1. 取得 List 长度
const arr1 = List([1, 2, 3]);
arr1.size
// => 3

// 2. 新增或取代 List 元素内容
// set(index: number, value: T)
// 将 index 位置的元素替换
const arr2 = arr1.set(-1, 7);
// => [1, 2, 7]
const arr3 = arr1.set(4, 0);
// => [1, 2, 3, undefined, 0]

// 3. 删除 List 元素
// delete(index: number)
// 删除 index 位置的元素
const arr4 = arr1.delete(1);
// => [1, 3]

// 4. 插入元素到 List
// insert(index: number, value: T)
// 在 index 位置插入 value
const arr5 = arr1.insert(1, 2);
// => [1, 2, 2, 3]

// 5. 清空 List
// clear()
const arr6 = arr1.clear();
// => []
```

### 3. Set：没有顺序且不能重复的列表
```
const Set= Immutable.Set;

// 1. 建立 Set
const set1 = Set([1, 2, 3]);
// => Set { 1, 2, 3 }

// 2. 新增元素
const set2 = set1.add(1).add(5);
// => Set { 1, 2, 3, 5 } 
// 由于 Set 为不能重复集合，故 1 只能出现一次

// 3. 删除元素
const set3 = set1.delete(3);
// => Set { 1, 2 }

// 4. 取联集
const set4 = Set([2, 3, 4, 5, 6]);
set1.union(set4);
// => Set { 1, 2, 3, 4, 5, 6 }

// 5. 取交集
set1.intersect(set4);
// => Set { 2, 3 }

// 6. 取差集
set1.subtract(set4);
// => Set { 1 }
```
## ImmutableJS 的优点整理
### 1. 持久数据结构（Persistent Data Structure）
可变（Mutable）数据耦合了 Time 和 Value 的概念，造成了数据很难被回溯。<br/>
在 ImmutableJS 的世界里，<br/>
就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。<br/>
就不会发生下列的状况：
```
var obj = {
 a: 1
};

funcationA(obj);

console.log(obj.a) // 不确定结果为多少？
```

使用 `ImmutableJS` 就没有这个问题：
```
// 有些开发者在使用时会在 ``Immutable` 变数前加 `$` 以示区别。

const $obj = fromJS({
 a: 1
});

funcationA($obj);

console.log($obj.get('a')) // 1
```

### 2. 结构共享（Structural Sharing）
为了维持资料的不可变，又要避免像`深拷贝（deepCopy）`一样复制所有的节点资料而造成的资源损耗，<br/>
在 ImmutableJS 使用的是 `结构共享` 特性，即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。
```
const obj = {
  count: 1,
  list: [1, 2, 3, 4, 5]
}
var map1 = Immutable.fromJS(obj);
var map2 = map1.set('count', 4);

console.log(map1.list === map2.list); // true
```

### 3. 支持惰性操作 （Support Lazy Operation）
```
Immutable.Range(1, Infinity)
.map(n => -n)
// Error: Cannot perform this action with an infinite size.

Immutable.Range(1, Infinity)
.map(n => -n)
.take(2)
.reduce((r, n) => r + n, 0); 
// -3
```
### 4.丰富的 API 并提供快速转换原生 JavaScript 的方式 
在 ImmutableJS 中可以使用 fromJS()、toJS() 进行 JavaScript 和 ImmutableJS 之间的转换。但由于在转换之间会非常耗费资源，所以若是你决定引入 ImmutableJS 的话请尽量维持资料处在 Immutable 的状态。

### 5. 函数式编程  （Functional Programming）
Immutable 本身就是 Functional Programming（函数式程式设计）的概念，所以在 ImmutableJS 中可以使用许多 Functional Programming 的方法，例如：map、filter、groupBy、reduce、find、findIndex 等。

### 6. 容易实现 Redo/Undo 历史回顾
因为每次数据都是不一样的，只要把这些数据放到一个数组里储存起来，想回退到哪里就拿出对应数据即可，很容易开发出撤销重做这种功能。



## Immutable 缺点

### 1. 需要学习新的 API

### 2. 增加了资源文件大小

### 3. 容易与原生对象混淆
这点是我们使用 Immutable.js 过程中遇到最大的问题。写代码要做思维上的转变。<br/>
虽然 Immutable.js 尽量尝试把 API 设计的原生对象类似，有的时候还是很难区别到底是 Immutable 对象还是原生对象，容易混淆操作。<br/>
Immutable 中的 Map 和 List 虽对应原生 Object 和 Array，但操作非常不同，<br/>
比如你要用 map.get('key') 而不是 map.key，array.get(0) 而不是 array[0]。<br/>
另外 Immutable 每次修改都会返回新对象，也很容易忘记赋值。<br/>
当使用外部库的时候，一般需要使用原生对象，也很容易忘记转换。

下面给出一些办法来避免类似问题发生：

1. 使用 Flow 或 TypeScript 这类有静态类型检查的工具
2. 约定变量命名规则：如所有 Immutable 类型对象以 $$ 开头。
3. 使用 Immutable.fromJS 而不是 Immutable.Map 或 Immutable.List 来创建对象，这样可以避免 Immutable 和原生对象间的混用。


## React 性能优化
ImmutableJS 除了可以和 Flux/Redux 整合外，<br/>
也可以用于基本 react 效能优化。以下是一般使用性能优化的简单方式：

传统 JavaScript 比较方式，若资料型态为 Primitive 就不会有问题：
```
// 在 shouldComponentUpdate 比较接下来的 props 是否一致，若相同则不重新渲染，提升效能
shouldComponentUpdate (nextProps) {
    return this.props.value !== nextProps.value;
}
```

但当比较的是对象的话就会出现问题：
```
// 假设 this.props.value 为 { foo: 'app' }
// 假设 nextProps.value 为 { foo: 'app' },
// 虽然两者值是一样，但由于 reference 位置不同，所以视为不同。但由于值一样应该要避免重复渲染
this.props.value !== nextProps.value; // true
```

使用 `ImmutableJS`：
```
var SomeRecord = Immutable.Record({ foo: null });
var x = new SomeRecord({ foo: 'app'  });
var y = x.set('foo', 'azz');
x === y; // false
```

在 ES6 中可以使用官方文件上的 PureRenderMixin 进行比较，可以让代码更简洁：
```
import PureRenderMixin from 'react-addons-pure-render-mixin';
class FooComponent extends React.Component {
  constructor(props) {
    super(props);
    this.shouldComponentUpdate = PureRenderMixin.shouldComponentUpdate.bind(this);
  }
  render() {
    return <div className={this.props.className}>foo</div>;
  }
}
```
