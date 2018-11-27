# CSS 层叠样式表（Cascading Style Sheets）

## 文档流
1. 文档流：简单来说就是，文档内元素的流动方向

2. 内联元素：从左往右流动！ 受到阻碍，会换行

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


## 居中

1. .xxx {text-align: center;} 的作用是？
  - 可以让 .xxx 子代中的内联元素居中

2. 我需要一个 div 高度为 30px，div 里有一行字垂直居中，字的大小为 14px，应该怎么写 CSS?
  - 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 20px; padding: 5px 0;
 `- 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 24px; padding: 3px 0;
 `- 给 div 的样式为 font-size: 14px; 子元素包起文字：line-height: 30px
  - 总结：子元素的行高 + 子元素上下内边距 = 外层盒子高度 （实现文字垂直居中）

3. 如果你有一个div 有宽度，可以给它 margin-left right: auto，就可以实现水平居中
  
  
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
  
1. 给元素定义： position:fixed 会脱离文档流，也就是说，body的高度会减去这个fixed的高度，因它已经不是body内的文档流； 此元素会相对于屏幕固定；
  - 也不会独占一行了，会缩起来

  
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
  
  
  
  
附：
1. max-width:  会自适应，不出现滚动条
  
2. 外面加padding的时候，整个盒模型会变形，变大起来
  - 所以我在里面多写一个div，然后给里面加padding
  
  
3. 对于一些图片标志：我们使用阿里巴巴的iconfont symbol 
  - 使用：复制在线生成代码，然后按照使用说明来就好
  - 改变大小：width height 
  - 颜色：fill
  
  
  
  
  
  
  
  
  
  
  
  
  
