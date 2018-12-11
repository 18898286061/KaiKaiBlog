# JavaScript 里的数据类型

JS有七种数据类型：Number String Boolean Symbol Undefined Null Object

**注意：没有 Array 类型也没有 Function 类型，他们属于Object类型**


1. <b>Number</b>

  - 整数和小数：`1` 、 `1.1` 、 `.1`
  - 科学记数法：`1.23e2`
  - 二进制：`0b11`
  - 八进制：`011`（后来 ES5 添加了 0o11 语法）
  - 十六进制：`0x11`
  
2. <b>String</b>

  - 空字符串：`''`  （请注意：空字符串不是空格字符串，一旦引号加上空格就有长度）
  - 多行字符串：
  ```
    var string = '12345' +
                '67890' // 无回车符号
    或
    
    var string = `12345
    67890` // 含回车符号
  ```
  
3. <b>Boolean</b>

  - 只有两个值：true 和 false
  - `a && b` 在 a 和 b 都为 true 时，取值为 true；否则都为 false
  - `a || b` 在 a 和 b 都为 false 时，取值为 false；否则都为 true

4. <b>Symbol</b>

  - ES 6 引入了一个新的数据类型 Symbol
  - Symbol 的用途：可以创建一个独一无二的值（但并不是字符串）
  - **Symbol 生成一个全局唯一的值**
  
  MDN： Symbol
  阮一峰：ECMAScript 6入门


5. <b>Undefined 与 Null</b>

**它们俩都表示没有值**

- 如果一个变量没有被赋值，那么这个变量的值就是 Undefiend
- 如果你想表示一个还没赋值的**对象**，就用 null
- 如果你想表示一个还没赋值的**字符串/数字/布尔/symbol**，就用 undefined（但是实际上你直接 var xxx 一下就行了，不用写 var xxx = undefined）

6. <b>Object</b>

  - object 就是上面几种基本类型（无序地）组合在一起
  - object 里面可以再有 object
  ```
    var person = {
        name: 'Frank', 
        'child': {
            name: 'Jack'
        }, // 最后这个逗号可有可无
    }
  ```
  - object 的 key 一律是字符串，不存在其他类型的 key
  - `object['']`（空字符串）是合法的
  - `object['key']` 可以写作 `object.key`
  - 注意 `object.key` 与 `object[key]` 不同
  - `delete object['key']` ：将该对象的某个key整个删除（键值都删除）
  - `'key' in object` 用来遍历对象，看该key值存不存在与该对象
  - ```
      for(var key in object) {
        console.log(key); // 遍历打印key值
        console.log(object[key]) //遍历打印value值
    }
    ```

  
  
## typeof 操作符

xxx 的类型	| string |	number |	boolean |	symbol	| undefined	| null |	object |	function
-------- | --- |--- |--- |--- |---| --- |---| --- 
typeof xxx	| 'string'	| 'number' | 'boolean'	|'symbol'	| 'undefined' |	'object' |	'object' | 'function'


注意： **function 并不是一个类型**















