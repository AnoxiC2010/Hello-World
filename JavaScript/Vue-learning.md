# Vue notes

# Vue 是什么

Vue.js------ 渐进式框架。与其他重量级框架不同的是，Vue 采用<span style="color:red">自底向上增量开发</span>的设计。Vue 的<span style="color:red">核心库只关注视图层</span>，并且非常容易学习，非常容易与其它库或已有项目整合。另一方面，Vue 完全有能力驱动采用单文件组件和Vue生态系统支持开发复杂单页应用。

- 渐进式:从核心到完备的全家桶
- 增量:从少到多,从一页到多页,从简单到复杂
- 单文件组件: 一个文件描述一个组件
- 单页应用: 经过打包生成一个单页的html文件和一些js文件

Vue 基本语法(简单)

- 简洁,轻量,快速,数据驱动,模块友好,组件化

Vue 各种插件(完善)

- Vuex , vue-router, axios …



Vue 优缺点

- Vue 不缺入门教程，可是很缺乏高阶教程与文档。
- Vue不支持IE8
- 量级问题(angular和react)



一个Vue 页面(在HTML中)以及插值表达式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue</title>
    <!--引入vue.js最好放在head部分引入（避免页面抖屏）-->
    <script src="./vue.js"></script>
</head>
<body>
<!--<div id="root" > hello world</div>-->
<div id="root">{{msg}}</div>

<script>
    new  Vue({
        el:"#root",
        data:{
            msg: "hello date"
        }
    })
</script>

</body>
</html>
```



# Vue基础’v-**:’指令

## V-bind

`v-bind:`单向绑定
用来绑定数据和属性以及表达式，缩写为’`：`’
示例

```html
<div v-bind:title="text" >hello</div>
<div v-bind:title="'***'+text" >hello</div>
<input type="text" v-bind:value="text">
new Vue({
    el:"#root",
    data:{
        text:"this is ***"
    },
})
```



## V-model

`v-model:`实现双向数据绑定

示例

输入框:

```html
<input type="text" v-model=" text" /> 
<input type="text" v-model:value ="text"/>
 <div >{{text}}</div>
```

单选:

```html
1:<input type="radio" value="one" v-model="text" />
2:<input type="radio" value="two" v-model="text" />
  <div >{{text}}</div>
new Vue({
    el:"#data2",
    data:{
    	text:"-----"
    },
})
```



## V-text

`v-text`类似更新元素的 innerText

示例
输入框:

```html
<h1 v-text="number"></h1>
new Vue({
    el:"#root",
    data:{
        msg: "hello data",
        number:123,
        me:"li"
    }
})
```



## V-html

`v-html`更新元素的`innerHTML`
示例
输入框:

```html
<h1 v-html="number"></h1>
new Vue({
    el:"#root",
    data:{
        msg: "hello data",
        number:123,
        text:"<h1>test</h1>“
    }
})
```



## V-show

`v-show`标签控制隐藏,  (`display`设置`none`)
示例

```html
<div  v-if="show">is show</div>
<button @click="aaa" >button</button>
new Vue({
    el:"#event",
    data:{
    	show:true       
    },
    methods:{
        aaa:function () {
        	this.show = !this.show
        }
    }
})
```



## V-if

`v-if`根据表达式的值的真假条件渲染元素。
`v-else-if` , `-else`
示例

```html
<div  v-if="ifData === '1'">输入1</div>
<div  v-else-if="ifData === '2'">输入2</div>
<div  v-else>输入其他值</div>
<input type="text" v-model="ifData">
new Vue({
    el:"#if",
    data:{
    	ifData:'1'
    }
})
```



## v-if和v-show区别

`v-show`不同于`v-if`
如果`v-if=false` 页面是把`v-if`所在标签整个标签取消在DOM中
而`v-show=false`则是把这个标签隐藏，但是这个标签还存在与DOM树上



## V-for

`v-for:`基于源数据多次渲染元素或模板块(循环渲染元素)。

示例

```html
<!--item表示数组元素，index是数组下标，这里把key绑到数组下标了，实际开发中后台数据库返回的数据一般都有唯一id，常把key绑到元素对象的id上-->
<li v-for="item of alist" :key="item">{{item}}</li>
<li v-for="(item,index) of alist" :key="index">{{item}}</li>
new Vue({
    el:"#for",
    data:{
    	alist:[1,2,3,5,4]
    }
})
```

<span style="color:red">注意1: 对于一个v-for指令, 这个指令写在那个html标签上循环渲染的就是这个标签</span>

<span style="color:red">注意2: 对于v-for指令渲染除了元素, 每一个元素都要有一个key, 并且key要唯一 </span>

`:key`

<span style="background:yellow">这个key值不是给程序员用, 是Vue的V-for指令底层渲染要使用</span>

<span style="color:red">注意3: 对于v-for的遍历 可以用in/of</span>

```html
<!--aaa为数组元素，tag为数组下标，key绑到元素对象的id上-->
<li v-for="(aaa, tag)  of  list"  :key="aaa.id">
<li v-for="(aaa, tag)  in  list"  :key="aaa.id">
```



## V-on和Vue方法

`v-on:`绑定事件监听器。可简写`@`
示例

```html
<div v-on:click="handleClick">{{text}}</div>
methods :
new Vue({
    el:"#event",
    data:{
    	text:"hello"
    },
    methods:{
        handleClick:function () {
        	alert("谁在点击我")
        }
    }
})
```



## V-pre

`v-pre`可以用来阻止预编译，有`v-pre`指令的标签内部的内容不会被编译，会原样输出。

示例

```html
<li v-pre >{{item}}</li>
new Vue({
    el:"#for",
    data:{
    	item :’123’
    }
})
```



## V-cloak

`v-cloak:`延迟显示。
这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 标签直到实例准备完毕
示例

```html
<style>
	[v-cloak] {
		display: none;
	}
