# JavaScript basic learning



概念

- JavaScript 是一种<span style="color:red">轻量级</span>的编程语言。
- JavaScript 是<span style="color:red">可插入 HTML 页面的编程代码</span>。
- JavaScript是一种<span style="color:red">弱类型语言</span>
- 它的解释器被称为JavaScript引擎，为浏览器的一部分
- 主要用来向HTML（标准通用标记语言下的一个应用）页面添加<span style="color:red">交互行为。</span> 
- 跨平台特性，在绝大多数浏览器的支持下。



<span style="color:red">ES6(2015年, 极大的扩充了js的能力) → ECMAScript  js版本</span>



HTML CSS JS     --- 浏览器
Java          ---   虚拟机
JS            ---   node(本质: chromev8 引擎核心,:   浏览器套壳)

TypeScript:  TS.  火了. 
TypeScript  super般JavaScript
TypeScript    →  先编译成  → JavaScript



<span style="background:yellow">HTML: 不负责存储数据, 仅负责显示</span>
<span style="background:yellow">JS功能: JS在前端的角度上不负责处理复杂的数据(可以处理),  它仅仅只是用来对一些数据进行存储, 稍作改变</span>



<span style="background:yellow">交互行为</span>

<span style="background:yellow">HTML(显示)  →  点击HTML相关内容  →触发JS一些方法  → JS方法中, 又反过来修改HTML</span>



更多：

ES6 → node, webpack,

三大基础；HTML CSS JS 
三大框架：Vue   R  A 

# 在HTML中引用JS

内部JS程序

- <span style="color:red">直接写在html页面上, 用script标签包裹:  (一般我们习惯于写在靠近body的尾标签)</span>

  HTML 中的JS脚本(代码)必须位于` <script>` 与 `</script>` 标签之间。

外部JS程序

- `<script src="1.js"></script>`

  在引用外部JS的script标签间不能编写JavaScript代码
  
  <span style="color:red">JavaScript 是可插入 HTML 页面的编程代码。(最开始就是做这个事情的)</span>



# JS日志和注释

常用测试

- `alert()`
- `console.log()`

注释语句

- `//`
- `/**/`



# 变量的声明var

声明一个变量

- JavaScript在声明时统一使用无类型（untyped）的“var”关键字
- var来声明一个变量,这是一个固定的写法，是js的语法。
- <span style="color:red">JavaScript并没有避开数据类型，它的数据类型是根据所赋值的类型来确定的。不能通过声明体现。</span>
- JavaScript 对<span style="color:red">大小写敏感</span>。

变量名有命名规范：

- 只能由英语字母、数字、下划线、美元符号$构成，且不能以数字开头，并且不能是JavaScript保留字

ES6 : (let const) 



# 变量和属性

类型检测

- `typeof`表示“某某的类型”

语法：

- `typeof`变量
- `instanceof`表示"是某某类型”

e.g.
`console.log(typeof b)`



# 变量定义

String

- `var str=‘123s’;`

Number

- `var x1=34.00;` 使用小数点来写
- `var x2=34;`不使用小数点来写
- 只要是个数，那么就是数值型的，无论整浮、无论大小、无论正负，都是number类型的

Boolean

- `var x=true`
- `var y=false`

数组

- `var cars=new Array();cars[0]="Audi";`
- `var cars=new Array("Audi","BMW","Volvo");`
- <span style="color:red;">`var cars=["Audi","BMW","Volvo"];`</span>

对象

- `var person={firstname:"Bill", lastname:"Gates", id:5566};`

- 获取参数

  ```javascript
  name=person.lastname;
  name=person["lastname"];
  ```

- 添加参数并赋值

  `person.aaa = 'aaaa’`

- 注意:

  ```javascript
  var person={firstname:"Bill", lastname:"Gates", id:5566};
  var b = person
  b.lastname = 'b'
  console.log(b)      // 结果: lastname为b
  console.log(person) // 结果: lastname为b
  ```

  

# 函数和事件

函数

函数就是包裹在花括号中的代码块，前面使用了关键词 `function`：

```javascript
function functionname(parm){
	// 执行代码
}
```

e.g.

