# JavaScript basic learning



概念

- JavaScript 是一种轻量级的编程语言。
- JavaScript 是可插入 HTML 页面的编程代码。
- JavaScript是一种弱类型语言
- 它的解释器被称为JavaScript引擎，为浏览器的一部分
- 主要用来向HTML（标准通用标记语言下的一个应用）页面添加交互行为。 
- 跨平台特性，在绝大多数浏览器的支持下。



# 在HTML中引用JS

内部JS程序

- 在HTML文本中

  HTML 中的JS脚本(代码)必须位于` <script>` 与 `</script>` 标签之间。

外部JS程序

- `<script src="1.js"></script>`

  在引用外部JS的script标签间不能编写JavaScript代码



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
- JavaScript并没有避开数据类型，它的数据类型是根据所赋值的类型来确定的。
- JavaScript 对大小写敏感

变量名有命名规范：

- 只能由英语字母、数字、下划线、美元符号$构成，且不能以数字开头，并且不能是JavaScript保留字

(let const) 



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

- 注意: 普通的for循环在JS中一般用来遍历数组, foreach循环在JS中一般用来遍历对象



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

`toPrecision() `返回字符串值，它包含了指定长度的数字：

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
fruits. reverse();            // 对 fruits 中的元素进行排序
```



## Math对象

`Math.ceil(x)` 返回大于等于x的最小整数
`Math.floor(x)` 返回小于等于x的最大整数
`Math.random()` 返回 0 ~ 1 之间的随机数 
`Math.round(x)` 把一个数四舍五入为最接近的整数。
`Math.max(x,y,z,...,n)` 返回最高值
`Math.min(x,y,z,...,n)` 返回最低值