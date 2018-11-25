# HTML
>HTML（超文本标记语言——HyperText Markup Language）是构成 Web 世界的一砖一瓦。它定义了网页内容的含义和结构。

# 标签

## 根元素：

元素 | 描述
-----  | ---
&lt;html> |	HTML <html> 元素 表示一个HTML文档的根（顶级元素），所以它也被称为根元素。所有其他元素必须是此元素的后代。

---


## 文档元数据节：

元数据（Metadata）含有页面的相关信息，包括样式、脚本及数据，能帮助一些软件 (如搜索引擎， 浏览器等等）更好地运用和渲染页面。对于样式和脚本的元数据，可以直接在网页里定义，也可以链接到包含相关信息的外部文件。

元素 |	描述
--- | ---
&lt;link>	| HTML 中<link>元素规定了外部资源与当前文档的关系。 这个元素可用来为导航定义一个关系框架。这个元素最常于链接样式表。
&lt;meta>	| HTML <meta> 元素表示那些不能由其它HTML元相关元素 (<base>, <link>, <script>, <style> 或 <title>) 之一表示的任何元数据信息.
&lt;style>	| HTML的<style>元素包含文档的样式信息或者文档的部分内容。默认情况下，该标签的样式信息通常是CSS的格式。
&lt;title>	| HTML <title> 元素 定义文档的标题，显示在浏览器的标题栏或标签页上。它只可以包含文本，若是包含有标签，则包含的任何标签都不会被解释。

##  常见标签
1. a、form、input、button、h1、p、ul、ol、small、strong、div、span、kbd、video、audio、svg
2. 若你知道标签对应单词的意思，就知道这个标签怎么用了，因为标签都是缩写，
  - 例如：
    - 列表：（UnorderedList -> ul）
    - 锚点：（anchor -> a）
    - 段落：（paragraph -> p）
    - （这也就是语义化）
3. 除了 div 和 span（他们俩没有任何语义，所以我们一般加上class），其他标签都有默认样式，div和span都代表了划分一块区域。div块级区域，span行区域。




4. 上面就举例几个，更多可以MDN 上可以去查看所有标签的&lt;档


# 空元素：
MDN说得很头疼，其实总结一下可以说是：

```
没有闭合的标签称为空标签，如：<br/>和<img/>等。他们不存在成对的情况（我是这么理解，觉得的不对的请指正我。）
反之具有成对性质的例如：<div></div>、<form></form>就不是空标签。
在HTML中，在空标签上使用闭标签是无效的，例如：<br></br>。这样的情况是无效的HTML。
```

在 HTML 中有以下这些空元素：

>&lt;area>

>&lt;base>

>&lt;br>

>&lt;col>

>&lt;colgroup> when the span is present

>&lt;command>

>&lt;embed>

>&lt;hr>

>&lt;img>

>&lt;input>

>&lt;keygen>

>&lt;link>

>&lt;meta>

>&lt;param>

>&lt;source>

>&lt;track>

>&lt;wbr>


# 可替换标签

 在CSS里，可替换元素(replaced element)的展现不是由CSS来控制的。这些元素是一类外观渲染独立于CSS的外部对象。
典型的可替换元素有&lt;img>、&lt;object>、&lt;video>和表单元素，如&lt;textarea>、&lt;input>。某些元素只在一些特殊情况下
表现为可替换元素，例如<audio>和<canvas>。



# 历史

1. HTML有很多种版本（W3C组织制定规范）
  - HTML 4.01
  - XHTML
  - HTML 5
  - HTML 5.1

2. 规范文档（Specifications）
  - 由 W3C 写文档
  - W3C 会去根据浏览器的实际情况总结文档，并不是凭空想象

3. DOCTYPE
  - >它用来告知浏览器的解析器用什么文档标准解析这个文档。
  - 除了 HTML 5 的 DOCTYPE（&lt;!DOCTYPE html> ），其他的都很难记：https://www.w3.org/QA/2002/04/valid-dtd-list.html
  - 如果你没写 DOCTYPE，浏览器或许有时解析文档可能会出现bug. 有兴趣可以去尝试..
  

4. 省略标签

  - head可以不写，W3C标准文档有明确说明，如果你的head没有内容可以省略。
  - body也可以。理由同上。
  - html 也可以也可以，理由同上。

（不过title要写）
最简单的HTML文档：

```
<!DOCTYPE html>
<title>Hi</title>
```
  如果你想看看你写的网页合不合法，可以到W3C验证器上去验证：https://validator.w3.org/unicorn/?ucn_lang=zh-Hans
  
  
  
  
  
  
# 学习笔记
  
## &lt;dl>、&lt;dt>、&lt;dd>

因为经常记不住&lt;dl>、&lt;dt>、&lt;dd>，而且个人有觉得很有语义， 这三标签通常被称为定义性列表： 一个术语对应一条解释或定义
```
<dl>
  <dt>Firefox</dt>
  <dd>A free, open source, cross-platform, graphical web browser
      developed by the Mozilla Corporation and hundreds of volunteers.</dd>
</dl>
```
## a标签的target


值	| 描述
---| ---
_blank	| 在新窗口中打开被链接文档。
_self |	默认。在相同的框架中打开被链接文档。
_parent	| 在父框架集中打开被链接文档。
_top	| 在整个窗口中打开被链接文档。
framename	| 在指定的框架中打开被链接文档。


## a标签和form标签
  
1. a 标签发起GET请求
2. form 标签发起POST请求， 如果 input 不加 name，那么在表单提交时，input 的值就不会出现在请求里。（也就是说记得给input 加上name属性。 让服务器知道你发的POST的值，例如：&lt;input type:checkbox name="fruit" value="orange">）
3. get 请求 ，它的请求的参数放在url上
4. post 请求，它的参数会放在第四部分


## form注意事项
如果一个form 有一个button 会自动升级为submit，但是指定了type=button，那它就是普通的按钮而已了。
  

## label标签
label标签的for属性指定关联和input的id属性；（不过label包住for可以取到同样的效果，如果你写了这种包住的方法，就别写for id了）




## 单选框（radio）
单选框有同一个name时，只能选一个啦 ~


## 下拉列表（select）
selected是默认选择的属性，若是写上multiple，则能够多选，写上disabled属性的选项将会变为不可选


## 多行文本  （textarea）  
cols="30" ：定义多行文本的列数  rows="10"  ：定义多行文本的行数  （不过建议它的宽高最好去用CSS制定。因为行列不太准）； 

resize=:none ：可以让多行文本不可以拖动调整大小；

## 表格（table）

1. table标签的表格，默认会有边框，而且边框有间距。使用`table border: collapse`；  可以让collapse合并边框  border没有空隙；

2. table的子元素：
  - thead
    - th
    - tr
  - tbody
    - tr
    - td
  - tfoot
    -tr
    -td

3. colgroup 属性
元素定义表格中的一组列表，以便于进行格式化

本来用于写表格的对齐方式和宽度，不过我查了MDN，以后就用CSS写吧。最新标准已经不支持了。
`col width=100`
`col width=200`