</style>

<div id="if">
    {{aa1}}
    4444
</div>

<script>
setTimeout(() => {
    let vm = new Vue({
        el: '#if', // 根元素或挂载点。
        data: {
            aa1:'123'
        }
    })
}, 3000);
</script>
```



## V-once

`v-once:`只渲染元素和组件一次。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。
示例

```html
<a v-once>{{aa1}}</a>
<a>{{aa1}}</a>
<button  @click="aa">点击</button>
new Vue({
    el:"#if",
    data:{
    	aa1:1
    },
    methods: {
        aa: function () {
        	this.aa1 = this.aa1 + 1
        }
    }
})
```



# 计算属性

`computed:`指的是一个属性通过其他属性计算而来。
示例

```html
first:<input  v-model="firstName" >
last:<input v-model="lastName">
<div>{{full}}  </div>

<script>
new Vue({
	el:"#add",
	data:{
		firstName:0,
		lastName:0
	},
	//计算属性
	//computed：指的是一个属性通过其他属性计算而来
	computed:{
		full: function () {
			return parseInt(this.firstName) + parseInt(this.lastName)
		}
	}
})
</script>
```



# 侦听器

`watch:`监听一个属性改变触发一个事件。
示例

```javascript
new Vue({
    el:"#add2",
    data:{
    	firstName:0,
    	lastName:0
    },
    computed:{
        full: function () {
       	 return parseInt(this.firstName) + parseInt(this.lastName)
        }
    },
    watch:{
        firstName:function () {
        	this.count++
        },
        lastName:function () {
        	this.count++
        },
        full() {
       		console.log('full')
        }
    }
})
```



# 模板

`template:`一个字符串模板作为 Vue 实例的标识使用。模板将会替换挂载的元素。挂载元素的内容都将被忽略.
示例

```html
<div id="root1">
	123
</div>

<script>
    new  Vue({
        el:"#root1",
        template:'<h1>-->{{msg}}</h1>',
        data:{
       	 msg: "hello data"
        }
    })
</script>
```

<span style="color:red">当Vue对象拥有了模板之后, 所有的HTML代码, JS 代码, 都可以转移到一个Vue对象中实现  → 1一个好处, 一个Vue对象可以管控自己的所有HTML,JS 代码</span>



# 组件

`component:`组件 

一个Vue对象就是一个组件

组件树:
组件套组件.
大组件引入小组件, 小组件引入更小的组件 → 组件树

![image-20210416195816348](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-notes.assets\image-20210416195816348.png)示例

```html
<todo></todo>
123
<to-do></to-do>

<script>
    //定义局部组件
    var  todo = {
        template: '<li>todo</li>'
    }
    new Vue({
        el:"#to",
        data:{
        },
        //引用（注册）局部组件
        components:{
            'todo':todo,
            'ToDo':todo
        }
    })
