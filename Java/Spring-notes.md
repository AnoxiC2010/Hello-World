# Spring notes

# 设计模式Design Pattern

经验 → 有经验的开关人员提出的分类编目、不断实践总结下来的这样的一些经验

 

设计代码的一个思路 → 套路

 

王婆卖瓜 自卖自夸

 

可读性比较强 → 工厂Factory → XXXFactory

拓展性和灵活性 

强壮 

 

# 软件开发的原则SOLID

S：单一职责原则 

O：开闭原则 → 开放、封闭 → 新增代码开放、修改代码封闭 → 调整业务的时候建议新增代码来做

L ：里氏替代原则 → 凡是子类出现的地方都可以用父类来接收。**子类尽量不要去重写父类的方法**，如果要重写，父类中定义抽象方法

Father object = new Son();

 

I : 接口隔离原则 → 不同的功能放到不同的接口中，**避免创建大接口。大接口成本高**

D：依赖倒置原则。具体依赖于抽象。 → 设计代码时先去设计接口

 

# 具体的设计模式

## 单例singleton

非常重要的，Spring阶段最常用的设计模式

应用程序中，要取出某个类型的实例 → 始终是同一个实例 → 有Servlet

 

为什么要使用单例 → 构建对象之间的关系 → 简化代码开发、提高了实例的可重用性

写单例

**1、** **构造方法私有**

**2、** **提供静态的方法来提供实例**

**3、** **提供一个自己本身类型的成员变量**

### 线程不安全的单例

```java
public class MySingleton {

    public void method1(){
        System.out.println("FGNB");
    }

    //3、自行提供一个全局变量（私有、静态）
    private static MySingleton mySingleton;
    //1、构造方法私有
    private MySingleton() {
    }


    //2、提供一个静态方法来获得实例
    public static MySingleton getInstance(){

        /**
         * 已经实例化了，就直接返回
         * 如果还没有实例化，就完成实例化
         */
        if (mySingleton == null) {
            mySingleton = new MySingleton();
        }
        return mySingleton;
    }
}
```



### 线程安全的单例

```JAVA
public class MySingleton2 {

    public void method1(){
        System.out.println("FGNB");
    }

    //3、自行提供一个全局变量（私有、静态）
    private static MySingleton2 mySingleton;
    //1、构造方法私有
    private MySingleton2() {
    }

    //2、提供一个静态方法来获得实例
    //同步代码 效率低
    public synchronized static MySingleton2 getInstance(){

        /**
         * 已经实例化了，就直接返回
         * 如果还没有实例化，就完成实例化
         */
        if (mySingleton == null) {
            mySingleton = new MySingleton2();
        }
        return mySingleton;
    }
}
```



### 懒加载和立即加载

获得实例 → getInstance方法

实例的实例化 → 在调用获得实例方法之前已经提前完成了实例化还是在使用的 时候才去完成实例化

 

饱汉模式

饿汉模式

### 线程安全的立即加载

```java
/**
 * 线程安全的立即单例 静态代码块实例化
 */
public class MySingleton3 {

    public void method1(){
        System.out.println("松哥牛皮");
    }

    //3、自行提供一个全局变量（私有、静态）
    private static MySingleton3 mySingleton;
    static {
        mySingleton = new MySingleton3();
    }
    //1、构造方法私有
    private MySingleton3() {
    }

    //2、提供一个静态方法来获得实例
    public static MySingleton3 getInstance(){

        return mySingleton;
    }
}
/**
 * 线程安全的立即单例 直接实例化
 */
public class MySingleton4 {

    public void method1(){
        System.out.println("松哥牛皮");
    }

    //3、自行提供一个全局变量（私有、静态）
    private static MySingleton4 mySingleton = new MySingleton4();


    //1、构造方法私有
    private MySingleton4() {
    }


    //2、提供一个静态方法来获得实例
    public static MySingleton4 getInstance(){

        return mySingleton;
    }
}
```



### 静态内部类 → 线程安全的懒加载

#### 静态内部类

```java
public class Outer {

    //没有调用静态内部类的方法
    public static void noInvokeInnerMethod(){
        System.out.println("没有调用静态内部类的方法");
    }
    //调用静态内部类的方法
    public static void invokeInnerMethod(){
        System.out.println("调用静态内部类的方法");
        Inner.innerMethod();
    }

    static class Inner{
        static {
            System.out.println("静态内部类的静态代码块");
        }
        public static void innerMethod(){
            System.out.println("静态内部类的方法");
        }
    }
}
```



##### 单元测试

```java
public class OuterTest {

    @Test
    public void mytest1(){
        //当我们没有使用到静态内部类的时候，没有完成静态内部类的加载
        Outer.noInvokeInnerMethod();
        //没有调用静态内部类的方法
    }
    @Test
    public void mytest2(){
        Outer.invokeInnerMethod();
        //调用静态内部类的方法
        //静态内部类的静态代码块 👉 当我们执行到静态内部类的方法的时候，完成静态内部类的类加载 👉 执行静态代码块
        //静态内部类的方法
    }

    @Test
    public void mytest3(){
        //静态内部类的静态代码块 在第一次调用静态内部类的方法的时候（类加载），只执行一次
        Outer.invokeInnerMethod();
        Outer.invokeInnerMethod();
        Outer.invokeInnerMethod();
        //调用静态内部类的方法
        //静态内部类的静态代码块
        //静态内部类的方法
        //调用静态内部类的方法
        //静态内部类的方法
        //调用静态内部类的方法
        //静态内部类的方法
    }
}
```



#### 线程安全的懒加载

```java
public class MySingleton5 {
    //构造方法私有
    private MySingleton5(){}

    //提供静态方法
    public static MySingleton5 getInstance() {
        return Inner.getInnerInstance();
    }
    //提供成员变量
    private static MySingleton5 mySingleton5;

    static class Inner{
        //将实例化的过程，放在了静态内部类的静态代码块
        static {
            mySingleton5 = new MySingleton5();
        }
        //提供一个静态方法 返回实例
        public static MySingleton5 getInnerInstance(){
            return mySingleton5;
        }
    }
}
```

```java
public class MySingleton6 {
    //构造方法私有
    private MySingleton6(){}

    //提供静态方法
    public static MySingleton6 getInstance() {
        return Inner.getInnerInstance();
    }

    static class Inner{
        //提供成员变量
        private static MySingleton6 mySingleton6 = new MySingleton6();
        //将实例化的过程，放在了静态内部类的静态代码块
        /*static {
            mySingleton6 = new MySingleton6();
        }*/
        //提供一个静态方法 返回实例
        public static MySingleton6 getInnerInstance(){
            return mySingleton6;
        }
    }
}
```





