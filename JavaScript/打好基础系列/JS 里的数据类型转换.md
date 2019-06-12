# JS 里的数据类型转换

## 任意类型转字符串

1. `String(x)`

![String().png](https://i.loli.net/2018/12/12/5c10d343c802b.png)

2. `x.toString()`

![toString().png](https://i.loli.net/2018/12/12/5c10d343d5557.png)

3. `x + ''`

![x+''.png](https://i.loli.net/2018/12/12/5c10d344c7eaa.png)

## 任意类型转数字
1. `Number(x)`
2. `parseInt(x, 10)` [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)
3. `parseFloat(x) `[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)
4. `x - 0`
5. `+x`

## 任意类型转布尔
1. Boolean(x)
2. !!x
  - 这种方法是用类似“负负得正的方法”巧妙转换成为布尔值