</script>
```



# Vue生命周期

什么是生命周期
从开始创建、初始化数据、编译模板、挂载DOM、渲染→更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。

生命周期函数

- `beforeCreate`
- <span style="background:yellow">`created` (去后台请求数据的时机)</span>
- `beforeMount`
- <span style="background:yellow">`mounted` (去后台请求数据的时机)</span>
- `beforeUpdtae`
- `updated`
- `beforeDestroy`
- `destoryed`

```javascript
beforeCreate:function () {
	console.log("控制台打印:beforeCreate")
},
created:function () {
	console.log("控制台打印:created")
},
beforeMount:function () {
	//页面还未被渲染
	console.log(this.$el),
	console.log("控制台打印:beforeMount")
},
mounted:function () {
	//页面渲染完成
	console.log(this.$el),
	console.log("控制台打印：mounted")
},
beforeDestroy:function () {
	console.log("控制台打印：beforeDestory")
},
destroyed:function () {
	console.log("控制台打印：destroyed")
},
beforeUpdate:function () {
	console.log("控制台打印：beforeUpdate")
},
updated:function () {
	console.log("控制台打印：updated")
}。
/*
在不关闭页面的情况下测试销毁Vue对象的方法（借助浏览器的功能）
1.给Vue对象个引用比如var root = new Vue({...})
2.浏览器console输入root.$destroy()并确认
3.查看输出
*/
```



# 前后端分离

解耦

```
java服务器 （对数据稍作处理）<=> 数据库
↑ ↓
↑ ↓获取数据
浏览器 （负责把数据和html相关代码合到一起，得到完整页面）
↓ ↑获得HTML相关代码，根据获得的HTML代码提示，找对应的数据
↓ ↑
前端服务器 （HTML JS CSS 基本图片）
```

没有最好, 只有最合适.



# Vue项目构建

## Vue开发流程

Vue的环境配置启动

Java 
Jvm开发环境 → idea 开发工具 → idea配置插件 → 用idea创建创建项目

Vue: (js)
Node(js运行环境) → vue-cli “脚手架”  开发vue项目的idea → webpack (模块打包机)

Maven: 
百万文件
Npm



拿到新项目后

1. 装包(在 这个项目路径下, 执行 cnpm install)
2. 启动



项目目录

- `config/index.js` webpack配置（也是这个项目的基础配置）
- `index.html` 项目入口
- `package.jason` 包文件版本/命令启动方式



`config/index.js`里的`proxyTable: {}`属性用于同源阻塞处理

浏览器的同源阻塞策略：跨域（ip or 端口 不一致浏览器会阻塞从服务器的请求结果）

```
后端服务器（这样需要服务器做认可不同ip的某种操作）
↑ ↓
浏览器		或者 浏览器<=>前端服务器<=>后端服务器
↑ ↓
前端服务器
```



在Vue项目中实现组件嵌套

Vue的组件化

## node

安装node

- https://nodejs.org/en/
- 默认安装方式
- 检查
  - node –v
  - npm –v

安装cnpm

- npm install -g cnpm --registry=https://registry.npm.taobao.org
- cnpm -v



## 初始化项目--准备工作

安装vue-cli

- cnpm install -g @vue/cli    (3.X)
- cnpm install -g @vue/cli-init    (2.X桥接工具:脚手架)
- vue -V (v必须大写)

安装webpack

- cnpm install -g webpack   (安装webpack)



初始化项目--创建项目

真正的创建项目

- vue init webpack my-project    (2.X)
- 拒绝npm安装
- cnpm install

![image-20210417092943412](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\image-20210417092943412.png)

![image-20210417092949958](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\image-20210417092949958.png)

![image-20210417092956483](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\image-20210417092956483.png)



## 多组件和标准Vue页面

```
<template>
  <div class="helloworld">
    <hello-header :text="text1"></hello-header>
  </div>
</template>
<script>
import HelloHeader from './HelloWorld/HelloHeader'
export default {
  name: 'HelloWorld',
  components: {
    HelloHeader: HelloHeader
  },
  data () {
    return {
      text1: 'HelloHeader'
    }
  }
}
</script>
<style scoped>
</style>

```

```
<template>
    <div id="helloheader">
      {{text}}-->
    </div>
</template>

<script>
export default {
  name: 'HelloHeader',
  props: {
    text: String
  }
}
</script>

