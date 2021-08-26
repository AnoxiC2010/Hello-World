# Arthas notes



## trace

追溯方法内部调用路径，并输出方法路径上的每个节点上耗时

`trace` 能方便的帮助你定位和发现因 RT 高而导致的性能问题缺陷，但其每次只能跟踪一级方法的调用链路。



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



