水平居中：

  内联：爸爸身上写 text-align:center;
  
  块级：margin-left: auto; margin-right: auto;

---

垂直居中：
  
  注：以下未特别写出HTML代码的实例均使用以下HTML:
  ```
  <body>
  <div class="parent">
    <div class="child">
      一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
    </div>
  </div>
</body>
  ```
  
  如果 .parent 的 高度( height ) 不写，你只需要写上内边距 padding: 10px 0; 就能将 .child 垂直居中；
  
  如果 .parent 的 height 写死了，就很难把 .child 居中，以下是垂直居中的方法。
  
  忠告：能不写 height 就千万别写 height。

```
.parent{
  border: 1px solid red;
  padding: 10px 0;
}
.child{
  border: 1px solid green;
  width: 300px;
  height: 100px;
}
```

1.table自带功能 :
```
<body>
  <table class="parent">
    <tr>
      <td class="child">
      一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
      </td>
    </tr>
  </table>
</body>
```

2.100% 高度的 afrer before 加上 inline block
HTML:
```
<div class="parent">
    <span class=before></span><div class="child">
      
      一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字一串文字
      
    </div><span class=after></span>
  </div>
```

CSS:
```
.parent{
  border: 3px solid red;
  height: 600px;
  text-align: center;
}

.child{
  border: 3px solid black;
  display: inline-block;
  width: 300px;
  vertical-align: middle;
}

.parent .before{
  outline: 3px solid red;
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
.parent .after{
  outline: 3px solid red;
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
```


3.margin-top -50%

```
.parent{
  height: 600px;
  border: 1px solid red;
  position: relative;
}
.child{
  border: 1px solid green;
  width: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -150px;
  height: 100px;
  margin-top: -50px;
}
```


4.absolute margin auto
```
.parent{
  height: 600px;
  border: 1px solid red;
  position: relative;
}
.child{
  border: 1px solid green;
  position: absolute;
  width: 300px;
  height: 200px;
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
```

5.translate -50%
```
.parent{
  height: 600px;
  border: 1px solid red;
  position: relative;
}
.child{
  border: 1px solid green;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
```


6.div 装成 table
```
.parent{
  border: 3px solid red;
  height: 600px;
  text-align: center;
}

.child{
  border: 3px solid black;
  display: inline-block;
  width: 300px;
  vertical-align: middle;
}

.parent:before{
  content:'';
  outline: 3px solid red;
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
.parent:after{
  content:'';
  outline: 3px solid red;
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
```

7.flex