<style scoped>

</style>

```



## 父子组件传值

父组件向子组件传值

父传递: `<right :url="url"></right>`
子接收: `props: ['url’]`

```javascript
父组件’递’数据: 通过单向绑定
<body1 class="body1" v-bind:aaaa="applist"></body1>

子组件”接”数据: 
// 专门用来接收父组件穿过来的值
// 可以当做data看待, 和data就有一个差别,
// (props数据是父组件传来的, 不可修改, 即使修改, 也要是父组件修改)
props:['aaaa'],
```



子组件向父组件传值
父接收:`<left @changeurl="changeurl"></left>`
子抛出: `this.$emit('changeurl', url)`

```javascript
子组件向上传递:
// this.$emit 就是用来子组件向父组件传值的
// 第一个参数, 向父组件抛出的方法名
// 第二个参数, 就是这个方法携带了什么参数
this.$emit("deletelist", index)

父组件接收
<body-son2 class="son2"  v-bind:bodyson="aaaa" v-on:deletelist="deletebody1"></body-son2>
```

高耦合，需要Bus传值简化



## VueBus( Vue事务总线)

`vue bus`可以实现不同组件间、不同页面间的通信，比如我在A页触发发点击事件，要B页面发生变化
一个中央事件总线bus，可以作为一个简单的组件传递数据，用于解决跨级和兄弟组件通信问题

<span style="color:red">思想: 我们独立的创建一个数据的中转位置, 让这个中转位置和所有的需要数据传值的组件(Vue对象)建立联系, 传值就以中转站为媒介来进行</span>

<span style="background:yellow">引入第三方插件</span>:

1. <span style="background:yellow">导包或者导入第三方配置文件</span>
2. <span style="background:yellow">在main.js做全局配置</span>
3. <span style="background:yellow">使用</span>



创建Bus.js (src/bus/Bus.js)

- ```javascript
  import Vue from 'vue'
  const Bus = new Vue()
  export default Bus
  ```

使用 emit   on   off 

```javascript
import Bus from './Bus'
export default {
    data() {
        return {
        	.........
        }
    },
    methods: {
        ....
        Bus.$emit('log', 120)
    },
}
```

```javascript
import Bus from './Bus'
export default {
    data() {
        return {
            .........
            }
      },
    mounted () {
       Bus.$on('log', content => { 
          console.log(content)
        });    
    }    
}
```



其它注册方式

- main.js中

  ```
  import bus from './vuebus/Bus'
  Vue.prototype.Bus = bus
  ```

- 使用

  ```
  this.Bus.$emit('clickTest', '我在footer点击’)
  this.Bus.$on('clickTest', content => {
        this.text = content
      })
  ```

  

## Axios与json

什么是json

- JSON 指的是 JavaScript 对象表示法（JavaScript Object Notation）
- JSON 是轻量级的文本数据交换格式
- JSON 独立于语言 *
- JSON 具有自我描述性，更易理解
- *JSON 使用 JavaScript 语法来描述数据对象，但是 JSON 仍然独立于语言和平台。JSON 解析器和 JSON 库支持许多不同的编程语言。

http

- HTTP协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的WWW文件都必须遵守这个标准。
  HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

前后台交互
接口的概念



什么是axios

- axios 是一个基于 promise 的 HTTP 库，在浏览器和 node.js 中使用。
- axios主要是用于向后台发起请求的，还有在请求中做更多是可控功能。

使用axios

- npm install axios  --save
- main.js
  - import axios from 'axios’
  - Vue.prototype.$axios = axios
- this.$axios.get('/terms')
        .then(this.res1Method).catch((err) => {
          this.catchMethod(err)
        })



## Vue项目(前端)部署上线

打包

- npm run build
- dist



前端资源了解

cnpm install element-ui --save

```javascript
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI);
```



iconfont



googlefont



# Vue项目创建流程

对比Java开发的项目创建流程

​    1. 首先我们安装了jvm, 这个是我们java代码运行的环境

​    2. 之后我们安装idea, 这是一个帮我们构建以及管理java项目的工具

​    3. 紧接着我们在idea里new Project 创建一个javase项目

​    4. 之后我们在idea所创建的java项目中写java代码

 

那么对应的, 如果我们创建一个Vue项目,并且想写一些Vue代码也需要上述流程

​    1, 我们需要安装node , node是js的运行环境(node的本质是个浏览器)

​    这一步等价于我们java安装jvm

​    2, 我们需要安装cnpm, (node自带npm工具, 但是我们没法快速访问国外内容, 所以需要安装中国镜像访问工具)(npm 或者cnpm 类似我们java的包管理工具Maven )

​    3, 安装vue-cli , 这是一个Vue项目构建工具

这一步等价于我们java安装idea软件

​    4, 安装webpack, 这是一个包管理工具

这一步等价于我们写java代码时, 在idea里安装一些插件

​    5, 创建Vue项目

这一步等价于我们写java代码时, 用idea创建项目

​    

## 具体步骤

## 安装node

根据电脑类型,在node官方网站下载node, 然后默认安装到计算机上 

安装成功的标志如下, 打开计算机命令行 减产node版本 以及 npm版本

![文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image002.jpg)

 

## 安装cnpm

因为创建一个Vue项目(并且在本地作为一个前端服务器运行起来 ), 需要很多第三方的包的支持(至少上百个包), 我们没有办法一个一个下载这些包(因为包需要 依赖另一个包, 并且依赖关系有版本要求), 所以我们前端就开发了一个工具叫npm, 这个工具的原理就是, 大家都把包存储到它的服务器上(有成千上万), 当我们需要的时候, 可以使用这个npm工具帮我们从它的服务器上下载正确的以及对应的包到我们本地,以供我们使用. 

但是, 这个npm服务器在国外,(另外安装node自带npm下载工具, node集成了npm), 所以下载很慢.

我们就需要一个国内镜像那就是cnpm(阿里提供), 所以我们可以安装cnpm的下载工具, 在我们国内镜像上下载我们需要的包

 

安装方式:如下

在计算机命令行执行如下命令

npm install -g cnpm --registry=https://registry.npm.taobao.org

![文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image004.jpg) 

安装成功的标志, 检查版本号

cnpm -v

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image006.jpg)

 

