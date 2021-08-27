# Arthas notes

## 安装Arthas

- 安装JDK

- 下载Arthas

  `$ wget https://arthas.aliyun.com/arthas-boot.jar`

## 启动Arthas

使用`java -jar`命令启动Arthas。

`$ java -jar arthas-boot.jar`

`arthas-boot`是Arthas的启动程序，它启动后，会列出所有的Java进程，用户可以选择需要诊断的目标进程。

输入 进程号 ，再`Enter/`回车：

Attach成功之后，会打印Arthas LOGO。

输入 `$ help` 可以获取到更多的帮助信息。

```bash
  ,---.  ,------. ,--------.,--.  ,--.  ,---.   ,---.                           
 /  O  \ |  .--. ''--.  .--'|  '--'  | /  O  \ '   .-'                          
|  .-.  ||  '--'.'   |  |   |  .--.  ||  .-.  |`.  `-.                          
|  | |  ||  |\  \    |  |   |  |  |  ||  | |  |.-'    |                         
`--' `--'`--' '--'   `--'   `--'  `--'`--' `--'`-----'  
```



## 退出Arthas

用 `$ exit` 或者 `$ quit` 命令可以退出Arthas。

exit/quit命令只是退出当前session,不影响其他session，arthas server还在目标进程中运行。

想完全退出Arthas，断开所有session，可以执行 `$ stop` 命令。





## 查看JVM信息

### sysprop

`$ sysprop` 可以打印所有的System Properties信息。

也可以指定单个key： `$ sysprop java.version`

也可以通过grep来过滤： `$ sysprop | grep user`

```
[arthas@2390]$ sysprop | grep user
 user.timezone       Asia/Shanghai
 user.country        US
 user.home           /home/shell
 user.language       en
 user.name           shell
 user.dir            /home/shell
```

可以设置新的value： `$ sysprop testKey testValue`

### sysenv

`$ sysenv` 命令可以获取到环境变量。和`$ sysprop`命令类似。

### jvm

`$ jvm` 命令会打印出`JVM`的各种详细信息。

### Dashboard

`$ dashboard` 命令可以查看当前系统的实时数据面板。

输入 q 或者 Ctrl+C 可以退出dashboard命令。

```
[arthas@1287]$ dashboard 
ID  NAME                      GROUP        PRIORIT STATE    %CPU    DELTA_TI TIME    INTERRUP DAEMON  
-1  VM Periodic Task Thread   -            -1      -        0.08    0.004    0:0.598 false    true    
22  Timer-for-arthas-dashboar system       5       RUNNABLE 0.05    0.002    0:0.167 false    true    
-1  C1 CompilerThread1        -            -1      -        0.02    0.001    0:0.669 false    true    
1   main                      main         5       TIMED_WA 0.01    0.000    0:0.228 false    false   
-1  C2 CompilerThread0        -            -1      -        0.01    0.000    0:0.755 false    true    
20  arthas-NettyHttpTelnetBoo system       5       RUNNABLE 0.01    0.000    0:0.144 false    true    
-1  VM Thread                 -            -1      -        0.0     0.000    0:0.131 false    true    
16  arthas-shell-server       system       9       TIMED_WA 0.0     0.000    0:0.002 false    true    
17  arthas-session-manager    system       9       TIMED_WA 0.0     0.000    0:0.001 false    true    
2   Reference Handler         system       10      WAITING  0.0     0.000    0:0.002 false    true    
3   Finalizer                 system       8       WAITING  0.0     0.000    0:0.002 false    true    
4   Signal Dispatcher         system       9       RUNNABLE 0.0     0.000    0:0.000 false    true    
8   Attach Listener           system       9       RUNNABLE 0.0     0.000    0:0.028 false    true    
Memory                used   total  max     usage  GC                                                 
heap                  21M    41M    444M    4.80%  gc.copy.count             9                        
eden_space            4M     11M    122M    3.43%  gc.copy.time(ms)          71                       
survivor_space        111K   1408K  15680K  0.71%  gc.marksweepcompact.count 1                        
tenured_gen           17M    28M    306M    5.55%  gc.marksweepcompact.time( 29                       
nonheap               28M    29M    -1      96.95% ms)                                                
code_cache            5M     5M     240M    2.41%                                                     
metaspace             20M    21M    -1      97.11%                                                    
                      2M     2M     1024M   0.25%                                                     
direct                8K     8K     -                                                                 
mapped                0K     0K     -       0.00%                                                     
Runtime                                                                                               
os.name                                            Linux                                              
os.version                                         3.10.0-957.21.3.el7.x86_64                         
java.version                                       1.8.0_282                                          
java.home                                          /usr/lib/jvm/java-8-openjdk-amd64/jre              
systemload.average                                 0.06                                               
processors                                         1                                                  
timestamp/uptime                                   Sat Aug 28 00:07:20 CST 2021/616s                  
[arthas@1287]$ 
```



