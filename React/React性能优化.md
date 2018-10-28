作者：darrell
链接：https://www.jianshu.com/p/333f390f2e84
來源：简书

## 2.3 Immutable.js

https://upload-images.jianshu.io/upload_images/1505342-3976df59afa7e620.gif?imageMogr2/auto-orient/


JavaScript中的对象一般都是可变的，因为使用了引用赋值，新的对象简单的引用了原始对象，改变新对象，改变新对象
将影响到原始对象。

举个例子：

```
foo = { a : 1};
bar = foo;
bar.a = 2;
```

  当我们给bar.a赋值后，会发现foo.a也变成了2，虽然我们可以通过深拷贝与浅拷贝解决这个问题，
但是这样做非常的昂贵，对cpu和内存会造成浪费。


  这里就需要用到Immutable，通过Immutable创建的Immutable Data一旦被创建，
就不能再更改。对Immutable对象进行修改、添加或删除操作，都会返回一个新的Immutable对象。


这里我们讲一下其中三个比较重要的数据结构

- Map：键值对集合，对应Object，Es6种也有专门的Map对象
- List：有序可重复列表，对应于Array
- ArraySet：有序且不可重复的列表

我们可以看两个例子：

使用Map生成一个immutable对象


```
import { Map , is } from 'immutable';

let obj = Map({
  'name': 'react study',
  'course': Map({name: 'react+redux'})
})

let obj1 = obj.set('name','darrell');

console.log(obj.get('course') === obj1.get('course')); // 返回true
console.log(obj === obj1); // 返回false
```

Immutable.is 比较的是两个对象的 hashCode 或 valueOf（对于 JavaScript 对象）。由于 immutable 内部使用了 Trie 数据结构来存储，只要两个对象的 hashCode 相等，值就是一样的。这样的算法避免了深度遍历比较，性能非常好。

```
let obj = Map({name:1,title:'react'});
let obj1 = Map({name:1,title:'react'});
console.log(is(obj,obj1)); // 返回true

let obj2 = {name:1,title:'react'};
let obj3 = {name:1,title:'react'};
console.log(is(obj2,obj3)); // 返回false

```


Immutable优点：

减少内存的使用
并发安全
降低项目的复杂度
便于比较复杂数据，定制shouldComponentUpdate方便
时间旅行功能
函数式编程
Immutable缺点：

学习成本
库的大小（建议使用seamless-immutable）
对现有项目入侵严重
容易与原生的对象进行混淆