## 安装Vue-cli

Vue-cli对于我们创建一个项目来说, 就像创建java项目我们需要安装一个idea一样

 

安装方式:如下

在计算机命令行执行如下命令(总共需要两个命令)

cnpm install -g @vue/cli

cnpm install -g @vue/cli-init

![图片包含 文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image008.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image010.jpg)

![图片包含 文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image012.jpg)

判断安装成功的标志:检查版本号

vue -V

![文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image014.jpg)

 

## 安装webpack

安装命令如下:

cnpm install -g webpack

![形状  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image016.jpg)

![文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image018.jpg)

 

上述安装均属于基础配置

比如我们计算机如果安装了jvm 安装了idea 就不需要重复安装(除非idea或者jvm崩溃了)

##  

## 创建项目

创建一个Vue项目, 就类似与通过idea创建一个se项目一样, 只不过我们创建项目是通过命令行的命令

创建项目的命令: (注意项目名建议全小写英文)

vue init webpack 项目名

 

注意, 创建项目一定要选好命令行路径

比如, 我希望在桌面创建一个Vue项目, 就把命令行路径切换到桌面, 那么当项目创建完成, 这个项目就在桌面上

![文本  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image020.jpg)



比如, 我希望在D盘MyProject目录下创建一个Vue项目, 就把命令行路径切换到D盘MyProject目录, 那么当项目创建完成, 这个项目就在D盘MyProject目录下

![图形用户界面, 文本, 应用程序  描述已自动生成](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\JavaScript\Vue-learning.assets\clip_image022.jpg)

 

例如在桌面创建项目

(注意, 就像我们可以通过idea反复创建一个se项目, 我们也可以通过vue init webpack 项目名 命令反复在不同地方创建项目, 但是如果创建了之后不想要了, 记得吧创建出来的项目删除)



如果在这一步一直卡着, 可以选择切换一下手机热点

 

以上步骤, 默认回车, 让选择的y或者n 统一选n(输入n 回车)

 

注意最后一步, 上下键, 选最后一个, 回车, 项目目录构建已经完成

 

最后一步, 安装包

根据命令提示

第二步注意使用 cnpm install

// ---------------项目创建完成--------------------

 启动这个前端vue项目

命令 npm run dev

 这也就意味着, 我在我的计算机里启动了一个Vue项目(是一个前端服务器) 端口在8080

 关闭这个项目(停止) 命令ctrl + c, 已经关闭