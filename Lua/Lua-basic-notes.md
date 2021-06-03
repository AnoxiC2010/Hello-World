# Lua notes

[官网](http://www.lua.org/)

# Linux安装使用

(需要make 和 gcc)

```shell
$ curl -R -O http://www.lua.org/ftp/lua-5.4.3.tar.gz
$ tar zxf lua-5.4.3.tar.gz
$ cd lua-5.4.3
$ make all test
$ make all install
```



查看版本

```shell
$ lua -v
```



进入交互模式

```shell
$ lua
```

每条命令会在回车后立即执行



退出

输入EOF控制字符（End-Of-File、POSIX环境下使用ctrl-D，Windows环境下使用ctrl-Z），或调用操作系统库的exit函数(执行os.exit())退出交互模式。



`-i`执行完程序段后进入交互模式

```shell
$ lua -i hello.lua
```



`-e`允许直接在命令行中输入代码

```shell
$ lua -e "print(math.sin(12))"	-->-0.53657291800043
```



`-l`加载库

```shell
$ lua -i -llib -e "x = 10"
```



交互模式下立即执行或加载一个文件

```lua
> dofile("hello.lua")
```

用户双窗口，一个编辑保存，然后一个执行查看结果



交互模式下如果不想输出结果，可以在行末加上和一个分号。

```lua
> io.flush()		--> true
> io.flush();
```

分号使最后一行语法上变成了无效的表达式，但仍被当作有效的命令执行。



# 词法规范

单行注释

`--`开始直到行末



多行注释

`--[[这里面都是注释的内容]]`

```
--[[
这样写的好处是，重启这段代码只需要在第一行多加一个-即可，
--]]
```



连续语句之间的分隔符不是必须，需要的话可以用`;`来分隔



标识符

任意字母、数字和下划线组成的字符串

不能以数字开头

大小写敏感

“下划线+大写字母”用作特殊用途

“下划线+小写字母”通常用做哑变量



保留字

and	break	do 	else	elseif
end	false	goto	for	function
if	in	local	nil	not
or	repeat	return	then	true
until	while



全局变量

无需声名即可使用，未初始化不会报错，结果为nil

```lua
> b		-->nil
```

把nil赋值给全局变量，Lua会回收该全局变量（使其为nil），回收该变量占用的内存

```lua
> b = 10
> b = nil
> b			--> nil
```



类型和值

Lua是动态类型语言，没有类型定义，每个值都带有其自身的类型信息。

8中基本类型：

nil (空)

boolean (布尔)

number (数值)

string (字符串)

userdata (用户数据)

function (函数)

thread (线程)

table (表)



使用type函数获取一个值对应的类型名称

```lua
> type(nil)		--> nil
> type(true)	--> boolean
> type(io.stdin)	--> userdata
> type(print)	--> function
...
```



userdata类型允许把任意C语言数据保存在Lua语言变量中。



变量没有预定义类型，任何变量都可以包含任何类型的值。

```lua
> type(a)		--> nil
> a = 10
> type(a)		--> number
> a = "a string"
> type(a)		--> string
```



nil

只有一个nil值

表示无效值(non-value，即没有用的值)



boolean

有两个值 true 和 false

但是在Lua的条件测试中会将除了boolean值的false和nil之外的所有值都视为真(包括把零和空字符串也是为真)



逻辑运算符 and、or、not

注意这里和java或c的区别还是蛮大的

and : 如果第一个操作数为"false"，返回第一个操作数，否则返回第二个操作数。遵循短路。优先级高于or。

or : 如果第一个操作数不为"false"，返回第一个操作数，否则返回第二个操作数。遵循短路。

```lua
--惯用写法
x = x or v
--等价于
if not x then x = v end
--即当x违背初始化，将其默认值设为v(假设x不是boolean类型的false)

--这两中写法等效，由于and优先级高于or
((a and b) or c)
(a and b or c)
--当b不伪(false和nil),以上写法等效c语言三目运算夫a?b:c
--如
(x>y) and x or y --选出x和y中较大的一个
```

not : 永远返回boolean类型的值

```lua
> not nil		--> true
> not 0			--> false
> not not nil	--> false
```