## 工厂factory

工厂是做什么的 → 生产东西

**生产实例的 → 工厂中都提供一个生产实例的方法**

为什么要通过工厂获得实例 → 通过方法 → 可以将获得实例的过程隐藏起来 → 将实例的实现细节隐藏在方法中

名称 → XXXFactory → WeiDragonFactory : 采用工厂设计模式、生产的是卫龙

### 简单工厂

通过传入不同的参数，响应不同的结果（不同类型实例）

AnimalFactory 小动物工厂

```java
public class AnimalFactory {

    public Animal getAnimal(String name) {
        if ("rabbit".equals(name)) {
            return new Rabbit();
        } else if ("pig".equals(name)) {
            return new Pig();
        }
        //如果要新增内容，我们直接在方法里新增了这部分内容 → 开闭原则
        else if ("cow".equals(name)) {
            return new Cow();
        }
        return null;
    }
}
```



### 工厂方法

提供一个方法的标准 → 工厂接口

由工厂的实现类来获得具体的实例

MoneyFactory → public Money getMoney();

 

RmbMoney

BitCoin

```java
public interface Money {}
public interface MoneyFactory {
    public Money getMoney();
}

public class RmbMoney implements Money {}
public class RmbMoneyFactory implements MoneyFactory{
    @Override
    public Money getMoney() {
        return new RmbMoney();
    }
}
```



## 代理proxy 👉 增强

什么叫代理 ？

番茄 → fq

中介

 

帮别人做事情，做的是原先要做的事情

```
客户端<==>代理服务器<=替客户发送请求，替客户接收响应=>Tik Tok
```



刘师傅 买早餐（热干面和豆浆）

新垣结衣 帮带早餐（热干面和豆浆 + 烤肠） → 刘师傅的代理

代理可以帮我们做额外的事情

 

静态代理和动态代理都有代理类，代理类中提供了方法 → 提供了和被代理类（委托类）相同的方法

 

静态代理这个代理类需要我们自己写

动态代理这个代理类是自动生成的

### 静态代理

```java
//委托类
public class Liushifu {

    public void buyBreakfast(){
        System.out.println("一碗热干面一杯蛋酒");
    }
}
//代理类
public class Xyjy {

    Liushifu liushifu = new Liushifu();

    public void buyBreakfast(){
        //代理类去做这件事情的时候 → 要执行到委托类的方法
        //执行委托类方法并做了增强
        liushifu.buyBreakfast();
        System.out.println("+ 烤肠");
    }
}
//代理类
public class Xyjy2 extends Liushifu{

    public void buyBreakfast(){
        //代理类去做这件事情的时候 → 要执行到委托类的方法
        //继承调用父类方法 执行委托类的方法
        super.buyBreakfast();
        System.out.println("+ 烤肠");
    }
}
```



### 动态代理

需要代理类，但是不是我们手写的，动态生成的

#### JDK动态代理

委托类必须要有接口的实现 → 代理类实现了委托类所实现的接口

如何接收代理对象？  → **使用接口来接收**

可否使用委托类来接收? NO

UserService1和UserService2都实现了UserService接口

我能用UserService1来接收UserService2的实例吗？不可以

我可以用UserService 接口来接收UserService的实例啊

