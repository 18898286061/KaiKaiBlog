# 简单实现一个jQuery的API

> 在学习中，我模仿了jQury实现API的方法实现两个API以此来学习jQuery的思想, 虽然这个函数库已经过时，但是它的经典之处还是值得我们学习的~


## 命名空间
> 首先我先使用了命名空间的方式来封装自己的函数库
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>命名空间 & 重写this</title>
    <style>
    .red{
    color: red;
    }
    .blue{
    color: blue;
    }
    </style>
</head>
<body>
    <ul>
        <li id="item1">选项1</li>
        <li id="item2">选项2</li>
        <li id="item3">选项3</li>
        <li id="item4">选项4</li>
        <li id="item5">选项5</li>
    </ul>
</body>
<script>
// 命名空间
window.kkdom = {};

kkdom.getSiblings = function(node) {
    var allChildren = node.parentNode.children;
    var array = {
        length: 0
    }
    for(let i = 0; i < allChildren.length; i++) {
        if(allChildren[i] !== node) {
            array[array.length] = allChildren[i];
            array.length += 1;
        }
    }
    console.log(array);
    return array;
}

kkdom.addClass = function(node, classes) {
    classes.forEach( (value) => node.classList.add(value));
}

kkdom.getSiblings(item3);
kkdom.addClass(item3, ['a', 'b', 'c']);

</script>
</html>
```

## 重写Node
> 但是上述使用命名空间的方法传参麻烦, 我们更希望使用this的方式来使用函数，所以就用了重写Node的方式，我们扩展 Node 接口直接在 Node.prototype 上加函数

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>重写DOM</title>
    <style>
        .blue {
            color: blue;
        }
        .red {
            color: red;
        }
    </style>
</head>
<body>
    <ul>
        <li id="item1">选项1</li>
        <li id="item2">选项2</li>
        <li id="item3">选项3</li>
        <li id="item4">选项4</li>
        <li id="item5">选项5</li>
    </ul>
<script>
// 重写Node
Node.prototype.getSiblings = function() {
    var allChildren = this.parentNode.children;

    var array = {
        length: 0
    }
    for(let i = 0; i < allChildren.length; i++) {
        if(allChildren[i] !== this) {
            array[array.length] = allChildren[i];
            array.length += 1;
        }
    }
    console.log(array);
    return array;
}

Node.prototype.addClass = function(classes) {
    classes.forEach((value) => this.classList.add(value));
}

item3.getSiblings();
item3.addClass(['a', 'b', 'c']);

item3.getSiblings.call(item3);
item3.addClass.call(item3, ['a', 'b', 'c']);    
</script>
</body>
</html>
```
## 无侵入

> 重写Node容易使方法名冲突, 若是大家都在Node上扩展接口，方法名冲突，会造成很大的损失，所以我们使用「无侵入」的方式，写一个新的接口，而jQuery就是以这种方式实现的

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>jQuery</title>
    <style>
        div{
            border: 1px solid green;
        }
    </style>
</head>
<body>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script>
    // console.log(typeof(jQuery))
    // console.log($('li') );
    // console.dir($('li').prototype);


window.jQuery = function (nodeOrSelector) {
    let nodes = {};
    if (typeof nodeOrSelector === 'string') {
        let temp = document.querySelectorAll(nodeOrSelector); //伪数组
        for (let i = 0; i < temp.length; i++) {
            nodes[i] = temp[i];
        }
        nodes.length = temp.length;
    } else if (nodeOrSelector instanceof Node) {
        nodes = {
            0: nodeOrSelector,
            length: 1
        }
    }
    nodes.addClass = function (classes) {
        for (let i = 0; i < nodes.length; i++) {
            nodes[i].classList.add(classes);
        }
    }
    nodes.setText = function (value) {
        for (let i = 0; i < nodes.length; i++) {
            nodes[i].textContent = value;
        }
    }
    return nodes;
}
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
</script>
</body>
</html>
```