```javascript
<button onclick="myFunction(‘123’,’yyyy')">点击这里</button> 
<script> 
	function myFunction(num,str){ alert(num + str); }
</script>
```



Event事件

| [onkeypress](https://www.w3school.com.cn/jsref/event_onkeypress.asp) | 某个键盘按键被按下并松开。   |
| ------------------------------------------------------------ | ---------------------------- |
| [onkeyup](https://www.w3school.com.cn/jsref/event_onkeyup.asp) | 某个键盘按键被松开。         |
| [onload](https://www.w3school.com.cn/jsref/event_onload.asp) | 一张页面或一幅图像完成加载。 |
| [onmousedown](https://www.w3school.com.cn/jsref/event_onmousedown.asp) | 鼠标按钮被按下。             |
| [onmousemove](https://www.w3school.com.cn/jsref/event_onmousemove.asp) | 鼠标被移动。                 |
| [onmouseout](https://www.w3school.com.cn/jsref/event_onmouseout.asp) | 鼠标从某元素移开。           |
| [onmouseover](https://www.w3school.com.cn/jsref/event_onmouseover.asp) | 鼠标移到某元素之上。         |
| [onmouseup](https://www.w3school.com.cn/jsref/event_onmouseup.asp) | 鼠标按键被松开。             |
| [onreset](https://www.w3school.com.cn/jsref/event_onreset.asp) | 重置按钮被点击。             |
| [onresize](https://www.w3school.com.cn/jsref/event_onresize.asp) | 窗口或框架被重新调整大小。   |
| [onselect](https://www.w3school.com.cn/jsref/event_onselect.asp) | 文本被选中。                 |
| [onsubmit](https://www.w3school.com.cn/jsref/event_onsubmit.asp) | 确认按钮被点击。             |
| [onunload](https://www.w3school.com.cn/jsref/event_onunload.asp) | 用户退出页面。               |



# 语言逻辑

运算符

- +,-,*,/,%,++,--
- =, +=, -=, *=, /=, %=

- 注意:
  - 加法:
    `20+10+'20’`
  - 减法
    `'30'-'10'`



比较和逻辑运算符

- !=, > , <, >=, <=, ? 三元
- && , ||, !

- 注意:

  - ==

    值相等

  - ===

    值相等并且类型相等

  - ！==

    值不相等或类型不相等



逻辑语句if----else

- ```
  if (条件 1) {
    当条件 1 为 true 时执行的代码
    } else if (条件 2) {
    当条件 2 为 true 时执行的代码
    } else {
    当条件 1 和 条件 2 都不为 true 时执行的代码
    }
  ```

- 注意if非boolean数据

  ```javascript
  var myBoolean=new Boolean();
  var myBoolean=new Boolean(0);
  var myBoolean=new Boolean(null);
  var myBoolean=new Boolean("");
  var myBoolean=new Boolean(false);
  ```

  

逻辑语句switch

- ```
  switch(n) {
  case 1:
  	执行代码块 1
  	break;
  case 2:
  	执行代码块 2
  	break;
  default:
  	n 与 case 1 和 case 2 不同时执行的代码
  }
  ```



逻辑语句while

- ```
  while (条件){
  	需要执行的代码
  }
  ```

  

逻辑语句for

- 种类一:

  ```javascript
  var person={fname:"John",lname:"Doe",age:25};
  for (x  in person) {
  	alert(x)             
  	alert(person[x])
  }
  ```

- 种类二:

  ```javascript
  for (var i=0; i<5; i++) {
  	x=x + "The number is " + i + "<br>";
  }
  ```

- <span style="color:red">注意: 普通的for循环在JS中一般用来遍历数组, foreach循环在JS中一般用来遍历对象</span>



break 语句用于跳出循环。
continue 用于跳过循环中的一个迭代。



# 核心对象

## Number

`toString()` 以字符串返回数值

```javascript
var ii = 123
console.log(typeof  ii)
console.log(typeof ii.toString())
```

`toFixed()` 返回字符串值，它包含了指定位数小数的数字(四舍五入)：

```javascript
var x = 9.6544;
x.toFixed(2);           // 返回 9.65
```

`toPrecision() `返回字符串值，它包含了指定长度的数字（四舍五入）：

```javascript
var x = 9.656;
x.toPrecision();        // 返回 9.656
x.toPrecision(2);       // 返回 9.7
```

`MAX_VALUE` 返回 JavaScript 中的最大数字。

```javascript
var x = Number.MAX_VALUE;
```

`MIN_VALUE` 返回 JavaScript 中的最小数字。

```javascript
var x = Number.MIN_VALUE;
```

`parseInt()` 方法
允许空格。只返回首个数字：

```javascript
parseInt("10");         // 返回 10
```

`parseFloat()`方法。
允许空格。只返回首个数字：

```javascript
parseFloat("10.33"); // 返回 10.33
```



## String

`length` 属性返回字符串的长度

```javascript
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length;
```

`indexOf()`方法返回字符串中指定文本首次出现的索引（位置）：

```javascript
var str = "The full name of China is the People's Republic of China.";
var pos = str.indexOf("China");
var pos = str.indexOf("China", 28);
```

`indexOf()`不存在返回 -1。
`slice()`提取字符串的某个部分并在新字符串中返回被提取的部分。

```javascript
var str = "Apple, Banana, Mango";
var res = str.slice(7,13);//裁剪字符串中位置 7 到位置 13 的片段
```

`split()` 将字符串转换为数组

```javascript
var txt = "a,b,c,d,e";   // 字符串
txt.split(",");          // 用逗号分隔
txt.split(" ");          // 用空格分隔
txt.split("|");          // 用竖线分隔
```

如果分隔符是 ""，被返回的数组将是间隔单个字符的数组：



## Array

`toString()` 返回数组转换的数组值（逗号分隔）的字符串。

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
alert(fruits.toString()); //Banana,Orange,Apple,Mango
```

`pop()` 方法从数组中删除最后一个元素,返回删除的元素

```javascript
fruits.pop();              // 从 fruits 删除最后一个元素（"Mango"）
var x = fruits.pop();      // x 的值是 "Mango"
```

`push()` 方法（在数组结尾处）向数组添加一个新的元素,返回数组长度

```javascript
fruits.push("Kiwi");       //  向 fruits 添加一个新元素
```

`splice()` 方法可用于向数组添加新项, 返回([])

```javascript
fruits.splice(2, 0, "Lemon", "Kiwi");
// 第一个参数:添加新元素的起始位置。第二个参数:定义应删除多少元素。
// 其余参数（“Lemon”，“Kiwi”）定义要添加的新元素。
```

`sort()` 方法以字母顺序对数组进行排序,返回值和原数组是经过排序的数组

```javascript
fruits.sort();            // 对 fruits 中的元素进行排序
```

`reverse()`方法反转数组中的元素。返回值和原数组都变为经过反转数组

```javascript
fruits.reverse();            // 对 fruits 中的元素进行排序
```



## Math对象

`Math.ceil(x)` 返回大于等于x的最小整数
`Math.floor(x)` 返回小于等于x的最大整数
`Math.random()` 返回 0 ~ 1 之间的随机数 
`Math.round(x)` 把一个数四舍五入为最接近的整数。
`Math.max(x,y,z,...,n)` 返回最高值
`Math.min(x,y,z,...,n)` 返回最低值



# DOM

<span style="color:red">文档对象模型(document object model )</span>。

HTML → DOM树(不同对象之间相互引用构建的内存模型) → 页面HTML DOM 将 HTML 文档视作树结构。这种结构被称为节点树：DOM Tree

![image-20210415220751143](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\JavaScript-basic-learning.assets\image-20210415220751143.png)



DOM相关概念

dom树中的节点彼此拥有层级关系(由节点构成)
整个文档是一个文档节点
每个 HTML 元素是元素节点
HTML 元素内的文本是文本节点
每个 HTML 属性是属性节点
注释是注释节点
在节点树中，顶端节点被称为根（root）
每个节点都有父节点、除了根（它没有父节点）
一个节点可拥有任意数量的子节点
父、子 和同胞(兄弟或姐妹）节点。
同胞是拥有相同父节点的节点



## DOM加载顺序(html从代码→真正显示)

解析HTML结构(从上向下的过程)。
加载外部脚本和样式表文件。
解析并执行脚本代码。
构造HTML DOM模型。
加载图片等外部文件。
页面加载完毕。



一个页面是怎么产生的:

1. 通知浏览器发起请求:  告诉浏览器一个url 

2. 浏览器根据这个url发起请求， 
   判断url第二部分， 到底是个域名还是个ip地址
   如果是个域名， 做dns解析（域名 变成  ip地址）
   根据ip地址在互联网找到这个ip地址所对应的计算机
   找到这个计算机的对应端口（自带了要去那个端口， 没带找默认：80 443）

3. 服务器监听某一个端口，获得用户请求的url（一整个报文）
   服务器分析这个url，（重点分析url第三部分： 服务器内部路径+参数部分） 
   根据分析的结果， 返回对应的内容。（第一次html页面代码）

4. 浏览器会获得到服务器的返回， （第一次，拿到html页面）

5. 解析html页面 → dom树
   解析HTML结构(<span style="color:red">从上向下的过程</span>)。---实际上在解析的过程中， 边解析，一边构建dom树 

   加载外部脚本js和样式表文件css。异步加载 

   解析并执行脚本代码。Js已经加载过来， 要执行 

   <span style="color:red">构造HTML DOM模型。→ 完成构建 → 立即显示</span>
   加载图片等外部文件。 页面加载完毕。



## 获得节点

获得节点的常用方法
getElementById()
getElementsByName()
getElementsByTagName()



## 添加节点

Element 对象表示 HTML 元素(节点)对象
Element 对象是拥有类型为元素节点、文本节点、注释节点等子节点的节点(标签)。
部分方法

element.appendChild()向元素添加新的子节点，作为最后一个子节点

```html
//注意ul所辖范围内标签之间有换行或空格就会多一个文本子节点
<ul id="myList"><li>Coffee</li><li>Tea</li></ul>
<input id="input"> 
<button onclick="myFunction()">添加</button>
<script>
function myFunction(){
  var node=document.createElement("LI");
  var input=document.getElementById("input")
  var textnode=document.createTextNode(input.value);
  node.appendChild(textnode);
  document.getElementById("myList").appendChild(node);
}
</script>
```



## 删除节点

element.removeChild():从元素中移除子节点

```html
<ul id="myList"><li>Coffee</li><li>Tea</li><li>Milk</li><li>1</li><li>2</li><li>3</li></ul>
节点位置:<input id="input"> 
<button onclick="myFunction()">删除</button>
<script>
function myFunction(){
    var input=document.getElementById("input")
    var list=document.getElementById("myList");
    list.removeChild(list.childNodes[input.value]);
}
</script>
```



## 替换节点

element.replaceChild():替换元素中的子节点。

e.g. 1

```html
<ul id="myList"><li>Coffee</li><li>Tea</li><li>Milk</li></ul>
节点位置:<input id="inputN"> 替换内容:<input id="inputS"> 
<button onclick="myFunction()">试一下</button>
<script>
function myFunction(){
    var inputN=document.getElementById("inputN")
    var inputS=document.getElementById("inputS")
    var textnode=document.createTextNode(inputS.value);
    var item=document.getElementById("myList").childNodes[inputN.value];
    item.replaceChild(textnode,item.childNodes[0]);
}
</script>
```

e.g. 2

```javascript
var inputN=document.getElementById("imgchange")
var img=document.createElement("img");
img.src=“----";
inputN.replaceChild(img,inputN.childNodes[1]);		
```



## INNER

Element 对象表示 HTML 元素(节点)
Element 对象是拥有类型为元素节点、文本节点、注释节点的子节点。
部分方法

<span style="color:red">innerText&innerHTML (一个结点的内部内容)</span>

```html
function changeLink(){
document.getElementById('aaa').innerHTML="<H1>cskaoyan</H1>";
document.getElementById('aaa').innerText="<H1>cskaoyan</H1>";
document.getElementById('aaa').href="http://www.cskaoyan.com";
document.getElementById('aaa').target="_blank";
}
</script>
</head>
<body>
<a id="aaa" href="http://www.microsoft.com">123</a>
<input type="button" onclick="changeLink()" value="更改链接">
```