![img](file:///C:/Users/ANOXIC~1/AppData/Local/Temp/msohtmlclip1/01/clip_image012.jpg)

```java
public class Execution {

    //为什么写main方法，没有写单元测试方法
    //后续要做一件事情 → 生成代理类的字节码文件
    public static void main(String[] args) {

        //默认的动态代理的字节码文件不生成 👉 可以让他保存下来
        //生成的文件保存到了哪里 👉 应用程序的working directory 👉 默认的工作路径是project路径
        System.setProperty("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");

        Liushifu liushifu = new Liushifu();
        //获得代理类的实例（对象）
        //1、classloader
        //2、interfaces 获得委托类所实现的接口数组
        //3、InvocationHandler → 接口 → 实现类要做的事情 → 增强 → 委托类方法的实现 + 额外的事情
        //我们需要自己来提供这个代理类的InvocationHandler → invoke方法
        //代理类对象 去执行委托类相同的这个方法 → 实际上就是去执行InvocationHandler的invoke方法
        // → invoke方法中 → 不仅要实现委托类的方法的调用 + 额外的增强
        //可以额外去写InvocationHandler的实现类，也可以采用匿名内部类的方式来做 → 推荐使用匿名内部类的方式
        // → 可以更方便的调用委托类的方法
        //要使用接口来接收Jdk代理对象
        Breakfast liushifuProxy = (Breakfast) Proxy.newProxyInstance(liushifu.getClass().getClassLoader(),
                liushifu.getClass().getInterfaces(),
                //new CustomInvocationHandler()
                new InvocationHandler() { //类的定义
                    //关注的就是invoke方法的写法 → 回调方法
                    //invoke方法是否是获得这个Proxy对象的时候执行的？？？ 代理对象去执行方法的时候 → 而不是定义匿名内部类的时候执行的
                    /**
                     * @param proxy → 代理对象 → 就是liushifuProxy
                     * @param method → 代理对象正在执行的委托类的方法
                     * @param args → 代理对象去执行委托类方法的时候携带的参数
                     * @return Object → 委托类方法的执行结果
                     * 增强范围 👉 委托类中的全部方法 👉 按照统一的方式增强
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

                        //委托类代码的执行
                        //liushifu.buyBreakfast(); //方法不要写死了
                        //liushifu\method\args 反射
                        Object invoke = method.invoke(liushifu, args);//执行了委托类的方法
                        //增强
                        System.out.println("+烤肠" + method.getName()); //增强全部方法
                        return invoke;
                    }
                }
        );
        //liushifuProxy.buyBreakfast();//使用代理对象执行方法
        liushifuProxy.buyBreakfast2("油条");
    }
}
```



生成代理类的字节码文件class 👉 反编译

代理类中的方法（委托类中的这些方法） 👉 invocationHandler的invoke方法

//Edit Configuration→Application→Working directory改为MODULE目录会把动态代理字节码文件存在当前module下

```java
// com/sun/proxy/$Proxy0.class

// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package com.sun.proxy;

import com.cskaoyan.proxy.Breakfast;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.lang.reflect.UndeclaredThrowableException;

public final class $Proxy0 extends Proxy implements Breakfast {
    private static Method m1;
    private static Method m4;
    private static Method m3;
    private static Method m2;
    private static Method m0;

    public $Proxy0(InvocationHandler var1) throws  {
        super(var1);
    }

    public final boolean equals(Object var1) throws  {
        try {
            return (Boolean)super.h.invoke(this, m1, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }
	//有参数的方法
    public final void buyBreakfast2(String var1) throws  {
        try {
            super.h.invoke(this, m4, new Object[]{var1});
        } catch (RuntimeException | Error var3) {
            throw var3;
        } catch (Throwable var4) {
            throw new UndeclaredThrowableException(var4);
        }
    }
	//无参数的方法
    public final void buyBreakfast() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String toString() throws  {
        try {
            return (String)super.h.invoke(this, m2, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final int hashCode() throws  {
        try {
            return (Integer)super.h.invoke(this, m0, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    static {
        try {
            m1 = Class.forName("java.lang.Object").getMethod("equals", Class.forName("java.lang.Object"));
            m4 = Class.forName("com.cskaoyan.proxy.Breakfast").getMethod("buyBreakfast2", Class.forName("java.lang.String"));
            m3 = Class.forName("com.cskaoyan.proxy.Breakfast").getMethod("buyBreakfast");
            m2 = Class.forName("java.lang.Object").getMethod("toString");
            m0 = Class.forName("java.lang.Object").getMethod("hashCode");
        } catch (NoSuchMethodException var2) {
            throw new NoSuchMethodError(var2.getMessage());
        } catch (ClassNotFoundException var3) {
            throw new NoClassDefFoundError(var3.getMessage());
        }
    }
}

```



 

####  Cglib动态代理

基于继承去实现的 👉 代理类继承了委托类

可以使用委托类来接受代理类

![img](file:///C:/Users/ANOXIC~1/AppData/Local/Temp/msohtmlclip1/01/clip_image022.jpg)

##### 引入依赖

```xml
<!--引入cglib的依赖-->
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.2.12</version>
</dependency>
</dependencies>
```

```java
public class Execution {
    public static void main(String[] args) {

        //保存一下生成的委托类的字节码文件 👉 字节码文件保存到哪里
        //生成的字节码文件的目录 👉 委托类的包目录一致的
        System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY,
                "D:\\WorkSpace\\j30_workspace\\codes\\day01-design-pattern\\demo6-cglib-dynamic-proxy");
        Liushifu liushifu = new Liushifu();
        //生成一个cglib代理对象
        //可以使用委托类来接收代理对象
        Liushifu proxy = (Liushifu) Enhancer.create(Liushifu.class, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("想不想赚W");
                Object invoke = method.invoke(liushifu, args);
                System.out.println("想");
                return invoke;
            }
        });
        //proxy.buyBreakfast();
        proxy.buyBreakfast2("豆皮");

    }
}

```

代理类字节码文件

```java
//com/cskaoyan/proxy/Liushifu$$EnhancerByCGLIB$$ca2612d1.class

// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//
import java.lang.reflect.Method;
import net.sf.cglib.proxy.Callback;
import net.sf.cglib.proxy.Factory;
import net.sf.cglib.proxy.InvocationHandler;
import net.sf.cglib.proxy.UndeclaredThrowableException;

public class Liushifu$$EnhancerByCGLIB$$ca2612d1 extends Liushifu implements Factory {
    private boolean CGLIB$BOUND;
    public static Object CGLIB$FACTORY_DATA;
    private static final ThreadLocal CGLIB$THREAD_CALLBACKS;
    private static final Callback[] CGLIB$STATIC_CALLBACKS;
    private InvocationHandler CGLIB$CALLBACK_0;
    private static Object CGLIB$CALLBACK_FILTER;
    private static final Method CGLIB$buyBreakfast2$0;
    private static final Method CGLIB$buyBreakfast$1;
    private static final Method CGLIB$equals$2;
    private static final Method CGLIB$toString$3;
    private static final Method CGLIB$hashCode$4;
    private static final Method CGLIB$clone$5;

    static void CGLIB$STATICHOOK1() {
        CGLIB$THREAD_CALLBACKS = new ThreadLocal();
        CGLIB$buyBreakfast2$0 = Class.forName("com.cskaoyan.proxy.Liushifu").getDeclaredMethod("buyBreakfast2", Class.forName("java.lang.String"));
        CGLIB$buyBreakfast$1 = Class.forName("com.cskaoyan.proxy.Liushifu").getDeclaredMethod("buyBreakfast");
        CGLIB$equals$2 = Class.forName("java.lang.Object").getDeclaredMethod("equals", Class.forName("java.lang.Object"));
        CGLIB$toString$3 = Class.forName("java.lang.Object").getDeclaredMethod("toString");
        CGLIB$hashCode$4 = Class.forName("java.lang.Object").getDeclaredMethod("hashCode");
        CGLIB$clone$5 = Class.forName("java.lang.Object").getDeclaredMethod("clone");
    }

    public final void buyBreakfast2(String var1) {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            var10000.invoke(this, CGLIB$buyBreakfast2$0, new Object[]{var1});
        } catch (Error | RuntimeException var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final void buyBreakfast() {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            var10000.invoke(this, CGLIB$buyBreakfast$1, new Object[0]);
        } catch (Error | RuntimeException var1) {
            throw var1;
        } catch (Throwable var2) {
            throw new UndeclaredThrowableException(var2);
        }
    }

    public final boolean equals(Object var1) {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            return (Boolean)var10000.invoke(this, CGLIB$equals$2, new Object[]{var1});
        } catch (Error | RuntimeException var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }

    public final String toString() {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            return (String)var10000.invoke(this, CGLIB$toString$3, new Object[0]);
        } catch (Error | RuntimeException var1) {
            throw var1;
        } catch (Throwable var2) {
            throw new UndeclaredThrowableException(var2);
        }
    }

    public final int hashCode() {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            return ((Number)var10000.invoke(this, CGLIB$hashCode$4, new Object[0])).intValue();
        } catch (Error | RuntimeException var1) {
            throw var1;
        } catch (Throwable var2) {
            throw new UndeclaredThrowableException(var2);
        }
    }

    protected final Object clone() throws CloneNotSupportedException {
        try {
            InvocationHandler var10000 = this.CGLIB$CALLBACK_0;
            if (var10000 == null) {
                CGLIB$BIND_CALLBACKS(this);
                var10000 = this.CGLIB$CALLBACK_0;
            }

            return var10000.invoke(this, CGLIB$clone$5, new Object[0]);
        } catch (Error | CloneNotSupportedException | RuntimeException var1) {
            throw var1;
        } catch (Throwable var2) {
            throw new UndeclaredThrowableException(var2);
        }
    }

    public Liushifu$$EnhancerByCGLIB$$ca2612d1() {
        CGLIB$BIND_CALLBACKS(this);
    }

    public static void CGLIB$SET_THREAD_CALLBACKS(Callback[] var0) {
        CGLIB$THREAD_CALLBACKS.set(var0);
    }

    public static void CGLIB$SET_STATIC_CALLBACKS(Callback[] var0) {
        CGLIB$STATIC_CALLBACKS = var0;
    }

    private static final void CGLIB$BIND_CALLBACKS(Object var0) {
        Liushifu$$EnhancerByCGLIB$$ca2612d1 var1 = (Liushifu$$EnhancerByCGLIB$$ca2612d1)var0;
        if (!var1.CGLIB$BOUND) {
            var1.CGLIB$BOUND = true;
            Object var10000 = CGLIB$THREAD_CALLBACKS.get();
            if (var10000 == null) {
                var10000 = CGLIB$STATIC_CALLBACKS;
                if (var10000 == null) {
                    return;
                }
            }

            var1.CGLIB$CALLBACK_0 = (InvocationHandler)((Callback[])var10000)[0];
        }

    }

    public Object newInstance(Callback[] var1) {
        CGLIB$SET_THREAD_CALLBACKS(var1);
        Liushifu$$EnhancerByCGLIB$$ca2612d1 var10000 = new Liushifu$$EnhancerByCGLIB$$ca2612d1();
        CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
        return var10000;
    }

    public Object newInstance(Callback var1) {
        CGLIB$SET_THREAD_CALLBACKS(new Callback[]{var1});
        Liushifu$$EnhancerByCGLIB$$ca2612d1 var10000 = new Liushifu$$EnhancerByCGLIB$$ca2612d1();
        CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
        return var10000;
    }

    public Object newInstance(Class[] var1, Object[] var2, Callback[] var3) {
        CGLIB$SET_THREAD_CALLBACKS(var3);
        Liushifu$$EnhancerByCGLIB$$ca2612d1 var10000 = new Liushifu$$EnhancerByCGLIB$$ca2612d1;
        switch(var1.length) {
        case 0:
            var10000.<init>();
            CGLIB$SET_THREAD_CALLBACKS((Callback[])null);
            return var10000;
        default:
            throw new IllegalArgumentException("Constructor not found");
        }
    }

    public Callback getCallback(int var1) {
        CGLIB$BIND_CALLBACKS(this);
        InvocationHandler var10000;
        switch(var1) {
        case 0:
            var10000 = this.CGLIB$CALLBACK_0;
            break;
        default:
            var10000 = null;
        }

        return var10000;
    }

    public void setCallback(int var1, Callback var2) {
        switch(var1) {
        case 0:
            this.CGLIB$CALLBACK_0 = (InvocationHandler)var2;
        default:
        }
    }

    public Callback[] getCallbacks() {
        CGLIB$BIND_CALLBACKS(this);
        return new Callback[]{this.CGLIB$CALLBACK_0};
    }

    public void setCallbacks(Callback[] var1) {
        this.CGLIB$CALLBACK_0 = (InvocationHandler)var1[0];
    }

    static {
        CGLIB$STATICHOOK1();
    }
}
```



#### 小结

```
获得代理对象

- JDK动态代理 Proxy.newProxyInstance → InvocationHandler
- Cglib动态代理 Enhancer.create() → InvocationHandler(名字一样但是不同)

invoke → 1.委托类代码method.invoke 2.额外增强代码
```

![image-20210525201149872](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\image-20210525201149872.png)

注意在debug模式下一步步查看代理类调用方法的过程会发现在实际执行想要的结果之前InvocationHandler中的增强代码已经执行了两遍，这是由于IDEA的debug模式机制造成的，debug模式下IDEA用于显示变量会调用tostring方法，代理类会增强所有方法，所以在代码执行到invoke方法时IDEA为了显示liushifuProxy和内部类中的proxy先调用了两次tostring方法，便有了这种现象。

```java
public class Execution {

    //为什么写main方法，没有写单元测试方法
    //后续要做一件事情 → 生成代理类的字节码文件
    public static void main(String[] args) {

        //默认的动态代理的字节码文件不生成 👉 可以让他保存下来
        //生成的文件保存到了哪里 👉 应用程序的working directory 👉 默认的工作路径是project路径
//        System.setProperty("sun.misc.ProxyGenerator.saveGeneratedFiles", "true");

        Liushifu liushifu = new Liushifu();
        //获得代理类的实例（对象）
        //1、classloader
        //2、interfaces 获得委托类所实现的接口数组
        //3、InvocationHandler → 接口 → 实现类要做的事情 → 增强 → 委托类方法的实现 + 额外的事情
        //我们需要自己来提供这个代理类的InvocationHandler → invoke方法
        //代理类对象 去执行委托类相同的这个方法 → 实际上就是去执行InvocationHandler的invoke方法
        // → invoke方法中 → 不仅要实现委托类的方法的调用 + 额外的增强
        //可以额外去写InvocationHandler的实现类，也可以采用匿名内部类的方式来做 → 推荐使用匿名内部类的方式
        // → 可以更方便的调用委托类的方法
        //要使用接口来接收Jdk代理对象
        Breakfast liushifuProxy = (Breakfast) Proxy.newProxyInstance(liushifu.getClass().getClassLoader(),
                liushifu.getClass().getInterfaces(),
                //new CustomInvocationHandler()
                new InvocationHandler() { //类的定义
                    //关注的就是invoke方法的写法 → 回调方法
                    //invoke方法是否是获得这个Proxy对象的时候执行的？？？ 代理对象去执行方法的时候 → 而不是定义匿名内部类的时候执行的
                    /**
                     * @param proxy → 代理对象 → 就是liushifuProxy
                     * @param method → 代理对象正在执行的委托类的方法
                     * @param args → 代理对象去执行委托类方法的时候携带的参数
                     * @return Object → 委托类方法的执行结果
                     * 增强范围 👉 委托类中的全部方法 👉 按照统一的方式增强
                     */
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

                        //委托类代码的执行
                        //liushifu.buyBreakfast(); //方法不要写死了
                        //liushifu\method\args 反射
                        Object invoke = method.invoke(liushifu, args);//执行了委托类的方法
                        //增强
                        System.out.println("+烤肠" + method.getName()); //增强全部方法
                        return invoke;
                    }
                }
        );
        liushifuProxy.buyBreakfast();//使用代理对象执行方法
//        liushifuProxy.buyBreakfast2("油条");
    }
}

//debug模式一步步查看会发现+烤肠提前执行了两次（伴随着toSting方法的调用执行的）
```



## 建造者builder

生产东西 👉 生产实例

侧重点不同 👉 工厂侧重于隐藏实现细节，建造者侧重于设置参数的过程

 

StringBuilder\StringBuffer

 

两类方法：设置参数、获得结果

 

XXXBuilder 👉 XXX

 

造人HumanBuilder 👉 Human

```java
@Data
public class Human {
    Head head = new Head();
    Arm arm = new Arm();
    Leg leg = new Leg();
    Body body = new Body();
}

public class HumanBuilder2 {

    Human human = new Human();

    /*static Human human;
    static {
        human = new Human();
    return this;
    }*/
    //参数设置的方法
    public HumanBuilder2 setHeadIq(Integer value) {
        //human = new Human(); //实例化要放在调用set方法之前
        Head head = human.getHead();
        head.setIq(value);
        return this;
    }

    public HumanBuilder2 setHeadEq(Integer value) {
        human.getHead().setEq(value);
        return this;
    }

    public HumanBuilder2 setHeadHair(String value) {
        human.getHead().setHair(value);
        return this;
    }

    public HumanBuilder2 setArmLength(Integer value) {
        human.getArm().setLength(value);
        return this;
    }

    public HumanBuilder2 setLegLength(Integer value) {
        human.getLeg().setLength(value);
        return this;
    }

    public HumanBuilder2 setLegStrong(boolean value) {
        human.getLeg().setStrong(value);
        return this;
    }

    public HumanBuilder2 setBodyMuscle(String value) {
        human.getBody().setMuscle(value);
        return this;
    }

    public HumanBuilder2 setBodyHeart(String value) {
        human.getBody().setHeart(value);
        return this;
    }

    public Human build() {
        return human;
    }

}
/**
     * 如果想要一直调用，就要求set方法的返回值是builder对象本身
     */
@Test
public void mytest3(){
    HumanBuilder2 humanBuilder2 = new HumanBuilder2();
    humanBuilder2.setHeadIq(120)
        .setHeadEq(180)
        .setHeadHair("浓密")
        .setLegLength(150)
        .setLegStrong(true)
        .setArmLength(150)
        .setBodyMuscle("十块腹肌")
        .setBodyHeart("专心");
    Human human = humanBuilder2.build();
    System.out.println(human);
}
```





# SpringFramework

Spring框架 👉 Spring

 

春天

Rod Johnson 👉 大佬



spring.io 👉 the Source of Modern Java

Java程序员的标配 👉 Spring工程师 

# IOC和DI（核心中的核心）

## IOC

Inverse of control控制反转

 

经常有方法的调用 👉 对象（实例）

 

应用程序来通过构造方法new出来实例 

**控制** **👉 实例的生成权**

**反转** **👉 实例的生成反转为由Spring来做**

原先由应用程序来获得实例 👉 现在由Spring来做

 

👉由Spring来做实例的统一生成、实例的管理

容器：装的是实例 👉 Spring容器或IOC容器 👉 生成并管理实例的地方（实例在容器中通常以单例的形式存在）

 

## DI

Dependency Injection依赖注入

应用程序和Spring容器：经过了控制反转 👉 谁贫穷谁富有

依赖：谁依赖谁？为什么？ 👉 应用程序依赖于Spring容器，Spring容器中包含了应用程序所必须的资源。

注入：谁注入谁，注入了什么？ 👉 Spring容器注入给应用程序，注入的就是应用程序所必须的实例和外部资源

 

👉 平台型框架 👉 淘宝网C2C 平台 👉 商家可以入住到淘宝平台 👉 辅助性支撑性的功能

 

# 入门案例1

将一个实例交给Spring容器来管理，从Spring容器中取出该实例，并且执行该实例的方法

## 引入依赖

**spring-context**

spring-aop

spring-core

spring-expression

spring-beans

jcl 日志包

```xml
<!--在pom.xml中只写context,maven会关联其他需要的依赖-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
```



## 构建Spring容器

获得容器的对象ApplicationContext（接口）

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext();
//加载Classpath目录下的Spring的xml配置文件，获得Spring容器的实例
```



## 提供Spring的配置文件xml

xml格式的 👉 格式要求、语法限制 👉 schema约束 👉 你可以使用什么标签、标签中由什么属性、子标签、标签之间的顺序

 

需要使用Spring配置文件的schema约束

1、 已有的配置文件

2、 官网 

**3、** **创建文件模板**

application.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- bean definitions here -->
    <!--实例的管理
            组件：Spring容器中所管理的实例
            注册：提供信息完成组件的管理
            注册组件：将实例交给Spring容器生成并管理
    -->
    <!--
        id属性：组件在容器中的唯一标识 👉 可以省略的，会给到一个和包名类名相关的一个id
        name属性：通常省略不写，以id作为默认name
        class属性：必需的，全类名
    -->
    <bean id="userService" class="com.cskaoyan.service.UserServiceImpl"/>

</beans>
```



## 单元测试

按照类型从容器中取组件的方式更常用

```java
@Test
public void mytest1() {
    //初始化容器，获得容器对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");

    //建议以接口的形式来进行接收 👉 有可能对组件做增强 👉 动态代理 👉 接口实现 👉 Jdk动态代理 👉 接口接收
    //从容器中取出组件 👉 applicationContext.getBean
    //getBean(String) 👉 通过组件id从容器中取出对应的值
    UserService userService1 = (UserService) applicationContext.getBean("userService");

    //getBean(Class) 👉 通过组件的类型取出组件 👉 容器中该类型的组件只有一个
    UserService userService2 = applicationContext.getBean(UserService.class);

    //getBean(String,Class)
    UserService userService3 = applicationContext.getBean("userService", UserService.class);

    userService1.sayHello("嘎子");
    userService2.sayHello("松哥");
    userService3.sayHello("景天");
}
```



# 入门案例2

向容器中注册多个组件，并且呢，多个组件之间存在关联关系。

UserService

UserDao

建立多个组件之间的依赖关系

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image012.jpg)

## 配置文件

```xml
<!-- bean definitions here -->
<!--注册UserService组件-->
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl">
    <!--维护容器中UserDao组件和UserService组件之间的关系-->
    <!--property标签：维护组件之间的依赖关系
                name属性：set方法名(通常set方法都是根据成员变量名生成的 👉 你可以认为写的是成员变量名)
                value属性：值的类型为值类型（基本类型以及包装类，字符串，class）
                ref属性：reference引用的组件id 👉 当你看到一个ref的时候，就要想到id
        -->
    <property name="userDaozzz" ref="userDao"/>
    <!--property标签：完成父标签组件注册过程中使用set方法完成属性值配置-->
</bean>
<!--注册UserDao组件-->
<bean id="userDao" class="com.cskaoyan.dao.UserDaoImpl"/>
```



## 单元测试

```java
@Test
public void mytest1() {
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");

    UserService userService = applicationContext.getBean(UserService.class);
    userService.sayHello("嘎子");

    UserDao userDao = applicationContext.getBean(UserDao.class);//debug来看userService里的userDao和直接取出的是否是同一个
}
```



# 核心接口

BeanFactory 👉 Bean工厂 👉 生产全部Bean的地方

ApplicationContext 👉 extends BeanFactory

都是我们的容器接口 👉 他们提供的方法就是容器提供给我们的功能

 

使用的是实现类

ClasspathXmlApplicationContext

FileSystemXmlApplicationContext

AnnotationConfigApplicationContext

MVC阶段

XmlWebApplicationContext

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image018.jpg)

