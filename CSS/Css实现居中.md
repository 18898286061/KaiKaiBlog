水平居中：
  内联：爸爸身上写 text-align:center;
  块级：margin-left: auto; margin-right: auto;

垂直居中：

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















