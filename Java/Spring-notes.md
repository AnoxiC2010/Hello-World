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

![img](file:///C:/Users/ANOXIC~1/AppData/Local/Temp/msohtmlclip1/01/clip_image028.jpg)



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