# Scope作用域

Spring容器中的组件的作用域

**singleton 单例** 👉 每一次从容器中取出组件都是同一个

prototype 原型 👉 每一次从容器中取出组件都是新的组件

```xml
<!--
scope属性：singleton、prototype
-->
<bean class="com.cskaoyan.bean.SingletonBean" scope="singleton"/>
<bean class="com.cskaoyan.bean.PrototypeBean" scope="prototype"/>
<bean class="com.cskaoyan.bean.DefaultBean"/><!--默认的scope 👉 Singleton-->
```

```java
@Test//debug观察地址
public void mytest1() {
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
	//地址一样
    SingletonBean singletonBean1 = applicationContext.getBean(SingletonBean.class);
    SingletonBean singletonBean2 = applicationContext.getBean(SingletonBean.class);
    SingletonBean singletonBean3 = applicationContext.getBean(SingletonBean.class);
	//地址不一样
    PrototypeBean prototypeBean1 = applicationContext.getBean(PrototypeBean.class);
    PrototypeBean prototypeBean2 = applicationContext.getBean(PrototypeBean.class);
    PrototypeBean prototypeBean3 = applicationContext.getBean(PrototypeBean.class);
	//地址一样
    DefaultBean defaultBean1 = applicationContext.getBean(DefaultBean.class);
    DefaultBean defaultBean2 = applicationContext.getBean(DefaultBean.class);
    DefaultBean defaultBean3 = applicationContext.getBean(DefaultBean.class);
}

```



