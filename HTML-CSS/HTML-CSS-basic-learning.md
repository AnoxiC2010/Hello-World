# HTML basic

了解前端基本知识, 了解Vue语法以及Vue开发流程

## 互联网三要素

1. <span style="color:red">url:  统一资源定位符</span>

   <span style="color:red">url是分为三部分(四小部分): 1, 协议  2, 域名或者ip+端口  3, 服务器内部路径 </span>

   <span style="color:red">四小部分: 3, 服务器内部路径  → 服务器内部路径 + 参数</span>

   默认处理机制: http协议默认找80端口 https: 默认找443

   服务器内部路径, 就是标记, 资源在目标计算机中的虚拟地址

2. http: 数据传输格式

3. html: 描述论文格式

html最开始是设计用来描述论文格式的



## url

统一资源定位符

url是分为三部分(四小部分): 1, 协议  2, 域名或者ip+端口  3, 服务器内部路径 

四小部分: 3, 服务器内部路径   服务器内部路径 + 参数

# lHTML概念

HTML(hyper text markup language)超文本标记语言

- 是一个以.html为后缀的文本

- 是一个文本,也是一个网页;该文本可以用浏览器打开

  (浏览器打开.html为后缀的文本, 会解析里面的代码--会渲染出页面)

- '超文本':包含文本字体,图片,链接,甚至音乐,程序等元素的文本

- HTML 是一个使用'标签'来描述网页的文本 (标签在HTML文本中有实际意义)

```
超文本标记语言 html是一个语言: 
是一个文本语言
是一个用标记/标签 来描述的文本语言

一个偏标准的编程语言应该具有哪些特点? 
变量的定义
运算符
分支和循环结构...

学习html就是学习html的标签的含义
```



## 开发工具

- 记事本
  - EidtPlus
  - Notepad++
- 前端专业版
  - Vscode
  - WebStorm
  - Sublime
  - Hbuilder



# HTML结构

HTML标准结构模板

```html
<html>
	<head>
	</head>
	<body>
	</body>
</html>
```

结构特点

- 由标签描述的文本

- 标签成对出现,有开始和结束

  <标签名></标签名>. 不是绝对的

- 文本由`<html>`标签包裹

- 文本分为两个部分(分别由头尾标签包裹)

- 标签嵌套: 通过标签的层级嵌套, 来描述结构关系(父子关系)



HTML重要body标签

```html
<hr> 
<br>
<h1>-<h6> 
<div>
<a> 
<p>
<img> 
<input>
< textarea > 
< select >
<ol>--<ul>
< table >--<tr>---<td>---<th> 
< form >
```



Html就是由标签构成
Html用标签来描述文档

一个Html文档分为两部分: Head body
标签也分为两大类:  <span style="background:yellow;">body子标签</span>,  head的子标签
现在: 组件化前端时代
body控制显示

xhtml: 已经夭折

W3C曾疯狂给html扩充标签, 添加属性  →  CSS（取而代之）

```html
<!DOCTYPE html>   html5语法声明
<html lang="en">     
    <head>
        <meta charset="UTF-8"> meta标记一些网页元信息
        <title>Title</title>
    </head>
    <body>
        
    </body>
</html>
```



# HTML(body)标签分类

## 块级标签

特性

- 独占一行，每一个块级元素都会从新的一行重新开始，从上到下排布
- 可以直接控制宽度、高度以及盒子模型的相关css属性
- 在不设置宽度的情况下，块级元素的宽度是它父级元素内容的宽度
- 在不设置高度的情况下，块级元素的高度是它本身内容的高度
- 块级元素是指本身属性为display:block;的元素。
- 通常使用块级元素来进行大布局（大结构）的搭建

常见的块级元素：

div、p、h1、 h2、 h3、 h4、 h5、 h6，ol、ul、dl、li、form、table



## 行级标签

特性

- 和其他行级元素从左到右在一行显示
- 不能直接控制宽度、高度以及盒子模型的相关css属性，(但是直接设置内外边距的左右值是可以的)
- 行级元素的宽高是由本身内容的大小决定（文字、图片等）
- 行级元素只能容纳文本或者其他行级元素（不要在行级元素中嵌套块级元素）
- 行级元素是指本身属性为display:inline;的元素。
- 通常使用行级元素来进行文字、小图标（小结构）的搭建。



常见的行级元素：

- span 常用行级容器，定义文本内区块;
- a 锚点;
- b 加粗;     strong 加粗强调;    i 斜体;    strike 中划线; 
- br 强制换行;   u 下划线;
- textarea 多行文本输入框; 
- input 输入框;
- select 下拉列表
- img 引入图片



## 行内块标签

特性

- 本质属于行级元素
- 可以与其他行级元素共处一行
- 可以设置宽高、内外边距
- 属性为display:inline-block;的元素。



常见的行内块元素：

- input 输入框
- img 引入图片



可以在行内样式或css样式中改变元素的display将三种元素进行转换。

- display: block；(将元素变为块级元素)
- display: inline； (将元素变为行级元素)
- display: inline-block；(将元素变为行级块元素)



## HTML重要标签

### `<hr> `

