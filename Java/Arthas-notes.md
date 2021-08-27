# Arthas notes

## 安装

- 安装JDK

- 下载Arthas

  `$ wget https://arthas.aliyun.com/arthas-boot.jar`

## 启动

使用`java -jar`命令启动Arthas。

`arthas-boot`是Arthas的启动程序，它启动后，会列出所有的Java进程，用户可以选择需要诊断的目标进程。

输入 进程号 ，再`Enter/`回车：

Attach成功之后，会打印Arthas LOGO。

```bash
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.                           
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'                          
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.                          
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |                         
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'  
```

## Dashboard

`$ dashboard` 命令可以查看当前系统的实时数据面板。

输入 q 或者 Ctrl+C 可以退出dashboard命令。

## Thread

`$ thread 1` 命令会打印线程ID为1的栈。

Arthas支持管道，可以用 `$ thread 1 | grep 'main('` 查找到main class。

## jad

反编译

## 临时修改变量

## trace

追溯方法内部调用路径，并输出方法路径上的每个节点上耗时

>  `trace` 能方便的帮助你定位和发现因 RT 高而导致的性能问题缺陷，但其每次只能跟踪一级方法的调用链路。



`trace`次数限制
如果方法调用的次数很多，那么可以用-n参数指定捕捉结果的次数。比如下面的例子里，捕捉到一次调用就退出命令。

如：`$ trace demo.MathGame run -n 1`



是否包含JDK的函数

`--skipJDKMethod <value>` 是否跳过JDK方法足迹，默认的`value`为`true`。

默认情况下，`trace`不会包含JDK里的函数调用，如果希望 `trace`  JDK里的函数，需要显式设置`--skipJDKMethod false`

如：`$ trace --skipJDKMethod false demo.MathGame run`



根据调用耗时过滤

`$ trace demo.MathGame run '#cost > 10'`

以上命令只会展示耗时大于10ms的调用路径，有助于在排查问题的时候，只关注异常情况。

> watch/stack/trace这个三个命令都支持`#cost`



`trace`个类或者多个函数