# Bean的实例化

Spring容器的功能是**生成**并管理实例

## 构造方法

### 无参构造（最常用）

class属性 👉 全限定类名

```java
/**
 * 提供无参构造方法
 * 无参：最常用
 */
@Data
public class NoParamConstructorBean {
    String username;
    String password;
}
```

```xml
<!-- bean definitions here -->
<bean class="com.cskaoyan.bean.NoParamConstructorBean">
    <property name="username" value="songge"/>
    <property name="password" value="yuanzhi"/>
</bean>
```



### 有参构造

```java
/**
 * 提供有参构造方法
 */
@Data
@AllArgsConstructor
public class HasParamConstructorBean {
    String username;
    String password;
}
```

```xml
<bean class="com.cskaoyan.bean.HasParamConstructorBean">
    <!--constructor-arg标签：意味着你要使用有参构造方法
            name属性：对应的是有参构造方法的形参名
        -->
    <constructor-arg name="username" value="ligenli"/>
    <constructor-arg name="password" value="tianming"/>
</bean>
```



## 工厂

static

### 静态工厂&实例工厂

静态工厂 👉 方法是静态的

实例工厂 👉 方法不是静态的

```java
public class StaticFactory {
    public static Money create() {
        return new Money();
    }
}

public class InstanceFactory {
    public Money create() {
        return new Money();
    }
}
```