标签在 HTML 页面中创建一条水平线。

`<hr>` 标签没有结束标签。

标签属性

- `noshade`:颜色是否有阴影(纯色)
- `size`:高度( 厚度,不同于height,不带单位时--默认px )
- `width`:宽度(不带单位时--默认px)
- `align`:对其方式

```html
eg:
<hr>
<hr />: 这种写法不对, xhtml语法遗留产物
<hr noshade="noshade">
<hr noshade="noshade" size="50">
<hr noshade="noshade" size="50" width="50">
<hr noshade="noshade" size="50" width="50%">
<hr noshade="noshade" size="50" width="50%" align="left">
```

注意: html和css, 长度单位
px  像素,  逻辑像素

### `<br>`

可插入一个简单的换行符。

标签是单标签（意味着它没有结束标签，因此这是错误的：`<br></br>`）

`<br>` 标签只是简单地开始新的一行



<span style="color:red">注意: 在html代码上连续的空格或者换行, 在页面最终显示的时候, 仅会渲染出一个空格</span>
注意: html有一些特殊字符(不需要记忆, 只需要知道)

### `<h1>...<h6>`

`<h1>` - `<h6>` 标签可定义标题。

`<h1>` 定义最大的标题。`<h6>` 定义最小的标题。

属性`align`

- `left`:左对齐内容（默认值）
- `center`:右对齐内容
- `right`:居中对齐内容

<span style="color:red">注意1: 标题标签只有这六个</span>
<span style="color:red">注意2:  h1标签在页面上仅能出现一次(h1可以充当关键字), h2-h6可以出现多次</span>

网络爬虫, seo优化



### `<div>`

`<div>` 可定义文档中的分区或节。

`<div>` 标签可以把文档分割为独立的、不同的部分。

属性`align`

- `left`:左对齐内容（默认值）
- `center`:右对齐内容
- `right`:居中对齐内容



### `<p>`

`<p>` 标签定义段落。

`<p>` 元素会自动在其前后创建一些空白。浏览器会自动添加这些空间，您也可以在样式表中规定。



### `<a>`

`<a>` 标签定义超链接，用于从一张页面链接到另一张页面。

`<a>` 元素最重要的属性是 `href` 属性，它指示链接的目标

`<a href="http://www.baidu.com.cn">baidu</a>`

属性`href`: 指示打开的页面路径 (本质是个url)

属性`target`: 表示打开的方式

- 超链接属性`target`的`value`值时默认是`_self``

- ``_blank`:在新窗口打开

- `_parent`:在父窗口打开

- `_self`:它使目标文档显示在超链接所在框架或者窗口中

- `_top`:在顶级窗口打开

- 同组值:通过单击一个窗口中的不同链接控制另一窗口内容变化 ; 浏览器会找与`target`值相符的框架或者窗口中的文档，有则在其中显示文档。如果不存在，浏览器打开一个新窗口

  ```html
  _blank _self 同组值
  Eg:
  <a href="http://www.w3school.com.cn">默认</a>
  <a href="http://www.w3school.com.cn" target="_blank">_blank</a>
  <a href="http://www.w3school.com.cn" target="_self">_self</a>
  <hr>
  <li><a href="http://www.w3school.com.cn" target="view1">1</a></li>
  <li><a href="http://www.w3school.com.cn" target="view1">2</a></li>
  ```

  ```html
  _parent _top
  Eg:
  
  A.html
  <body bgcolor="red">
  --------a-------<br>
  <iframe src="b.html" width="800px" height="800px"></iframe>
  </body>
  
  b.html
  <body bgcolor="darkgrey">
      --------b-------<br>
      <iframe src="c.html" width="600px" height="600px"></iframe>
  </body>
  
  c.html同上
  
  d.html
  <body  bgcolor="#b8860b">
  --------d-------<br>
  <!--<a href="http://www.baidu.com" width="400px" height="400px">默认</a>-->
  <!--<a href="http://www.baidu.com" target="_self" width="400px" height="400px">_self</a>-->
  <!--<a href="http://www.baidu.com" target="_parent" width="400px" height="400px">_parent</a>-->
  <a href="http://www.baidu.com" target="_top" width="400px" height="400px">_top</a>
  </body>
  ```

  

### `<img>`

图片标签

属性

- `src` 图片地址

- `width`
- `height`
- 单位 px, %

eg:
`<img src=""  alt="加载错误" width="200" height="200" />`



<span style="color:red;background:yellow">注意: </span>
<span style="color:red;background:yellow">在前端中, 绝对路径, 不再是以盘符开头的路径.</span>
<span style="color:red;background:yellow">前端中本质上只有url,</span>
<span style="color:red;background:yellow">所谓的url就是前端的绝对路劲</span>

### `<input>`

<span style="color:red;background:yellow">\<input\>是一个表单元素</span>

`<input>`标签用于搜集用户信息。

根据不同的 type 属性值，输入字段拥有很多种形式。输入字段可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等等。

属性`type`

- `text`:默认文本

- `password`:密码

- `button`:按钮

- `hidden`:隐藏输入框

- `radio`:单选框

- `reset`:定义重置按钮

