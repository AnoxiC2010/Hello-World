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
- 
- cnpm install -g @vue/cli-init    (2.X桥接工具:脚手架)
  vue -V (v必须大写)

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
	
子组件向父组件传值
父接收:`<left @changeurl="changeurl"></left>`
子抛出: `this.$emit('changeurl', url)`



## VueBus( Vue事务总线)

vue bus可以实现不同组件间、不同页面间的通信，比如我在A页触发发点击事件，要B页面发生变化
一个中央事件总线bus，可以作为一个简单的组件传递数据，用于解决跨级和兄弟组件通信问题
创建Bus.js

- import Vue from 'vue'
- const Bus = new Vue()
- export default Bus

使用 emit   on   off 

```
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

```
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

- Main.js中

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