```xml
<!--**************工厂*******************-->
<!--
        factory-method属性：组件的类型是和factory-method的返回值类型相关的，组件并不是class类型
        实例工厂的时候，没有使用class属性
    -->
<bean id="moneyFromStaticFactory" class="com.cskaoyan.factory.StaticFactory" factory-method="create"/>

<bean id="instanceFactory" class="com.cskaoyan.factory.InstanceFactory"/>
<bean id="moneyFromInstanceFactory" factory-bean="instanceFactory" factory-method="create"/>
```



### FactoryBean（工厂方法）

工厂Bean的意思

BeanFactory和FactoryBean之间有什么样的联系和区别？？

都是组件注册。

BeanFactory生成所有的组件，FactoryBean生成特定的组件

XXXFactoryBean 👉 XXX

 

生产方法 👉 **getObject** **👉 通过FactoryBean注册的组件类型就和getObject的返回值是相关的**

```java
@Data
public class MoneyFactoryBean implements FactoryBean<Money> {
    String type;
    //在getObject方法中完成bean的实例化
    @Override
    public Money getObject() throws Exception {
        Money money = new Money();
        money.setType(type);
        return money;
    }
    @Override
    public Class<?> getObjectType() {
        return null;
    }
}

@Data
public class Money {
    String type;
}
```

```xml
<!--FactoryBean-->
<bean id="moneyFromFactoryBean" class="com.cskaoyan.factory.MoneyFactoryBean">
    <property name="type" value="dollar"/>
</bean>
<!--通常在整合一些其他框架的时候会使用到FactoryBean，可以对实例做一些额外的修饰-->
```



# 生命周期

Spring容器中组件的生命周期

 

组件要使用的话，要执行哪一些方法 👉 完善我们的组件