- `submit`:提交

  ```html
  <input type="text" name="email" />
  <input type="password" name="pwd" />
  <input type="hidden" name="country" value="Norway" />
  <input type="radio" name="sex" value="male" /> Male
  <input type="radio" name="sex" value="female" /> Female
  <form action="form_action.asp" method="get">
  	<input type="text" name="email" />
  	<input type="reset" />
  	<input type="submit" />
  </form>
  
  ```

  

### `<textarea>`

<span style="color:red;background:yellow">\<textarea\>是一个表单元素</span>

`<textarea>` 标签定义多行的文本输入控件。

可以通过 `cols` 和 `rows` 属性来规定 `textarea` 的尺寸

属性

- `cols`:列

- `rows`:行

- `maxlength`:最大

- `placeholder`:提示

  ```html
  <textarea placeholder="请输入..." rows="5" cols="100" maxlength="50" >
  </textarea>`
  ```

- `readonly` :只读

  ```html
  <textarea placeholder="请输入..." rows="5" cols="100" maxlength="50" readonly>
  </textarea>
  ```

  

### `<select>`

<span style="color:red;background:yellow">\<select\>是一个表单元素</span>

选择下拉框

属性

- option:`selected`(`selected="selected"`选中状态)

- select :`multiple`(`multiple =“multiple“` 允许多选)

  按住`shift`+鼠标单击多选

- select :`size`(`size =“4”`下拉可见数4)

```html
eg:
<select multiple="multiple" size="1" >
    <option value=“aaa">aaa</option>
    <option value=“bbb">bbb</option>
    <option value=“ccc" selected="selected">ccc</option>
    <option value=“ddd">ddd</option>
</select>
```



### `<ol>`

`<ol>` 标签定义有序列表(有序:数字,字母)
属性:

- `type`:规定在列表中使用的标记类型(1,A,a,I,i)。

- `start`:规定有序列表的起始值

- `reversed`:规定列表顺序为降序。(9,8,7...)

  ```html
  Eg:
  <ol type="A" start="10" reversed>
      <li>1</li>
      <li>2</li>
      <li>3</li>
  </ol>
  ```

  

### `<ul>`

标签定义无序列表(圆点,空心点)

属性`type`:

- `disc`:默认值。实心圆。
- `circle` :空心圆。
- `square` :实心方块。

```html
Eg:
<ul type="circle">
    <li>Coffee</li>
    <li>Tea</li>
    <li>Milk</li>
</ul>
```



### `<li> `

`<li>` 标签定义列表项目。

`<li>` 标签可用在有序列表 (`<ol>`) 和无序列表 (`<ul>`) 中。



### `<table>`

`<table>` 标签定义 HTML 表格。

成套标签，结合标签`<tr><td><th>`标签使用

一个table标签代表一个表格, 一个tr标签代表一行, 一个td标签代表一行中一个单元格

主要属性

- `bgcolor`
- `border`
- `cellpadding`:规定单元边沿与其内容之间的空白。
- `cellspacing`:规定单元格之间的空白。

```html
Eg:
<table border="1px" bgcolor="red" cellpadding="30" cellspacing="100">
    <tr>
        <td>11</td>
        <td>12</td>
    </tr>
    <tr>
        <td>21</td>
        <td>22</td>
    </tr>
</table>
```

补充: th: 表头取代td  thead, tbody, tfoot 

注释：如果您使用 thead、tfoot 以及 tbody 元素，您就必须使用全部的元素。它们的出现次序是：thead、tfoot、tbody，这样浏览器就可以在收到所有数据前呈现页脚了。您必须在 table 元素内部使用这些标签。

```html
<table border="1">
  <thead>
    <tr>
      <th>Month</th>
      <th>Savings</th>
    </tr>
  </thead>

  <tfoot>
    <tr>
      <td>Sum</td>
      <td>$180</td>
    </tr>
  </tfoot>

  <tbody>
    <tr>
      <td>January</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>
```



### `<tr>`

`<tr>` 标签定义 HTML 表格中的行。

`<tr>` 元素包含一个或多个 <th> 或 <td> 元素。

属性

- bgcolor
- valign:规定表格行中内容的垂直对齐方式。
  - top
  - middle
  - bottom



### `<td>`

`<td>` 标签定义 HTML 表格中的标准单元格。

属性

- `colspan`:横跨列数
- `rowspan`:横跨行数

```HTML
Eg:
<table border="1px" bgcolor="red" cellpadding="30" cellspacing="100">
    <tr>
        <td colspan="2">11</td>
    </tr>
    <tr>
        <td rowspan="2">21</td>
        <td>22</td>
    </tr>
    <tr>
        <td>12</td>
    </tr>
</table>
```



### `<form>`

<span style="color:red;background:yellow;">表单元素常用于表单提交数据, 表单元素有一个默认的属性value</span>

表单

`<form>`用于与服务器端的交互。

```html
Eg:
<form action=“url" method="get">
    <p>First name: <input type="text" name="fname" /></p>
    <p>Last name: <input type="text" name="lname" /></p>
    <input type="submit" value="Submit" />
</form>
```