## Arthas使用提示

### help

Arthas里每一个命令都有详细的帮助信息。可以用`-h`来查看。帮助信息里有`EXAMPLES`和`WIKI`链接。

比如：

`$ sysprop -h`



### 自动补全

Arthas支持丰富的自动补全功能，在使用有疑惑时，可以输入`Tab`来获取更多信息。

比如输入 `$ sysprop java.` 之后，再输入`Tab`，会补全出对应的key：

```
$ sysprop java.
java.runtime.name             java.protocol.handler.pkgs    java.vm.version
java.vm.vendor                java.vendor.url               java.vm.name
...
```

### readline的快捷键支持

Arthas支持常见的命令行快捷键，比如`Ctrl + A`跳转行首，`Ctrl + E`跳转行尾。

更多的快捷键可以用 `keymap` 命令查看。

`$ keymap`



### 历史命令的补全

如果想再执行之前的命令，可以在输入一半时，按`Up/↑` 或者 `Ddown/↓`，来匹配到之前的命令。

比如之前执行过`$ sysprop java.version`，那么在输入`$ sysprop ja`之后，可以输入`Up/↑`，就会自动补全为`$ sysprop java.version`。

如果想查看所有的历史命令，也可以通过 `$ history` 命令查看到。



### pipeline

Arthas支持在pipeline之后，执行一些简单的命令，比如：

`$ sysprop | grep java`

`$ sysprop | wc -l`

## Thread

`$ thread 1` 命令会打印线程ID为1的栈。

Ctrl+C退出。

Arthas支持管道，可以用 `$ thread 1 | grep 'main('` 查找到main class。

```bash
$ thread 1 | grep 'main('
    at demo.MathGame.main(MathGame.java:17)
```



## Sc

可以通过 sc 命令来查找JVM里已加载的类：

`$ sc -d *MathGame`

```
[arthas@1287]$ sc -d *MathGame
 class-info        demo.MathGame                                                                      
 code-source       /home/shell/arthas-demo.jar                                                        
 name              demo.MathGame                                                                      
 isInterface       false                                                                              
 isAnnotation      false                                                                              
 isEnum            false                                                                              
 isAnonymousClass  false                                                                              
 isArray           false                                                                              
 isLocalClass      false                                                                              
 isMemberClass     false                                                                              
 isPrimitive       false                                                                              
 isSynthetic       false                                                                              
 simple-name       MathGame                                                                           
 modifier          public                                                                             
 annotation                                                                                           
 interfaces                                                                                           
 super-class       +-java.lang.Object                                                                 
 class-loader      +-sun.misc.Launcher$AppClassLoader@1b6d3586                                        
                     +-sun.misc.Launcher$ExtClassLoader@7d8a54ef                                      
 classLoaderHash   1b6d3586                                                                           

Affect(row-cnt:1) cost in 61 ms.
[arthas@1287]$ 
```



## jad

通过 jad 命令来反编译代码：

`$ jad demo.MathGame`



## Watch

通过watch命令可以查看函数的参数/返回值/异常信息。

`$ watch demo.MathGame primeFactors returnObj`

输入 q 或者 Ctrl+C 退出watch命令

```
[arthas@1287]$ watch demo.MathGame primeFactors returnObj
Press Q or Ctrl+C to abort.
Affect(class count: 1 , method count: 1) cost in 200 ms, listenerId: 1
method=demo.MathGame.primeFactors location=AtExit
ts=2021-08-28 00:21:27; [cost=1.479334ms] result=@ArrayList[
    @Integer[2],
    @Integer[2],
    @Integer[34313],
]
...
```



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