```java
public class UserServiceImpl implements UserService, BeanNameAware,
        BeanFactoryAware, ApplicationContextAware, InitializingBean, DisposableBean {

    UserDao userDao;

    public void setUserDaozzz(UserDao userDao) {
        System.out.println("2、使用到了set方法");
        this.userDao = userDao;
    }

    public void sayHello(String value) {
        System.out.println("想赚W吗?" + value);
        userDao.sayHello(value);
    }

    public UserServiceImpl() {
        System.out.println("1、userServiceImpl的无参构造方法");
    }
    String beanName;//维护一个beanName属性
    @Override
    public void setBeanName(String beanName) {
        System.out.println("3、BeanNameAware的setBeanName方法");
        this.beanName = beanName;//当前组件能够获得它在容器中的组件名（id）
    }

    BeanFactory beanFactory;//维护一个beanFactory属性
    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("4、BeanFactoryAware的setBeanFactory方法");
        this.beanFactory = beanFactory;//让当前组件能够获得BeanFactory
    }

    ApplicationContext applicationContext;//维护一个applicationContext属性
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("5、ApplicationContextAware的setApplicationContext方法");
        this.applicationContext = applicationContext;
    }


    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("7、afterPropertiesSet方法");
    }

    //自定义的init方法，方法名任意
    public void customInit() {
        System.out.println("8、自定义的init方法");

    }

    @Override
    public void destroy() throws Exception {
        System.out.println("DisposableBean的destroy");
    }
    //自定义的destroy方法，名字任意
    public void customDestroy(){
        System.out.println("自定义的destroy");
    }
}
```

```
↓
Bean的实例化（最常用无参构造方法）		一定执行到的
↓
设置参数set方法（property标签）
↓									Aware
↓									实现了Aware接口之后，才会取执行对应的Aware相关方法
↓									使用Aware是为了组件中间可以使用这样的一些值
BeanNameAware			→			setBeanName
↓
BeanFactoryAware		→			setBeanFactory
↓
ApplicationContextAware	→			setApplicationContext
↓
BeanPostProcessor的before			BeanPostProcessor并不是指当前组件实现该接口，而是单独提供一个组件
↓
InitializingBean的afterPropertiesSet
↓
自定义的init方法
↓
BeanPostProcessor的after
↓
----组件达到可用状态----
	使用组件
----容器关闭----
↓
DisposableBean的destroy				只有scope为singleton的组件会执行这两步
↓
自定义的destroy
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image034.jpg)

## BeanPostProcessor

前面的几个Aware我们都是当前组件实现接口，才会去执行对应的方法

 

通用型的处理器：在组件到达可用状态之前做一些预处理

 

**容器中有实现BeanPostProcessor类型的组件**，都会执行到该BeanPostProcessor组件的before和after方法

```java
package org.springframework.beans.factory.config;

import org.springframework.beans.BeansException;
import org.springframework.lang.Nullable;

public interface BeanPostProcessor {
    @Nullable
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Nullable
    default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }
}
//都是在组件达到可用状态之前的
```

```java
/**
 * BeanPostProcessor的作用范围：除了他本身，其他的所有组件
 *
 * 对容器中的所有的其他组件做统一的事情 👉 增强、特殊字符的处理、狸猫换太子（替换为一个增强Bean返回去, 即代理）
 */
public class CustomBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("6、BeanPostProcessor的before:" + beanName);
        //伪代码
        //if (bean instanceof DataSource){//对容器中的组件中的参数可以做一些额外的加工
        //    DataSource dataSource = (DataSource) bean;
        //    String password = dataSource.getPassword();//321ihznauy
        //    String realPassword = password.reverse(); //yuanzhi123
        //    dataSource.setPassword(realPassword);
        //    return dataSource;
        //}

        //Object proxy = proxyBean(bean);return proxy;//通过委托类组件，生成一个代理组件
        //将容器中的所有组件都替换为代理组件 👉 所有的组件的方法上都做了统一的增强
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("9、BeanPostProcessor的after:" + beanName);
        return bean;
    }
}

```

```xml
<bean class="com.cskaoyan.processor.CustomBeanPostProcessor"/>

<!--伪代码
  <bean class="datasource">
        <property name="username" value="root"/>
        <property name="password" value="321ihznauy"/>&lt;!&ndash;避免了原始的密码直接写在配置文件中&ndash;&gt;
  </bean>
-->
```



## Scope对生命周期的影响

在获得组件之前已经完成了生命周期到达可用状态之前的方法

**singleton：**在获得组件之前已经完成了上面的生命周期，容器初始化的时候

**prototype：**当你获得组件的时候才开始生命周期，每一次获得组件，都开始一个全新的生命周期

## 容器关闭

singleton才有destroy

```
↓
----容器关闭----
↓
DisposableBean的destroy
↓
自定义的destroy

(注意只有Scope为Singleton的组件会执行到这两步)
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image038.jpg)

```xml
<bean id="userDao" class="com.cskaoyan.dao.UserDaoImpl"/>
<!--配置自定义的init和destroy方法，方法名自定义-->
<bean id="userService" class="com.cskaoyan.service.UserServiceImpl"
      init-method="customInit" destroy-method="customDestroy" scope="singleton">
    <property name="userDaozzz" ref="userDao"/>
</bean>

<bean class="com.cskaoyan.processor.CustomBeanPostProcessor"/>
```

```java
@Test
public void mytest3() {
    ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
    UserService userService1 = applicationContext.getBean(UserService.class);
    UserService userService2 = applicationContext.getBean(UserService.class);
    UserService userService3 = applicationContext.getBean(UserService.class);
	
    //ApplicationContext类没有close方法，ClassPathXmlApplicationContext中有close()方法
    applicationContext.close();
}
```



# 注解（重中之重）

整个Spring阶段最常用的内容

## 组件注册功能

当前是使用bean标签完成组件的注册 👉 主要使用的class属性

 

扫描某个包目录下的类，如果这个类上包含了组件注册功能的注解 👉 就把他注册到容器中

 

1、 圈定扫描包的范围2、使用组件注册功能的注解

### 扫描包配置

```xml
<!-- 需要引入context的schema约束 -->
<!--扫描包的值：扫描当前包目录以及所有子包
        com.cskaoyan √
        com.cskaoyan.service √
        com.cskaoyan.dao.impl √
    -->
<context:component-scan base-package="com.cskaoyan"/>
```



### 组件注册功能的注解

@Component

@Service

@Repository

MVC阶段

@Controller

 

**使用注解后组件id是什么：**

**1、** **默认id（默认id是类名的首字母小写）**

**2、** **指定组件id** **👉 value属性值指定**

```java
//@Component
//@Repository //组件id：userDaoImpl
//@Repository(value = "userDao") //组件id：userDao
@Repository("userDao") //组件id：userDao 👉 如果注解中只有一个value属性，”value=“是可以省略的
public class UserDaoImpl implements UserDao{
    @Override
    public void sayHello(String value) {
        System.out.println("你把握不住啊 " + value);
    }
}
```



