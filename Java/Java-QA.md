# HashMap

## 为什么HashMap线程不安全

[为什么HashMap线程不安全 - 简书 (jianshu.com)](https://www.jianshu.com/p/e2f75c8cce01)

put方法源码





# 保证Redis和数据库数据的一致性





# 多线程



## 线程池的参数

[(2条消息) Java线程池七个参数详解_IT小跟班-CSDN博客_线程池的七个参数](https://blog.csdn.net/ye17186/article/details/89467919)

## ReentrantLock

[ReentrantLock(重入锁)功能详解和应用演示 - takumiCX - 博客园 (cnblogs.com)](https://www.cnblogs.com/takumicx/p/9338983.html)

ReentrantLock是可重入的独占锁。比起synchronized功能更加丰富，支持公平锁实现，支持中断响应以及限时等待等等。可以配合一个或多个Condition条件方便的实现等待通知机制。



# 设计模式

## 单例

双重校验锁实现单例模式

```java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    private Singleton() {
    }
    public static Singleton getUniqueInstance() {
        //先判断对象是否已经实例过，没有实例化过才进⼊加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```



# NIO

Java面试常考的 BIO，NIO，AIO 总结

https://blog.csdn.net/m0_38109046/article/details/89449305



# Spring

循环依赖

https://www.zhihu.com/question/438247718/answer/1730527725

