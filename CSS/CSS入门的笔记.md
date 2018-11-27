# CSS 层叠样式表（Cascading Style Sheets）

## 文档流
1. 文档流：简单来说就是，文档内元素的流动方向

2. 内联元素：从左往右流动！ 受到阻碍的时候（也就是一行占满时），会换行

3. 块级元素：从上往下流动，独占一行  每个另起一行
  - 变成一行的方式是：
    - 子元素全部float:left，父元素加上clearfix:after 
  - div（块级元素）高度如果不写死 由其内部文档流元素 的高度总和决定
    - div嵌套span（内联元素）： 由最高的内联元素决定
  - body 的高度是根据内部文档流元素的高度决定

注意：

1. 中文太长会**头尾分离**换行，而英文不会，会一直缩（可以理解为英文一个单词是一个整体）除非添加属性（word-break:break-all）相当于打断单词

2. 字体显示分为：上部 基线 下部
  - 两行文字里会有一个行高，行高如果不写，浏览器就取默认行高
  - font-size: 100px;  测出来这100px大概是一个单词的最高部分和最低部分
  - 若是span变为140px  ping fang字体：默认1.4倍的行高；  而Tahoma字体：默认1.21倍行高（也就是说每个字体都有每个字体不同的默认行高）


3. 内联元素的高度：文字居中就是底线居中  （基线）
  - line-height 是可以确定内联元素高度的。

关于行高可以Google：lineheight   方应杭

## 左右布局
1. 左右布局
	- 用浮动float实现左右布局。给左盒子添加`float:left`；(向左浮动)，给右盒子添加`float:right`；(向右浮动)；但是加了浮动效果，同时就要给左右盒子的父级盒子加上class名为`clearfix`，`clearfix::after{content:''; display:block; clear:both;}`
	- 用左浮动`float:left`+外边距`margin`实现左右布局。给左边盒子设置右边距`margin-right`，或者给右边盒子设置左边距`margin-left`
	- 用绝对定位`position:absolute`实现左右布局。给左右盒子的父级加相对定位`position:ralative`，给左右盒子加绝对定位`position:absolute`，同时设置`top`，`left`，`bottom`，`right`样式属性

2. 左中右布局
用浮动实现左中右布局。同上1（2）和1（3）的方法



## 居中

1. .xxx {text-align: center;} 的作用是？
  - 可以让 .xxx 子代中的**内联元素**居中

2. 我需要一个 div 高度为 30px，div 里有一行字垂直居中，字的大小为 14px，应该怎么写 CSS?
  - 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 20px; padding: 5px 0;
  - 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 24px; padding: 3px 0;
  - 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 30px
  - 总结：子元素的行高 + 子元素上下内边距 = 外层盒子高度 （实现文字垂直居中）

3. 如果你有一个div 有指定的**宽度**，可以给它 margin-left right: auto，就可以实现水平居中
  
4.https://github.com/18898286061/KaiKaiBlog/blob/master/CSS/Css%E5%AE%9E%E7%8E%B0%E5%B1%85%E4%B8%AD.md
	- 我上面这个链接的文章有对垂直居中的总结


## 浮动
1. 用 float 做横向结构需要做哪两件事？
  -. 给所有子元素加 float: left 
  -. 给父元素加 clearfix 类

clearfix的写法：
```
clearfix:after {
	content: '';
	display: block;
	clear:both;
}
```
  
  
## position

1. 定位元素（positioned element）是其计算后位置属性为 relative, absolute, fixed 或 sticky 的一个元素。
	- 相对定位元素（relatively positioned element）是计算后位置属性为 relative 的元素。
	- 绝对定位元素（absolutely positioned element）是计算后位置属性为 absolute 或 fixed 的元素。
	- 粘性定位元素（stickily positioned element）是计算后位置属性为 sticky 的元素。
  
2. `fixed` 会脱离文档流，也就是说，body的高度会减去这个fixed的高度，因它已经不是body内的文档流； 此元素会相对于屏幕固定；
  - 也不会独占一行了，会缩起来，若是我们在做topNavbar就会给width宽度100% 

3.`absolute`
	- 不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移，来确定元素位置。绝对定位的元素可以设置外边距（margins），且不会与其他边距合并。 （记住：给元素绝对定位会使它变成block）
	
4. `static`
	- 该关键字指定元素使用正常的布局行为，即元素在文档常规流中当前的布局位置。此时 top, right, bottom, left 和 z-index 属性无效。

5. `relative`
	- 该关键字下，元素先放置在未添加定位时的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

6. `sticky `
	- 盒位置根据正常流计算(这称为正常流动中的位置)，然后相对于该元素在流中的 flow root（BFC）和 containing block（最近的块级祖先元素）定位。在所有情况下（即便被定位元素为 table 时），该元素定位均不对后续元素造成影响。当元素 B 被粘性定位时，后续元素的位置仍按照 B 未定位时的位置来确定。position: sticky 对 table 元素的效果与 position: relative 相同。
	
	
## 背景图片居中：
1. background-position: center center;
2. 使背景图按比例缩放：background-size: cover;




## 使用CSS画一个三角形

1. 我给一个div边框加粗后，变成四个梯形
2. 然后我不要div的宽高。他就会变成四个三角形
3. 接着我使其他三个方向的边框透明。就会出现一个三角形；
```
div {
	border: 10px solid red;
	width: 0px;
	height: 0px;
	border-top-color: transparent;
	border-right-color: transparent;
	border-left-color: transparent;
}
```

4. 若是我需要得不是一个不是等腰三角形：调整border-xxx-width:
```
div {
	border: 20px（border值可以调整图案的大小） solid transparent;
	width: 0px;
	border-left-color: #E6686A;
	border-top-width: 0px;
}
```
注意：若是放出来以后你发现看不到，因为颜色相同了，你需要改变颜色；

5.然后又发现，三角形占了span使span撑高了起来，所以接着做绝对定位和相对定位，使三角形脱离文档流；
```
top: 100%;
left: 4px;
```
6. 更多这些小图案的画图方法Google： css tricks shape
  


## 伪类、伪元素
1. 伪类的写法 ->  `div:（一个英文冒号）`
	- 伪类在这里就用举例子的方式来说
	- 选中元素:`nth-child(odd):`可以使第单数个元素实现特定的样式
	- 选中元素:`nth-child(even):`可以使第双数个元素实现特定的样式
	- `:nth-child(1) {}` `:first-child {}`可以使第一个元素实现特定的样式



2. `伪元素的写法  -> div::（两个英文冒号）`
	- 可以去替代一个div
	- 伪元素无法选中
	- 可以认为它是一个内联元素，但是我们也可以通过display的方式让它改变为block


  
  
## 附一些小技巧：
1. max-width:  会自适应，不出现滚动条
  
2. 外面加padding的时候，整个盒模型会变形，变大起来
  - 所以我在里面多写一个div，然后给里面加padding
  
3. 对于一些图片标志：我们使用阿里巴巴的iconfont symbol 
  - 使用：复制在线生成代码，然后按照使用说明来就好
  - 改变大小：width height 
  - 颜色：fill

4. css中给属性加了display：inline-block；就同时要加vertical-align：top；
	- 因为添加了display：inline-block；容易出现bug，而加vertical-align：top；可以消除这些bug，或者说可以消除hug的出现

5. 给阴影加上过渡时间就不会显得很突兀，transition：shadow 0.2s；

  
  
  
  
  
  
  
  
  
  
  