## 注入功能

注解注入不需要set方法，配置文件property属性注入需要set方法

### 注入组件

从容器中取出组件 👉 之前是如何从容器中取出组件 👉 applicationContext的getBean方法（3种方式：组件id、类型、id+类型）

 

使用注解可以不去使用set方法

 

1、@Autowired 按照组件的类型

2、@Autowired + @Qualifier 指定组件id

3、@Resource 默认按照组件类型，也可以使用name属性指定组件id

```java
//@Component
@Service
public class UserServiceImpl implements UserService{

    @Autowired                  //容器中该类型的组件只有一个，直接使用@Autowired
    UserDao userDao;

    @Autowired
    @Qualifier("orderDaoImpl1") //使用@Qualifier的value属性指定组件id
    OrderDao orderDao1;

    @Resource(name = "orderDaoImpl2")//name属性指定组件id
    OrderDao orderDao2;
```



**注意：注入功能的注解，要在容器中的组件中才可以使用**

### 注入值

```java
//@Value("songgeniupi") //值和代码直接耦合在一起了
@Value("${service.location}") //使用了SpEL获得了对应的key的value;需要配置文件
String location;
```

```xml
<!--application.xml-->
<!--classpath: 指的是类加载路径-->
<context:property-placeholder location="classpath:parameter.properties"/>
```

```properties
# parmeter.properties
service.location=d://myproject/spring
```

# 

## 值的引用

```xml
<!--application.xml-->
<context:property-placeholder location="classpath:parameter.properties"/>
<!--Spring配置文件中也可以引用↑的属性配置-->
<bean class="com.alibaba.druid.pool.DruidDataSource">
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean>
```

```properties
# parameter.properties
db.username=root
db.password=123456
# 如果直接写username没有写一个前缀，容器引用到系统变量
# username=root
# password=123456
```



## Scope和生命周期

@Scope

```java
@Scope("prototype")//不写名字或者不写Scope注解默认就为singleton
@Component
public class ScopeBean {
}
```



自定义的init方法：@PostConstruct

自定义的destroy方法：@PreDestroy

```java
@Component
public class LifeCycleBean {
    @PostConstruct
    public void customInit(){
        System.out.println("init");
    }
    @PreDestroy
    public void customDestroy(){
        System.out.println("destroy");
    }
}
```



## Spring对单元测试的支持

在单元测试类中直接去使用注入功能的注解(单元测试类当作是容器中的组件)

### 引入依赖spring-test

```xml
<!--版本最好和springframework一致-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.5.RELEASE</version>
</dependency>
```



### 注解写法

@Runwith 👉 junit

@ContextConfiguration 👉 spring-test

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class AnnotationTest {

    @Autowired
    UserService userService;
    @Autowired
    ApplicationContext applicationContext;
    @Test
    public void mytest1() {
        //ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        //UserService userService = applicationContext.getBean(UserService.class);
        userService.sayHello("嘎子");
    }
}
```

### 获得特定类型的组件

注解 和 Aware接口都可以获得applicationContext

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class AnnotationTest {

    @Autowired
    UserService userService;
    @Autowired
    ApplicationContext applicationContext;
    @Test
    public void mytest1() {
        //ApplicationContext applicationContext = new ClassPathXmlApplicationContext("application.xml");
        //UserService userService = applicationContext.getBean(UserService.class);
    
        String[] beanDefinitionNames = applicationContext.getBeanDefinitionNames();//获得所有的组件名称
        Map<String, UserService> beansOfType = applicationContext.getBeansOfType(UserService.class);
        Map<String, OrderDao> beansOfType1 = applicationContext.getBeansOfType(OrderDao.class);
    }
}
```





# AOP

Aspect Oriented Programming 面向切面编程 

OOP面向对象

 

增强

动态代理和AOP之间的关系是什么？ AOP是基于动态代理来实现的，

动态代理范围：委托类的全部方法

AOP：组件中特定的方法（指定方法范围 👉 更灵活的处理增强范围）

 

👉 通用性的工作

## 核心术语 

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image014-1622012742926.jpg)

# 实战案例

## 动态代理（略）

jdk动态代理和cglib动态代理

 

SpringAOP基于JDK动态代理还是CGlib动态代理？？全都要

如果你有接口的实现JDK动态代理

如果没有接口实现 cglib动态代理

## SpringAOP



![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image016-1622012742926.jpg)

### 构建userService业务并且注册到容器中

```java
@Service
public class UserServiceImpl implements UserService{
    @Override
    public void sayHello(String name) {
        System.out.println("叔，我是" + name);
    }
}
```



### 提供通知组件并且注册到容器中

implements MethodInterceptor 👉 通知组件 👉 when和what

```java
@Component
public class CustomAdvice implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        //when（相对时间）和what
        System.out.println("你把握不住啊");
        Object proceed = methodInvocation.proceed();//执行委托类的代码
        System.out.println("赚钱的事儿，让叔来");
        return proceed;
    }
}
```



### 代理组件的注册

```xml
<!--application.xml-->
<!--FactoryBean来注册代理组件 👉 ProxyFactoryBean-->
<bean id="userServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    <!--target：委托类组件是谁-->
    <property name="target" ref="userServiceImpl"/>
	<!--根据委托类组件和通知类组件注册代理组件-->
    <!--interceptorNames：代理组件根据哪个通知去做的增强-->
    <property name="interceptorNames" value="customAdvice"/>
</bean>
```



### 单元测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class MyTest {

    @Autowired
    @Qualifier("userServiceProxy") //因为委托类组件有接口的实现 👉 代理组件是根据JDK动态代理生成的 👉 也是接口类型的
    UserService userService;
    @Test
    public void mytest1() {
        userService.sayHello("嘎子");
    }

    @Autowired
    @Qualifier("orderServiceProxy")
    OrderService orderService; // 没有接口的实现 👉 cglib动态代理生成代理组件
    @Test
    public void mytest2() {
        orderService.buy();
    }
}
```

```java
@Service
public class OrderService {
    public void buy(){
        System.out.println("买假酒");
    }
}
```

```xml
<bean id="orderServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target" ref="orderService"/>
    <property name="interceptorNames" value="customAdvice"/>
</bean>
```



### 怎么样

不怎么样？

针对于每一个委托类组件，都要单独使用ProxyFactoryBean生成代理组件

每次取出组件时 👉 都要指定组件id

## 3.3   Aspectj

 

 

 

