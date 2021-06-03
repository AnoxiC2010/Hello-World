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



# @Value标签读取.properties文件的中文乱码

1.在配置spring.xml文件时,声明所需的∗.properties文件时直接使用"utf−8"编码

```xml
<!--application.xml-->
<context:property-placeholder location="classpath:conf/*.properties" file-encoding="UTF-8"/>
```

2.如果在所需类上注入可使用以下方式来声明编码格式:

```java
@Component
@PropertySource(value = "classpath:conf/copyWriteUI.properties",encoding = "utf-8")
@Getter
public class CopyWriteUI {
    @Value("${a}")
    private String a;
    @Value("${b}")
    private String b;
}
```

3.不设置编码格式,编写文件时将中文转化为unicode编码

4.如果是IntelliJIDEA那么按如下图操作以上步骤都可以省去!idea会自动帮我们进行如上的第三步!

​	Settings→File Encoding→ Properties Files的Default encoding or proterties files

​	设置为UTF-8并勾选Transparent native-to-ascii conversion

- IDEA中看到的properties文件内容

  ```
  cat.name=细嗅蔷薇
  tiger.name=萌虎
  ```

- notepad中看到的proterties文件内容

  ```
  cat.name=\u7EC6\u55C5\u8537\u8587
  tiger.name=\u840C\u864E
  ```



spring `<context:property-placeholder/>`的属性说明

```xml
<context:property-placeholder   
        location="属性文件，多个之间逗号分隔"  
        file-encoding="文件编码"  
        ignore-resource-not-found="是否忽略找不到的属性文件"  
        ignore-unresolvable="是否忽略解析不到的属性，如果不忽略，找不到将抛出异常"  
        properties-ref="本地Properties配置"  
        local-override="是否本地覆盖模式，即如果true，那么properties-ref的属性将覆盖location加载的属性，否则相反"  
        system-properties-mode="系统属性模式，默认ENVIRONMENT（表示先找ENVIRONMENT，再找properties-ref/location的），NEVER：表示永远不用ENVIRONMENT的，OVERRIDE类似于ENVIRONMENT"  
        order="顺序"  
        />
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





# AspectJ

增强方法的指定需要使用Pointcut

Advice

 

首先要引入依赖aspectjweaver 👉 groupId带 org的这个版本

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```



## Pointcut切入点表达式

如何指定增强范围

### 表达式语言execution

匹配 方法

execution(修饰符 返回值 包名、类名、方法名(形参))

 

提供一个记忆的角度：能否省略、能否通配、特殊用法

#### 修饰符

可以省略 👉 全部修饰符

```xml
<!--修饰符：可以省略不写代表任意修饰符 👉 当前这个表达式也可以增强到sayHello方法-->
<aop:pointcut id="servicePointcut2" expression="execution(void com.cskaoyan.service.UserServiceImpl.sayHello(String))"/>
```



#### 返回值

可否省略：不能省略

可否通配：使用通配符* 👉 代表任意返回值

特殊用法：全类名

```xml
<!--返回值：
                  不能省略
                  使用*作为通配符 👉 代表任意类型的返回值
                  全类名
        -->
<!--当前这个切入点表达式：仅仅增强了sayHello2方法-->
<aop:pointcut id="servicePointcut3" expression="execution(String com.cskaoyan.service.UserServiceImpl.sayHello2(String))"/>
<!--当前这个表达式，可以增强到sayHello2-->
<aop:pointcut id="servicePointcut4" expression="execution(* com.cskaoyan.service.UserServiceImpl.sayHello2(String))"/>
<!--当前这个表达式，仅仅增强到sayHello3-->
<aop:pointcut id="servicePointcut5" expression="execution(com.cskaoyan.bean.User com.cskaoyan.service.UserServiceImpl.sayHello3(String))"/>
```



#### 包名、类名、方法名

```xml
<!--包名、类名、方法名：
                  能否省略: 能 👉 部分省略，头尾不能省略 👉 中间任意一部分都可以省略 ，使用 .. 来进行省略
                  能否通配: 使用通配符*
        -->
<!--当前的切入点表达式 头是com 尾是sayHello3
            中间的任意一部分都可以省略
        -->
<aop:pointcut id="servicePointcut6" expression="execution(com.cskaoyan.bean.User com.cskaoyan.service.UserServiceImpl.sayHello3(String))"/>
<aop:pointcut id="servicePointcut7" expression="execution(com.cskaoyan.bean.User com..sayHello3(String))"/>
<aop:pointcut id="servicePointcut8" expression="execution(com.cskaoyan.bean.User com..service..sayHello3(String))"/>
<aop:pointcut id="servicePointcut9" expression="execution(com.cskaoyan.bean.User com.cskaoyan..UserServiceImpl.sayHello3(String))"/>

<!--头尾都可以直接使用通配符
            通配符也可以通配一段内容的一部分 比如sayHello*
        -->
<aop:pointcut id="servicePointcut10" expression="execution(* com.cskaoyan.service..*(String))"/>
<aop:pointcut id="servicePointcut11" expression="execution(* com.cskaoyan.service..sayHello*(String))"/>
<aop:pointcut id="servicePointcut12" expression="execution(* *.cskaoyan.service..sayHello*(String))"/>
```



 

#### 形参

```xml
<!--形参：
                  能否省略：可以省略不写 👉 无参方法
                  能否通配：* 和 ..
                  特殊用法：全类名

        -->
<aop:pointcut id="servicePointcut13" expression="execution(* *.cskaoyan.service..sayHello*())"/>
<!--当前切入点表达式对应的形参：单个任意类型的形参-->
<aop:pointcut id="servicePointcut14" expression="execution(* *.cskaoyan.service..sayHello*(*))"/>
<!--当前切入点表达式对应的形参：两个任意类型的形参-->
<aop:pointcut id="servicePointcut15" expression="execution(* *.cskaoyan.service..sayHello*(*,*))"/>
<!--当前切入点表达式对应的形参：任意参数 👉 数量和类型上的任意-->
<aop:pointcut id="servicePointcut16" expression="execution(* *.cskaoyan.service..sayHello*(..))"/>
<!--如果参数类名是bean 👉 全类名-->
<aop:pointcut id="servicePointcut17" expression="execution(* *.cskaoyan.service..sayHello*(com.cskaoyan.bean.User))"/>
```



### 自定义注解 @annotation

指哪打哪（保证是容器中的组件中的方法）

#### 自定义的注解

```java
@Target(ElementType.METHOD) //自定义注解能够写在哪里 👉 方法上
@Retention(RetentionPolicy.RUNTIME) //注解在运行时生效
public @interface CountTime {
}
```



#### 通知

计算方法执行时间的通知

```java
//计算方法执行时间的通知
@Component
public class CustomAdvice implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = methodInvocation.proceed();
        long end = System.currentTimeMillis();
        System.out.println("执行时间：" + (end - start));
        return proceed;
    }
}
```



#### 切入点配置

```xml
<context:component-scan base-package="com.cskaoyan"/>
<aop:config>
    <!--@annotation(自定义注解的全类名)-->
    <aop:pointcut id="annotationPointcut" expression="@annotation(com.cskaoyan.anno.CountTime)"/>
    <aop:advisor advice-ref="customAdvice" pointcut-ref="annotationPointcut"/>
</aop:config>
<!--注解指定的方法按照advice的方式去做增强-->
```



#### 单元测试

```java
@CountTime//包含注解，被增强了→注意：要在容器中的组件中的方法里使用注解
@Override
public void sayHello(String name) {
    System.out.println("hello " + name);
    try {
        Thread.sleep(5);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

}
@Override//没有注解，没有增强
public void sayHello() {
    System.out.println("hello xxxx");
}

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class MyTest {

    @Autowired
    UserService userService;
    @Test
    public void mytest1() {
        userService.sayHello();
    }
    @Test
    public void mytest2() {
        userService.sayHello("嘎子");
    }
}
```



## Advisor 通知器

表述我们的增强 pointcut 和 advice（自定义）

 

要使用aop开头的标签 👉 aop的schema约束 引入

### 通知组件

```java
@Component
public class CustomAdvice implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        System.out.println("正道的光");
        Object proceed = methodInvocation.proceed();
        System.out.println("照在大地上");
        return proceed;
    }
}
```



### advisor

非侵入式

```xml
<!--aop标签-->
<aop:config>
    <!--
            advice-ref属性：通知组件的id
            pointcut属性：直接写切入点表达式
            pointcut-ref属性：引入切入点id
            第一个切入点表达式：userService中的sayHello方法
            👉 execution(修饰符 返回值 包名、类名、方法名(形参))
        -->
    <!--<aop:advisor advice-ref="customAdvice" pointcut="execution(public void com.cskaoyan.service.UserServiceImpl.sayHello(String))"/>-->

    <!--
            使用aop:pointcut标签相当于定义了一个全局变量 👉 后面标签里可以引用id
            id属性: 切入点的id
            expression属性：切入点表达式
        -->
    <aop:pointcut id="servicePointcut" expression="execution(public void com.cskaoyan.service.UserServiceImpl.sayHello(String))"/>
    <aop:advisor advice-ref="customAdvice" pointcut-ref="servicePointcut"/>
</aop:config>
```

多个advisor作用于同一个pointcut方法上时，写在前面的在外层，写在后面的在内层

## Aspect 切面

pointcut 和 advice（提供了一些通知 👉 时间）

 

相对时间 👉 相对于委托类的方法

before、after、around、after-returning、after-throwing

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image028-1622118850275.jpg)

### 委托类的方法

```java
@Service
public class UserServiceImpl implements UserService{
    @Override
    public String sayHello(String name) {
        String result = "hello " + name;
        System.out.println(result);
        return result;
    }
}
```



### 切面组件以及方法

```java
/**
 * 定义一个类叫切面类 👉 注册到容器中
 * 提供通知
 */
@Component//没有实现接口和继承类
public class CustomAspect {

    public void methodBefore() {
        System.out.println("before通知的方法");
    }

    public void methodAfter() {
        System.out.println("after通知的方法");
    }

    //返回值为Object，参数中包含ProceedingJoinPoint
    public Object methodAround(ProceedingJoinPoint joinPoint) throws Throwable { //环绕通知 👉 需要有一行代码 👉 委托类方法的执行
        System.out.println("around通知的方法：前半部分");
        Object proceed = null;

        proceed = joinPoint.proceed();//执行的是委托类的方法

        System.out.println("around通知的方法：后半部分");
        return proceed;
    }

    //AfterReturning通知 👉 委托类方法的执行结果，可以直接放在形参中
    //                  👉形参名可以任意写，但是需要指定
    public void methodAfterReturning(Object result){
        System.out.println("AfterReturning通知：" + result);
    }

    //AfterThrowing通知
    //通过Exception或Throwable类型的形参来接收委托类方法抛出的异常
    public void methodAfterThrowing(Exception exception){
        System.out.println("AfterThrowing通知的方法：" + exception.getMessage());
    }
}
```



### 配置文件

```xml
<aop:config>
    <aop:pointcut id="servicePointcut" expression="execution(* com..service..*(..))"/>
    <!--ref属性：指定容器中的组件为切面组件-->
    <aop:aspect ref="customAspect">
        <!--如果aop：pointcut标签写在aop:aspect标签内，作用范围就是当前的aspect-->
        <aop:pointcut id="servicePointcut2" expression="execution(* com..service..*(..))"/>
        <!--
                method属性：指定切面组件中的方法为通知方法
                pointcut属性：切入点表达式
                pointcut-ref属性：引用切入点id
                -->
        <!--<aop:before method="methodBefore" pointcut="execution(* com..service..*(..))"/>-->
        <aop:before method="methodBefore" pointcut-ref="servicePointcut"/>
        <aop:after method="methodAfter" pointcut-ref="servicePointcut"/>
        <aop:around method="methodAround" pointcut-ref="servicePointcut"/>
        <!--returning属性：method属性对应的方法中Object类型的参数名，将委托类的方法的执行结果给到这个参数-->
        <aop:after-returning method="methodAfterReturning" pointcut-ref="servicePointcut" returning="result"/>

        <!--throwing属性：method属性对应的方法中Exception(Throwable)类型的参数名，将委托类的方法抛出的异常给到这个参数-->
        <aop:after-throwing method="methodAfterThrowing" pointcut-ref="servicePointcut" throwing="exception"/>
    </aop:aspect>
</aop:config>
```



### 执行结果

before通知的方法
around统治的方法：前半部分
hello 嘎子
AfterReturning通知：hello 嘎子
around通知的方法：后半部分
after通知的方法





### AfterThrowing通知

让委托类方法抛出异常

执行结果；

before通知的方法
around统治的方法：前半部分
hello 嘎子
~~AfterReturning通知：hello 嘎子~~ AfterThrowing通知
~~around通知的方法：后半部分~~
after通知的方法

```java
//AfterThrowing通知
//通过Exception或Throwable类型的形参来接收委托类方法抛出的异常
public void methodAfterThrowing(Exception exception){//这里的参数名要和配置文件的throwing属性对应
    System.out.println("AfterThrowing通知的方法：" + exception.getMessage());
}
```

```xml
<!--throwing属性：method属性对应的方法中Exception(Throwable)类型的参数名，将委托类的方法抛出的异常给到这个参数-->
<aop:after-throwing method="methodAfterThrowing" pointcut-ref="servicePointcut" throwing="exception"/>
```



注意一个点：并不是因为新增了AfterThrowing通知 👉 执行情况发生改变 👉 而是因为抛出了异常 👉 执行情况要根据 try-catch还是抛出来的

 

### JoinPoint 连接点

可以获得执行过程中的一些值 目标类对象、代理对象、方法、参数

 

直接写入到通知方法的形参中即可

```java
@Component
public class CustomAspect {

    public void methodBefore(JoinPoint joinPoint) {
        System.out.println("before通知的方法");
        Object target = joinPoint.getTarget(); //目标类对象
        Object aThis = joinPoint.getThis();    //代理对象

        System.out.println(target);
        System.out.println(aThis);

        Signature signature = joinPoint.getSignature(); //方法的参数
        String name = signature.getName();     //方法名
        Object[] args = joinPoint.getArgs();   //参数

        System.out.println(name);
        System.out.println(Arrays.asList(args));
    }
	//ProceedingJoinPoint是JoinPoint的子类
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        Object[] args = joinPoint.getArgs();
        String methodName = joinPoint.getSignature().getName();
        Object[] argsTransfer = new Object[0];
        if ("sayHello".equals(methodName)) {
            argsTransfer =  new Object[]{"嘎子"};
        }

        Object proceed = joinPoint.proceed(argsTransfer);
        return proceed;
    }

}
```



### aspect的注解使用

aop:config标签给他变更为注解的方式

1、 指定了切入点

2、 指定了切面组件

3、 指定方法为通知方法

#### 打开注解开关

```xml
<!--application.xml-->
<aop:aspectj-autoproxy/>
```



#### 切入点方法

切入点在切面组件中以方法的形式存在

```java
/**
* 修饰符：public / private
* 返回值：void
* 方法名：任意写 👉 方法名作为切入点id
* 形参：没有参数
* 方法体：没有内容
* @Pointcut注解的value属性：切入点表达式
*/
@Pointcut("execution(* com..service..*(..))")
public void mypointcut() {
}
```

相当于

```xml
<aop:pointcut id="servicePointcut2" expression="execution(* com..service..*(..))"/>
```



#### 切面组件的指定

```java
@Aspect
@Component
public class CustomAspect {}
```

相当于

```xml
<aop:aspect ref="customAspect"></aop:aspect>
```



#### 方法的指定

@Before、@After、@Around、@AfterReturning、@AfterThrowing

写在方法上

 

注解的value属性 👉 指定增强范围

```java
/**
* value属性可以直接写切入点表达式 👉 pointcut属性
* value属性也可以引用切入点方法   👉 pointcut-ref属性
*/
//@Before("execution(* com..service..*(..))")
@Before("mypointcut()")   //如果通过切入点方法进行指定，后面有一对括号
public void methodBefore() {
    System.out.println("before通知的方法");
}

@After("mypointcut()")
public void methodAfter() {
    System.out.println("after通知的方法");
}

//返回值为Object，参数中包含ProceedingJoinPoint
@Around("mypointcut()")
public Object methodAround(ProceedingJoinPoint joinPoint) throws Throwable { //环绕通知 👉 需要有一行代码 👉 委托类方法的执行
    System.out.println("around通知的方法：前半部分");
    Object proceed = null;

    proceed = joinPoint.proceed();//执行的是委托类的方法

    System.out.println("around通知的方法：后半部分");
    return proceed;
}

//AfterReturning通知 👉 委托类方法的执行结果，可以直接放在形参中
//                  👉形参名可以任意写，但是需要指定
@AfterReturning(value = "mypointcut()",returning = "result")//对应参数名
public void methodAfterReturning(Object result){
    System.out.println("AfterReturning通知：" + result);
}

//AfterThrowing通知
//通过Exception或Throwable类型的形参来接收委托类方法抛出的异常
@AfterThrowing(value = "mypointcut()",throwing = "exception")//对应参数名
public void methodAfterThrowing(Exception exception){
    System.out.println("AfterThrowing通知的方法：" + exception.getMessage());
}
```





# Spring整合MyBatis

## 原先的MyBatis代码

```java
@Test
public void mytest1() throws Exception{
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();

    //SqlSessionFactory能否通过Spring管理 👉 √
    SqlSessionFactory sqlSessionFactory = builder.build(Resources.getResourceAsStream("mybatis.xml"));

    //SqlSession是否要通过Spring管理 👉 SqlSession线程不安全
    SqlSession sqlSession = sqlSessionFactory.openSession();

    //要考虑交给spring容器管理
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    User user = mapper.selectById(1);
    System.out.println(user);
}
```



## 引入依赖

```xml
<dependencies>
    <!--spring-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.5.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.2.5.RELEASE</version>
        <scope>test</scope>
    </dependency>
    <!--spring对mybatis的支持-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>
    <!--jdbc、tx-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.5.RELEASE</version>
    </dependency>
    <!--druid-->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.6</version>
    </dependency>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.20</version>
    </dependency>
</dependencies>

```



## 组件注册

Mapper组件

```xml
<!--application.xml-->
<context:component-scan base-package="com.cskaoyan"/>

    <!--datasource组件-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/j30_db?useUnicode=true&amp;characterEncoding=utf-8"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean>

    <!--组件类型是SqlSessionFactory-->
    <!--implements FactoryBean<SqlSessionFactory> -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--引入mybatis配置文件可以分开做更多的mybatis配置-->
        <property name="configLocation" value="classpath:mybatis.xml"/>
        <property name="typeAliasesPackage" value="com.cskaoyan.bean"/>
    </bean>

    <!--MapperScannerConfiguer mapper的扫描配置
        通过SqlSessionFactory将包目录下的mapper接口对应的mapper组件注册到容器中
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--SqlSessionFactory-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--包目录配置-->
        <property name="basePackage" value="com.cskaoyan.mapper"/>
    </bean>
<!--以上都是为了Mapper组件注册-->
```



# Spring事务

## 事务的回顾

事务的特性：

A原子性 → 数据库操作的最小单位，能够在分割

C一致性 → 一致性状态

I隔离性 → 事务操作彼此之间是隔离

D持久性 → 持久化

 

事务并发引起的问题：脏读、不可重复读、虚（幻）读

脏读：一个事务读取到另外一个事务**还没有提交的**数据

不可重复读：一个事务读取到另外一个事务**已经提交的**数据（数据变更）

虚读：一个事务读取到另外一个事务**已经提交**的数据（数据量变化）

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image058.png)

 

数据库的隔离级别



|          | 脏读 | 不可重复读 | 虚读 |
| -------- | ---- | ---------- | ---- |
| 读未提交 | ×    | ×          | ×    |
| 读已提交 | √    | ×          | ×    |
| 可重复读 | √    | √          | ×    |
| 串行化   | √    | √          | √    |

 

mysql默认的隔离级别是什么？ 可重复读 → MySql不会导致虚读

 

## 核心接口

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image060.jpg)

### PlatformTransactionManager 事务管理器

Spring要管理事务 → 一定要使用到平台事务管理器

 

DataSourceTransactionManager

HibernateTransactionManager

 

getTransaction

commit

rollback

```java
package org.springframework.transaction;

import org.springframework.lang.Nullable;

public interface PlatformTransactionManager extends TransactionManager {
    //传入TransactionDefinition获得TransactionStatus
    TransactionStatus getTransaction(@Nullable TransactionDefinition var1) throws TransactionException;
	//传入TransactionStatus做提交和回滚
    void commit(TransactionStatus var1) throws TransactionException;

    void rollback(TransactionStatus var1) throws TransactionException;
}
```

开发人员不需关注status，而是关注definition配置方法

### TransactionStatus 事务的状态

是否已完成，是否是新事物，是否有保存点...

过程值，开发过程中其实不会使用到

TransactionStatus.class INherited members

```
<I> TransactionStatus
	m createSavepoint(): Object→SavepointManager
	m flush():void
	m hasSavepoint():boolean
	m isCompleted():boolean→TransactionExecution
	m isNewTransaction():boolean→TransactionExecution
	m isRollbackOnly():boolean→TransactionExecution
	m releaseSavepoint(Object):void→SavepointManager
	m rollbackToSavepoint(Object):void→SavepointManager
	m setRollbackOnly():void→TransactionExecution
```



###  TransactionDefinition 事务的定义

事务的名称、隔离级别、只读属性、**传播行为**、超时时间、回滚的异常、不回滚的异常

 

#### 传播行为

多个方法之间如何来共享事务。

多个方法之间存在调用关系，发生异常的时候，如何回滚。

method2、method1

##### Required 默认的传播行为

如果不包含事务，就新增一个事务；如果你包含事务，我就加入进来，作为一个事务。

 

一荣俱荣，一损俱损：要么一起提交，要么一起回滚。

methodB 调用了methodA

methodA发生异常：都回滚

methodB发生异常：都回滚

##### Requires_new

如果不包含事务，就新增一个事务；如果包含了事务，则新建一个新的事务。

 

自私型：外围不能影响内部，内部可以影响外围。

methodB 调用了methodA

methodA发生异常：A是内部。A、B都回滚

methodB发生异常：B是外围。B回滚

 

##### nested

如果不包含事务，就新增一个事务；如果包含了事务，则以嵌套事务的方式运行。

 

无私型：外围可以影响内部，内部不会影响外围。 方法之间存在调用关系的时候，内部的方法不重要。

methodB 调用了methodA

methodA发生异常：A是内部。A回滚

methodB发生异常：B是外围。AB回滚

 

PDD → 获得新用户

 

register（外围） → sendCoupon（内部）

发放优惠券失败了，注册要保留下来

注册失败了，优惠券也失败



## 事务的案例

TransactionManager → 依赖于 DataSource

```xml
<!--application.xml-->
<!--TransactionManager-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```



MyBatis的事务交给Spring来进行管理 → 每一次执行方法都会提交

### transactionTemplate 事务模板

```xml
<!--application.xml-->
<!--TransactionManager-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

```java
@Service
public class AccountServiceImpl implements AccountService{

    @Autowired
    AccountMapper accountMapper;

    @Autowired
    TransactionTemplate transactionTemplate;

    @Override
    public void transfer(Integer fromId, Integer destId, Double money) {
        Double fromMoney = accountMapper.selectMoneyById(fromId);
        Double destMoney = accountMapper.selectMoneyById(destId);

        fromMoney -= money; //计算转账后应该有多少钱
        destMoney += money; //计算转账后应该有多少钱

        Double finalFromMoney = fromMoney;
        Double finalDestMoney = destMoney;
        Object execute = transactionTemplate.execute(new TransactionCallback<Object>() {
            //将需要增加事务的代码 → 放入到doInTransaction方法中即可
            @Override
            public Object doInTransaction(TransactionStatus transactionStatus) {
                int x = accountMapper.updateMoney(fromId, finalFromMoney);
                int i = 1 / 0;
                int y = accountMapper.updateMoney(destId, finalDestMoney);
                return (x + y); //如果需要增加事务的代码 它需要返回值 → 直接返回去
            }
        });//transactionTemplate的execute方法的返回值 → doInTransaction方法的返回值

        //如果不需要返回值
        /*transactionTemplate.execute(new TransactionCallbackWithoutResult() {
            @Override
            protected void doInTransactionWithoutResult(TransactionStatus transactionStatus) {
                 //把需要增加事务的代码放入进来即可
            }
        });*/
    }
}
```



### TransactionProxyFactoryBean

将一个委托类对象 → 生成一个事务的代理对象  → 类似于SpringAOP案例

 

```xml
<!--application.xml-->
<bean id="accountServiceProxy" class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
    <!--target-->
    <property name="target" ref="accountServiceImpl"/>
    <!--transactionManager-->
    <property name="transactionManager" ref="transactionManager"/>
    <!--transactionAttributes → Definition-->
    <property name="transactionAttributes">
        <!--properties: props-->
        <props>
            <!--
                    key：方法名
                    value：definition
                        ISOLATION_XXX:隔离级别
                        PROPAGATION_XXX:传播行为
                        readOnly: 只读
                        timeout_xxx: 数字，单位是秒
                        +XXXException: noRollbackFor
                        -XXXException: rollbackFor
                -->
            <!--<prop key="transfer">ISOLATION_DEFAULT,PROPAGATION_REQUIRED</prop>-->
            <!--<prop key="transfer">ISOLATION_DEFAULT,PROPAGATION_REQUIRED,readOnly</prop>-->
            <!--<prop key="transfer">ISOLATION_DEFAULT,PROPAGATION_REQUIRED,timeout_5</prop>-->
            <prop key="transfer">ISOLATION_DEFAULT,PROPAGATION_REQUIRED,+java.lang.ArithmeticException</prop>
        </props>
    </property>
</bean>
```

```java
@Service
public class AccountServiceImpl implements AccountService{

    @Autowired
    AccountMapper accountMapper;

    @Override
    public void transfer(Integer fromId, Integer destId, Double money) {
        Double fromMoney = accountMapper.selectMoneyById(fromId);
        Double destMoney = accountMapper.selectMoneyById(destId);

        fromMoney -= money; //计算转账后应该有多少钱
        destMoney += money; //计算转账后应该有多少钱

        int x = accountMapper.updateMoney(fromId,fromMoney);
        int i = 1 / 0;
        int y = accountMapper.updateMoney(destId,destMoney);
    }
}
```

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:application.xml")
public class MyTest {

    @Autowired
    @Qualifier("accountServiceProxy")
    AccountService accountService; //代理对象

    @Test
    public void mytest1(){
        accountService.transfer(1,2,5000D);
    }
}
```

 

### advisor （advice组件）

```xml
<!--pom.xml-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```

```xml
<!--application.xml-->
<!--之前整合mybatis相关配置-->
<!--TransactionManager-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<aop:config>
    <aop:pointcut id="transactionPointcut" expression="execution(* com.cskaoyan.service..*(..))"/>
    <aop:advisor advice-ref="transactionAdvice" pointcut-ref="transactionPointcut"/>
</aop:config>

<!--tx:advice标签提供了advice通知-->
<tx:advice id="transactionAdvice" transaction-manager="transactionManager">
    <!--definition-->
    <tx:attributes>
        <!--
                name属性:方法名，也可以使用通配符
                其他属性：definition相关的属性
            -->
        <tx:method name="*" isolation="REPEATABLE_READ" propagation="REQUIRED" />
    </tx:attributes>
</tx:advice>
```



### Transactional（最简单也是最重要）

打开注解开关 → tx:annotation-driven

```xml
<!--application.xml-->
<tx:annotation-driven transaction-manager="transactionManager"/>
```

```java
package org.springframework.transaction.annotation;
@Target({ElementType.TYPE, ElementType.METHOD})//可以写在类或方法上
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {
    @AliasFor("transactionManager")
    String value() default "";

    @AliasFor("value")
    String transactionManager() default "";

    Propagation propagation() default Propagation.REQUIRED;

    Isolation isolation() default Isolation.DEFAULT;

    int timeout() default -1;

    boolean readOnly() default false;

    Class<? extends Throwable>[] rollbackFor() default {};

    String[] rollbackForClassName() default {};

    Class<? extends Throwable>[] noRollbackFor() default {};

    String[] noRollbackForClassName() default {};
}
```

```java
//@Transactional//写在这里作用于类中所有方法
@Service
public class AccountServiceImpl implements AccountService{

    @Autowired
    AccountMapper accountMapper;
	//使用TransactionDefinition相关属性
    @Transactional(isolation = Isolation.REPEATABLE_READ,propagation = Propagation.NESTED)
    @Override
    public void transfer(Integer fromId, Integer destId, Double money) {
        Double fromMoney = accountMapper.selectMoneyById(fromId);
        Double destMoney = accountMapper.selectMoneyById(destId);

        fromMoney -= money; //计算转账后应该有多少钱
        destMoney += money; //计算转账后应该有多少钱

        int x = accountMapper.updateMoney(fromId,fromMoney);
        int i = 1 / 0;
        int y = accountMapper.updateMoney(destId,destMoney);
    }
}
```



# JavaConfig

使用Java代码来进行Spring的配置

 

xml、注解

 

干掉xml配置文件 → SpringBoot → 干掉xml

## 配置类

@Configuration **把当前类作为容器中的组件，同时呢作为配置类**

```java
@Configuration
public class SpringConfiguration {}
```



## 组件注册

xml注册组件 → java方法来注册

以方法的形式存在

```java
@Configuration//注册为容器组件和作为配置类
public class SpringConfiguration {
    /**
    * 组件注册
    * 返回值：对应的组件的class或者其接口
    * 方法名：作为默认的组件id
    * 也可以使用@Bean注解的value属性指定组件id
    */
    //@Bean //组件id是druidDataSource
    @Bean("dataSource") //组件id是dataSource
    public DruidDataSource druidDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/j30_db?useUnicode=true&characterEncoding=utf-8");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }
}
```

相当于

```xml
<!--application.xml-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/j30_db?useUnicode=true&amp;characterEncoding=utf-8"/>
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean>
```

同理其他配置

```java
@Configuration//注册为容器组件和作为配置类
@ComponentScan("com.cskaoyan")//扫描包
@PropertySource("classpath:db.properties")//配置文件路径
@EnableAspectJAutoProxy//aspectj注解配置
@EnableTransactionManagement//开启事务
public class SpringConfiguration {

    @Value("${db.username}")//从配置文件获取参数，如配置datasource用
    String username;
    //注意用junit测试发现这是读取不到的，日志提示：INFO: Cannot enhance @Configuration bean definition 'springConfiguration' since its singleton instance has been created too early. The typical cause is a non-static @Bean method with a BeanDefinitionRegistryPostProcessor return type: Consider declaring such methods as 'static'.
    //应该是实例化顺序的问题，这在springboot中应该不存在这问题
    /**
     * 组件注册
     * 返回值：对应的组件的class或者其接口
     * 方法名：作为默认的组件id
     * 也可以使用@Bean注解的value属性指定组件id
     */
    //@Bean //组件id是druidDataSource
    @Bean("dataSource") //组件id是dataSource
    public DruidDataSource druidDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/j30_db?useUnicode=true&characterEncoding=utf-8");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }
    @Bean("dataSource2") //组件id是dataSource
    public DruidDataSource druidDataSource2(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/cskaoyan_db?useUnicode=true&characterEncoding=utf-8");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    /**
     * 形参：从容器中取出对应的类型的组件。默认是按照类型去取，容器中该类型的组件只有一个。
     *                                通过@Qualifier指定组件id
     */
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(@Qualifier("dataSource") DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource); //也应该是来源于容器中的DataSource组件
        return sqlSessionFactoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
        mapperScannerConfigurer.setSqlSessionFactoryBeanName("sqlSessionFactory");
        mapperScannerConfigurer.setBasePackage("com.cskaoyan.mapper");
        return mapperScannerConfigurer;
    }
    @Bean
    public DataSourceTransactionManager transactionManager(@Qualifier("dataSource") DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }
}
```

相当于

```xml
<!--application.xml-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/j30_db?useUnicode=true&amp;characterEncoding=utf-8"/>
    <property name="username" value="${db.username}"/>
    <property name="password" value="${db.password}"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"/>
</bean>

<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    <property name="basePackage" value="com.cskaoyan.mapper"/>
</bean>

<!--TransactionManager-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--aspectj注解开关-->
<aop:aspectj-autoproxy/>
```



在config类中使用bean注解把不是我们自己写的类的实例注册为组件，而@Bean注解也可以用于我们自己写的类，只需要提供一个返回对象的方法。

## 功能性配置

### 扫描包 @ComponentScan

context:component-scan base-package

```java
@Configuration
@ComponentScan("com.cskaoyan")
public class SpringConfiguration {}
```

相当于

```xml
<!--application.xml-->
<context:component-scan base-package="com.cskaoyan"/>
```



### aspectj → @EnableAspectJAutoProxy

```java
@Configuration
@ComponentScan("com.cskaoyan")
@EnableAspectJAutoProxy//打开aspectj注解开关
public class SpringConfiguration {}
```

相当于

```xml
<!--application.xml-->
<aop:aspectj-autoproxy/>
```



### 事务 → @EnableTransactionManagement

```java

@Configuration
@ComponentScan("com.cskaoyan")
@EnableTransactionManagement//开启事务注解开关
public class SpringConfiguration {...}
```

相当于

```xml
<!--application.xml-->
<tx:annotation-driven transaction-manager.../>
```



### 引入properties配置文件

```java
@Configuration
@ComponentScan("com.cskaoyan")
@PropertySource("classpath:db.properties")//引入properties配置文件
public class SpringConfiguration {}
```

相当于

```xml
<!--application.xml-->
<context:property-placeholder location="classpath:db.properties"/>
```



## 加载配置类

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)//使用classes属性加载配置类
public class MyTest {

    @Autowired
    AccountService accountService;

    @Test
    public void mytest1(){
        accountService.transfer(1,2,5000D);
    }
}
```





# JavaEE

控制层Servlet

# SpringMVC

干掉Servlet → 基于Servlet

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image002-1622211001072.jpg)

 

SpringMVC的核心流程

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image004-1622211001072.jpg)

在初始化DispatcherServlet的过程中构建了Spring容器



HandlerMapping映射url到handler， HandlerAdapter请求参数类型转换和参数校验等等...

# 入门案例1

## 引入依赖

5+2（web\webmvc）+1

aop&beans&context&core&expression + webmvc&web + jcl

servlet-api(provided)

```xml
<!--pom.xml-->   
<packaging>war</packaging>
    <dependencies>
        <!--5+2+1 都包括了 之前用的spring-context只包含5+1-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>3.0-alpha-1</version>
            <scope>provided</scope><!--打包web应用时不要打包进去-->
        </dependency>
    </dependencies>
```



## DispatcherServlet

通过Servlet的生命周期的init方法会维护一个WebApplicationContext

 

doGet、doPost → doDispatch → 通过handlermapping和HandlerAdapter → handler（反射）

```xml
<!--web.xml-->
<!--DispatcherServlet
        Servlet : Class、ServletMapping
        WebApplicationContext → 在DispatcherServlet初始化的时候 → init方法 → initWebApplicationContext
        给DispatcherServlet提供一个参数 → set方法 → contextConfigLocation
    -->
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:application.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <!--
	全局 除了jsp的请求. 
	/*会出问题因为.jsp默认要交给tomcat的jspservlet处理,
	-->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

但是dispatcherServlet的url-pattern为/取代了tomcat处理静态资源的defaultservlet会导致静态资源不能显示，spring提供了静态资源放行的配置：

[参考]([不能访问webapp下的图片文件_JavaWeb技术（5）：Servlet的理解（下）_weixin_39522486的博客-CSDN博客](https://blog.csdn.net/weixin_39522486/article/details/111126479))

```xml
<!--application.xml-->
<mvc:default-servlet-handler/><!-- 静态资源放行 -->
<mvc:annotation-driven/><!-- 开启注解支持 -->
```

这个配置让DispatchServlet发现请求是一个静态资源那么会交给default servlet即交给Tomcat处理

## 中文乱码

注意
表单的提交方式必须是post
在web.xml中配置CharacterEncodingFilter编码格式要和JSP页面的编码格式一致
解决中文乱码必须使用过滤器(在DispatcherServlet之前执行)，而不能使用springmvc的拦截器，因为过滤器在DispatcherServlet之前，所以设置好编码后，DispatcherServlet和Controller都可以获取到正确的数据，而拦截器运行在DispatcherServlet之后，也即是意味着DispatcherServlet获取的数据已经是乱码，那么在拦截器中调整乱码是没有意义的

```xml
<!--web.xml 不用自己写，spring提供了-->
<!-- 配置 CharacterEncodingFilter解决中文乱码问题-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

    <!-- 配置编码格式为UTF-8 -->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



## 配置文件

```xml
<!--application.xml-->
<!-- 扫描包配置 -->
<context:component-scan base-package="com.cskaoyan"/>

<!--HandlerMapping和HandlerAdapter-->
<mvc:annotation-driven/>
<!--这个标签做了很多工作，比如响应jason也是它做的-->
```



## 使用handler

以方法的形式存在 → 容器中的@Controller组件中的方法 → HandlerMethod（Handler方法）

/hello → 响应hello springmvc

```java
@Controller
public class HelloController {

    //Handler方法 → url映射到Handler方法上
    @RequestMapping("/hello")
    public String hello(){
        return "/hello.jsp";
    }
}
```



## 使用Handler响应json数据

Jackson-databind

```xml
<!--pom.xml-->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.11.4</version>
</dependency>
```

```java
@Controller
public class HelloController {

    @RequestMapping("/hello/user")
    @ResponseBody //响应json数据，要增加一个@ResponseBody注解
    public User helloUser(){
        User user = new User();
        user.setUsername("松哥");
        user.setPassword("远志");
        return user;
    }
}
```



## 响应Json数据

```java
//@Controller
//@ResponseBody //当前类下的所有方法，响应的都是Json数据 →
@RestController// 引申注解@RestController = @ResponseBody+ @Controller
public class HelloController {

    @RequestMapping(value = {"home","index","home/index"})
    //@ResponseBody
    public BaseRespVo index(){
        return BaseRespVo.ok("访问主页");
    }

    @RequestMapping({"hello*","hello/*"})
    //@ResponseBody
    public BaseRespVo hello(){
        return BaseRespVo.ok("你好");
    }
}
```

不加注解浏览器404

```
The origin server did not find a current representation for the target resource or is not willing to disclose that one exists.
```

# Handler的映射关系@RequestMapping

默认get post请求都接收 最开始的/可写可不写

## url路径映射（核心）

将请求url和Handler方法之间建立映射关系 → 使用value属性建立映射关系

### 将多个url映射到同一个handler方法上

/index

/home

/home/index

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Mapping
public @interface RequestMapping {
    String name() default "";

    @AliasFor("path")
    String[] value() default {};//数组 多个
    ...
```

```java
@RequestMapping(value = {"home","index","home/index"})//利用value属性是数组，将多个url映射到一个handler方法上
@ResponseBody
public BaseRespVo index(){
    return BaseRespVo.ok("访问主页");
}
```



### 使用通配符映射

/hello*

/hello/*

```java
@RequestMapping({"hello*","hello/*"})
@ResponseBody
public BaseRespVo hello(){
    return BaseRespVo.ok("你好");
}
```



### 一个url映射到不同的方法上？？？

其实是可以 → 请求方法不同 → 后面再讲

## 窄化请求

user/login

user/register

user/modify

```java
@RestController
@RequestMapping("user")//开头的/可以加也可以不加
public class UserController {
	//同理这里也是开头的/可以加也可以不加
    //@RequestMapping("user/login")
    @RequestMapping("login") //映射的url → 类上的requestMapping的value属性值 + 方法上的value属性
    public BaseRespVo login(){
        return BaseRespVo.ok("登录");
    }
    //@RequestMapping("user/register")
    @RequestMapping("register")
    public BaseRespVo register(){
        return BaseRespVo.ok("注册");
    }
    //@RequestMapping("user/modify")
    @RequestMapping("modify")
    public BaseRespVo modify(){
        return BaseRespVo.ok("更新");
    }
//    @RequestMapping("user/delete")
}
```



## 请求方法限定 method属性

```java
/**
 * 请求方法限定
 */
@RestController
@RequestMapping("method")
public class MethodController {

    //@RequestMapping(value = "/get",method = RequestMethod.GET)
    @GetMapping("get")
    public BaseRespVo getLimit(){
        return BaseRespVo.ok("GET方法");
    }

    //@RequestMapping(value = "/post",method = RequestMethod.POST)
    @PostMapping("post")
    public BaseRespVo postLimit(){
        return BaseRespVo.ok("POST方法");
    }

    //多个请求方法之间的关系是OR
    @RequestMapping(value = "double",method = {RequestMethod.GET,RequestMethod.POST})
    public BaseRespVo doubleLimit(){
        return BaseRespVo.ok();
    }
}
```



### 引申注解

@GetMapping、@PostMapping

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(//这里
    method = {RequestMethod.GET}
)
public @interface GetMapping {...}

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(//这里
    method = {RequestMethod.POST}
)
public @interface PostMapping {...}
```

```java
//@RequestMapping(value = "/get",method = RequestMethod.GET)
@GetMapping("get")
public BaseRespVo getLimit(){
    return BaseRespVo.ok("GET方法");
}

//@RequestMapping(value = "/post",method = RequestMethod.POST)
@PostMapping("post")
public BaseRespVo postLimit(){
    return BaseRespVo.ok("POST方法");
}
```





## 请求参数限定 params → 400

这个问题时请求参数封装有问题，需要和前端沟通协调

请求过程中要携带什么样的参数

```java
/**
 * 请求参数限定
 */
@RestController
@RequestMapping("param")
public class ParamController {

    //先不管请求参数的接口，只管请求过程中要携带什么参数
    //localhost:8080/param/login?username=songge&password=yuanzhi
    @RequestMapping(value = "login",params = {"username!=songge","password"})//多个参数之间的关系 and
                                             //一定要携带username和password两个参数，且username不能等于songge
    public BaseRespVo login(){
        return BaseRespVo.ok();
    }

}
```



# Handler方法对应的@RequestMapping

## 请求头限定 headers

```java
@RestController
public class HeaderController {
    //headers 请求头的key有哪些 👉 多个请求头之间的关系 and
    @RequestMapping(value = "header/limit",headers = {"Cskaoyan-Token","My-Tag"})
    public BaseRespVo headerLimit(){
        return BaseRespVo.ok();
    }
}
```



## 特定请求头的值的限定

值的语法 xxx/xxx

### Content-Type → consumes

```java
//Content-Type
@RequestMapping(value = "header/consumes",consumes = "xxx/yyy")
public BaseRespVo contentType(){
    return BaseRespVo.ok();
}
```



### Accept → produces

```java
//限定的是value
//Accept
@RequestMapping(value = "header/produces",produces = "abc/def")
public BaseRespVo accept(){
    return BaseRespVo.ok();
}
```



# Handler方法的返回值

## 视图相关（了解）

不要增加@ResponseBody和@RestController

单体应用的时候，响应视图信息 ModelAndView

### ModelAndView

```java
@Controller
public class HelloController {
    @RequestMapping("hello1")
    public ModelAndView hello1(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("/WEB-INF/view/hello1.jsp");
        modelAndView.addObject("content", "泰勒");
        return modelAndView;
    }
}
//测试在@RestController下也能返回视图，但是不要这样做
```

```jsp
<!--在hello1.jsp中取数据-->
<body>
    <h1>hello  ${content}</h1>
</body>
```



### String 👉 视图名

```java
@RequestMapping("hello2")
public String hello2(Model model) {
    model.addAttribute("content", "斯威夫特");
    return "/WEB-INF/view/hello2.jsp";
}
//测试在@RestController下会404
```

同ModleAndView用jsp取数据

##  Json（主要是Json）

Jackson依赖 `<mvc:annotation-driven/>`

 

1、 Handler方法上增加@ResponseBody

2、 类上增加@ResponseBody或@RestController

# Handler方法的形参

主要要做的事情 👉 获得请求所携带的参数 👉 主要做的是请求参数的接收

 

之前request.getParameter

构造一个业务场景 👉 register

## 直接接收

```java
@RestController
@RequestMapping("user")
public class UserController {
    //可以使用request来接收，但是呢不建议
    @RequestMapping("register")
    public BaseRespVo register(HttpServletRequest request){
        String username = request.getParameter("username");
        return BaseRespVo.ok();
    }
}
```



**请求参数名和Handler方法的形参名一致**

### 字符串、基本类型、包装类

直接接收：1、请求参数名和Handler方法的形参名一致 2、包装类

```java
//请求参数名 和 handler方法的形参名一致
//如果要接收 👉 建议使用包装类
//localhost:8080/user/register?username=songge&password=yuanzhi&age=25&married=true
@RequestMapping("register")//本身接收的是字符串→SpringMVC提供了转换器,int最好用Integer,否则null和""会抛异常
public BaseRespVo register(String username,String password,int age,boolean married){
    //public BaseRespVo register(String username,String password,String age,String married){
    //Integer integer = Integer.valueOf(age);
    return BaseRespVo.ok();
}
```



### 数组接收

```java
//数组 👉 构造请求的时候，构造了多个相同的请求参数
//请求参数名和Handler方法形参名一致
//localhost:8080/user/register2?username=songge&password=yuanzhi&age=25&married=true
//                  &hobbys=sing&hobbys=dance&hobbys=rap&hobbys=basketball
@RequestMapping("register2")
public BaseRespVo register2(String username,String password,int age,boolean married,String[] hobbys){
    return BaseRespVo.ok(hobbys);
}
```



### Date

```java
//Date 👉 它其实是有类型转换器 👉 指定日期转换器的格式
//localhost:8080/user/register3?username=songge&password=yuanzhi&age=25&married=true
//                  &hobbys=sing&hobbys=dance&hobbys=rap&hobbys=basketball
//                  &birthday=1991-05-04
@RequestMapping("register3")
public BaseRespVo register3(String username, String password,
                            int age, boolean married, String[] hobbys, 
                            @DateTimeFormat(pattern = "yyyy-MM-dd") Date birthday){
    return BaseRespVo.ok();
}
```

如果不用注解指定日期转换格式：

```
浏览器端：
HTTP Status 400 – Bad Request
Type Status Report

Description The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

Apache Tomcat/8.5.37

IDEA debug日志：
29-May-2021 20:49:03.521 WARNING [http-nio-8080-exec-9] org.springframework.web.servlet.handler.AbstractHandlerExceptionResolver.logException Resolved [org.springframework.web.method.annotation.MethodArgumentTypeMismatchException: Failed to convert value of type 'java.lang.String' to required type 'java.util.Date'; nested exception is org.springframework.core.convert.ConversionFailedException: Failed to convert from type [java.lang.String] to type [java.util.Date] for value '2021-05-03'; nested exception is java.lang.IllegalArgumentException]
```



### 自定义转换器

```java
package org.springframework.core.convert.converter;
import org.springframework.lang.Nullable;
@FunctionalInterface
public interface Converter<S, T> {
    @Nullable
    T convert(S var1);
}//第一个泛型：参数类型；第二个泛型：返回值类型
```



```java
@Component
public class String2DateConverter implements Converter<String, Date> {
    @Override
    public Date convert(String s) {
        //转换业务 👉 自定义转换业务

        SimpleDateFormat simpleDateFormat = null;
        if (s.length() == 10) {
            simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        } else if (s.length() == 19) {
            simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        }

        Date parse = null;
        try {
            parse = simpleDateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return parse;
    }
}
```

```xml
<!--application.xml-->   
<mvc:annotation-driven conversion-service="conversionService"/>

<!--将自定义的转换器添加到SpringMVC的转换器列表里-->
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters"><!--set类型-->
        <set><!--局部组件-->
            <!--<bean class="com.cskaoyan.converter.String2DateConverter"/>-->
            <!--通过ref标签的bean属性指定组件id-->
            <ref bean="string2DateConverter"/><!--set中的数据-->
        </set>
    </property>
</bean>
```





### 文件类型

文件的接收 👉 非常简单

1、 接收到

2、 存起来

MultipartResolver

#### 引入依赖

commons-io\commons-fileupload

```xml
<!--pom.xml-->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.4</version>
</dependency>
```



#### 构造文件上传的请求

```html
<h1>单个文件的上传</h1>
<!--注意请求url 编码类型 请求类型-->
<form action="file/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="myfile"><br><!--请求参数名是myfile-->
    <input type="submit">
</form>
```



#### 组件注册

```xml
<!--application.xml-->
<!--组件id是一个固定值--><!--SpringMVC适合用这个组件的时候实际上是按照组件的id去使用的-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize" value="5120000"/>
</bean>
```



#### handler方法来接收文件

```properties
# param.proterties
# 配置上传文件的目录
file.uploadPath=d://stone/spring
```

```xml
<!--application.xml-->
<context:property-placeholder location="classpath:param.properties"/>
```

```java
@RestController
@RequestMapping("file")
public class FileController {

    //@Value("d://stone/spring")
    @Value("${file.uploadPath}")//对应配置文件
    String filePath;


    @RequestMapping("upload") //file/upload 👉 form表单中的action
    public BaseRespVo upload(MultipartFile myfile/*对应请求参数名*/) throws IOException {//使用MultipartFile来接收请求参数中的文件
        //获得上传的文件名
        String originalFilename = myfile.getOriginalFilename();
        String name = myfile.getName(); //请求参数名=myfile
        //获得上传的文件类型Content-Type
        String contentType = myfile.getContentType();
        //获得文件大小
        long size = myfile.getSize();
        //获得输入流
        InputStream in = myfile.getInputStream();

        //文件名也可以使用UUID或随机数生成器生成一个随机值 👉 避免出现上传同名文件时会覆盖
        //构造一个File接收MultipartFile 👉 文件上传
        File file = new File(filePath,originalFilename); //配置接收的路径和文件名
        myfile.transferTo(file); //文件上传

        return BaseRespVo.ok();
    }
}
```



#### 接收多个文件

```html
<h1>多个文件的上传</h1>
<form action="file/list/upload" enctype="multipart/form-data" method="post">
    <%--multiple:允许选择多个文件--%>
    <input type="file" multiple name="myfiles"><br><!--需要multiple布尔属性-->
    <input type="submit">
</form>
```

```java
@RequestMapping("list/upload")
public BaseRespVo listUpload(MultipartFile[] myfiles) throws IOException {
    for (MultipartFile myfile : myfiles) {
        //获得上传的文件名
        String originalFilename = myfile.getOriginalFilename();
        File file = new File(filePath,originalFilename); //配置接收的路径和文件名
        myfile.transferTo(file); //文件上传
    }
    return BaseRespVo.ok();
}
```



## JavaBean请求参数接收

直接接收 👉 JavaBean

localhost:8080/register?username=songge&password=yuanzhi&age=25&married=true&birthday=1991-05-04

 

register(String username,String password,Integer age,Boolean married,Date birthday,String[] hobbys)

 

👉 Javabean

 

```java
@Data
public class User {
    String username;
    String password;
    Integer age;
    Boolean married;
    @DateTimeFormat(pattern = "yyyy-MM-dd")//可以使用@DateTimeFormmat注解也可以使用自定义转换器
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") //响应的json数据格式
    Date birthday;
    String[] hobbys;
}
```

```java
@RestController
@RequestMapping("user")
public class UserController {

    //将请求参数对应的形参 👉 变更为javabean的成员变量
    //请求参数名 对应javabean的成员变量名 （set方法）
    @RequestMapping("register")
    public BaseRespVo register(User user) {
        return BaseRespVo.ok(user);
    }
}
```



### 嵌套JavaBean

```java
@Data
public class User {
    String username;
    String password;
    Integer age;
    Boolean married;
    @DateTimeFormat(pattern = "yyyy-MM-dd")//可以使用@DateTimeFormmat注解也可以使用自定义转换器
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") //响应的json数据格式
    Date birthday;
    String[] hobbys;

    //http://localhost:8080/user/register?
    //      username=songge&password=yuanzhi&age=25&married=true&birthday=1991-05-04&hobbys=sing
    //      &userDetail.email=songge@cskaoyan.com&userDetail.mobile=110
    //嵌套JavaBean 👉 请求参数名和JavaBean的成员变量名一致
    UserDetail userDetail;//注意 变量.变量 的请求参数写法
}
@Data
public class UserDetail {
    String email;
    String mobile;
}
```





###  javaBean的list

```java
@Data
public class User {
    //http://localhost:8080/user/register?
    //      username=songge&password=yuanzhi&age=25&married=true&birthday=1991-05-04&hobbys=sing
    //      &userDetail.email=songge@cskaoyan.com&userDetail.mobile=110
    //      &orderList[0].location=huashan&orderList[0].price=500
    //      &orderList[1].location=guanggu&orderList[1].price=400
    //      &orderList[2].location=hanjie&orderList[2].price=300

    //构造了一个请求👉 发现400 👉 请求参数[] 👉 URL编码
    //[ %5b
    //] %5d
    //&orderList%5b0%5d.location=huashan&orderList%5b0%5d.price=500
    //&orderList%5b1%5d.location=guanggu&orderList%5b1%5d.price=400
    //&orderList%5b2%5d.l  ocation=hanjie&orderList%5b2%5d.price=300
    //JavaBean的List 👉 请求参数名和JavaBean的成员变量名一致
    List<Order> orderList;//注意中括号在URL中不能直接写，需要编码才行
}
```

```java
@Data
public class Order {
    String location;
    Double price;
}
```



关于URL编码

[关于URL编码 - 阮一峰的网络日志 (ruanyifeng.com)](http://www.ruanyifeng.com/blog/2010/02/url_encoding.html)

[UrlEncode编码/UrlDecode解码 - 站长工具 (chinaz.com)](http://tool.chinaz.com/tools/urlencode.aspx)



## Json数据的接收

### 构造Json的请求

请求方法：post

Content-Type：application/json

data：Json字符串

 

postman构造Json请求

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image018-1622463776338.jpg)

### 构造一个请求的Json数据和对应的JavaBean

```java
@RestController
@RequestMapping("user")
public class UserController { 
    /**
     * @RequestBody 意味着接收的是Json数据
     * {"username":"songge","password":"yuanzhi"}
     */
    //@RequestMapping(value = "login", method = RequestMethod.POST)
    @PostMapping("login")
    public BaseRespVo login(@RequestBody LoginUser user) {
        return BaseRespVo.ok(user);
    }

    //@RequestMapping(value = "login", method = RequestMethod.GET)
    @GetMapping("login")
    public BaseRespVo login2(LoginUser user) { //localhost:8080/user/login?username=songge&password=yuanzhi
        return BaseRespVo.ok(user);
    }
}
//同一个URL映射到两个Handler方法，一个处理json参数请求，一个处理普通键值对参数请求
```



### 以Map来接收json数据

```java
//当你封装的值比较少
//JavaBean可以固定数据类型
@PostMapping("map/login")
public BaseRespVo mapLogin(@RequestBody Map user) {
    Object username = user.get("username");
    Object password = user.get("password");
    return BaseRespVo.ok(user);
}
```



## 响应的json的日期格式

@JsonFormmat

响应和接收的Json日期格式

```java
@Data
public class User {
    @DateTimeFormat(pattern = "yyyy-MM-dd")//可以使用@DateTimeFormmat注解也可以使用自定义转换器
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss",timezone = "GMT+8") //响应的json数据格式
    Date birthday;
}
```

{"data":{birthday":"1991-05-03 23:00:00"}}



## 其他参数

在handler方法的形参中还能够写什么

### request和response

```java
@RequestMapping("reqAndResp")//了解但不建议
public BaseRespVo reqAndResp(HttpServletRequest request, HttpServletResponse response) {
    return BaseRespVo.ok();
}
```



### Cookie

能否直接放在形参中？ 否

通过request来获得cookie

```java
@RequestMapping("cookie")
public BaseRespVo cookie(HttpServletRequest request) {
    Cookie[] cookies = request.getCookies();
    for (Cookie cookie : cookies) {
        String name = cookie.getName();
        String value = cookie.getValue();
        System.out.println(name + " → " + value);
    }
    return BaseRespVo.ok();
}
```



#### 构造cookie

chrome浏览器

developer tools -> application -> storate -> session storage

配置cookie的name和value

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image030-1622463776341.jpg)

通过postman的Headers设置Cookie请求头

通过分号来分割多个cookie

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image032-1622463776341.jpg)

### Session

能否直接写到形参中？？ 行 HttpSession

也可以通过request去获得

```java
@RequestMapping("session/put")
public BaseRespVo session1(HttpSession session, String value) {
    session.setAttribute("content", value);
    return BaseRespVo.ok();
}
//直接放在形参中或通过request来获得session
@RequestMapping("session/get")
//public BaseRespVo session2(HttpSession session) {
public BaseRespVo session2(HttpServletRequest request) {
    HttpSession session = request.getSession();
    Object content = session.getAttribute("content");
    return BaseRespVo.ok(content);
}
```



# RESTful

表述性状态传递

通过请求的表述去携带信息

 

localhost:8080/user

GET：查询

POST：新增

DELETE：删除

PUT：更新

 

前后端分离的应用，请求方法GET或POST

 

不同的请求url 👉 **资源 +** **操作**

 

localhost:8080/user/query

localhost:8080/user/update

localhost:8080/user/insert

localhost:8080/user/delete

 

可读性、窄化请求、配置拦截器、配置权限

 

响应JSON数据 👉 @ResponseBody、@RestController

 

注解：获得请求中携带的表述的状态 👉 请求中哪一些内容会给我们提供参数 👉 提供到Handler方法的形参中 👉 形参中要去获得一些值

 

1、 请求URL 👉 @PathVariable

2、 请求参数 👉 @RequestParam （没啥用）

3、 请求头 👉 @RequestHeader

4、 Cookie 👉 @CookieValue

5、 Session 👉 @SessionAttribute

 

## 请求URL 👉 @PathVariable

weixin_44991304/article/details/117355320

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image036-1622463776344.jpg)

用户名/article/details/文章id 👉 使用占位符

```java
@RestController
public class ArticleController {

    //使用大括号来指定占位符
    @RequestMapping("{username}/article/details/{id}")
    public BaseRespVo articleDetails(@PathVariable("username") String name,
                                     @PathVariable("id") Integer id) {
        System.out.println("id: " + id);
        System.out.println("name: " + name);
        return BaseRespVo.ok(name);
    }
}
```



## 请求参数 👉 @RequestParam （没啥用）

获得是请求参数中的值

```java
//localhost:8080/login?username=songge&password=yuanzhi
@RequestMapping("login")
public BaseRespVo login(@RequestParam("username") String name,
                        @RequestParam("password") String pwd) {
    return BaseRespVo.ok();
}
//比较繁琐,直接将形参名和请求参数对应起来即可->String username,String password
```



## 请求头 👉 @RequestHeader

请求头的值，通过指定请求头的key来获得对应的value

```java
//可以直接以String来接收对应的请求头的值，也可以使用数组来接收（请求头中的值里面包含 逗号）
@RequestMapping("header/get")
public BaseRespVo headerGet(@RequestHeader("Accept") String[] accept,
                            @RequestHeader("Host") String host){
    return BaseRespVo.ok();
}
```



## Cookie 👉 @CookieValue

通过cookie的name来获得cookie的value

```java
//通过cookie的name来获得对应的value
@RequestMapping("cookie/value")
public BaseRespVo cookieValue(@CookieValue("ligenli") String ligenli,
                              @CookieValue("songge") String songge){
    System.out.println(ligenli);//@CookieValue后面括号里是cookie的name
    System.out.println(songge);
    return BaseRespVo.ok();
}
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image042-1622463776346.jpg)

## Session 👉 @SessionAttribute

```java
@RequestMapping("session/put/{value}")
public BaseRespVo sessionPut(HttpSession httpSession, @PathVariable("value") String value) {
    httpSession.setAttribute("30th",value);
    return BaseRespVo.ok();
}

@RequestMapping("session/get")//通过key去除对应的value
public BaseRespVo getSession(@SessionAttribute("30th") String value) { //Session这里接收的value是Object，
    // 放入的是什么类型，就取出什么类型
    return BaseRespVo.ok(value);
}
```



# 静态资源处理

为什么我们要做静态资源的处理？

当我们整合SpringMVC之后，静态资源访问不到了。原先是由默认的servlet来进行出来，SpringMVC把静态资源处理的请求抢过去，并且没有提供对应的handler来进行处理

```xml
<!--web.xml,default-servlet从dispatcherServlet截获静态资源-->
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
</servlet-mapping>
```

## 默认的servlet

李冰 父子 治水 👉 都江堰

分流

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image048-1622463776347.jpg)

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

仅能访问到webapp路径（web资源根路径）下的资源

## default-servlet-handler

由DispatcherServlet分给Default-servlet-handler

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image052-1622463776350.jpg)

```xml
<!--application.xml-->
<mvc:default-servlet-handler/>
```



## 静态资源映射 ResourceHandler

映射的url是什么

静态资源在哪里

```xml
<!--application.xml-->
<!--静态资源映射 ResourceHandler
            mapping属性：映射url 👉 **意味着多级url
            location属性：静态资源所处的位置 👉 最后要加一个/
                1、web资源根路径 👉 /
                2、类加载路径classpath 👉 classpath:/
                3、文件路径 👉 file:路径
        想要访问到静态资源：mapping中的值 + 静态资源相对于location的值
    -->
<mvc:resources mapping="/pic/**" location="/"/>
<mvc:resources mapping="/pic2/**" location="classpath:/"/>
<mvc:resources mapping="/pic3/**" location="file:D:/stone/spring/"/><!--实际开发常用-->
```



# 异常处理

Handler方法向上抛出的异常

## HandlerExceptionResolver（了解）

统一的异常处理 👉 抛出异常 👉 之前响应视图相关的数据时

ModelAndView resolveException

```java
//注册到容器中即生效
//@Component
public class CustomHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override//Object handler是handler方法,e是抛出的异常
    public ModelAndView resolveException(HttpServletRequest httpServletRequest,
                                         HttpServletResponse httpServletResponse, Object handler, Exception e) {
        String message = e.getMessage();
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("/exception.jsp");
        modelAndView.addObject("message", message);
        return modelAndView;
    }
}
```



## ExceptionHandler（推荐）

通过异常的类型进行映射 👉 不同类型的异常可以通过不同的handler方法来处理

```java
@ControllerAdvice
//@ResponseBody
//@RestControllerAdvice = @ControllerAdvice + @ResponseBody
public class ExceptionControllerAdvice {
	//如果没有@ResponseBody或@RestControllerAdvice响应的是ModelAndView
    @ExceptionHandler(ArithmeticException.class)
    public String arithmeticException(){ //返回值String 👉 ModelAndView中的viewName
        return "/exception.jsp";
    }

    @ExceptionHandler(SensitiveWordException.class)
    @ResponseBody//对应的异常对象可以直接写在形参中,获得抛出的异常对象,也可以获得携带的信息
    public BaseRespVo sensitiveException(SensitiveWordException sensitiveWordException){
        //抛出异常时 可以携带信息
        //处理方法中 可以获得携带的信息
        String word = sensitiveWordException.getWord();//抛出异常时封装的信息
        return BaseRespVo.fail(word + "敏感词，查水表");
    }

    //可以通过一个方法处理多种类型的异常 //value=Class<? extends Throwable>[] 说明可以接收异常数组
    @ExceptionHandler({ParameterException.class,NullPointerException.class})//映射多个异常
    @ResponseBody
    public BaseRespVo parameterException(){
        return BaseRespVo.fail("参数有误");
    }
}
```



顶顶顶



# 1    拦截器

filter

## 1.1   CharacterEncodingFilter

字符编码 👉 设定字符集

POST请求过程中携带中文字符会乱码

```xml
<!--CharacterEncodingFilter-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```



## 1.2   HandlerInterceptor

handler方法的拦截器 👉 对Handler方法做增强

 

### 1.2.1 执行过程

HandlerMapping 👉 执行计划

 

`HandlerExecutionChain` 👉 `Handler`、`List<HandlerInterceptor>`

```java
package org.springframework.web.servlet;
public class DispatcherServlet extends FrameworkServlet {
    //这里调用到getHandler方法
    protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    	...
        mappedHandler = this.getHandler(processedRequest);
  		...
    }
     @Nullable
    protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
        if (this.handlerMappings != null) {
            Iterator var2 = this.handlerMappings.iterator();

            while(var2.hasNext()) {
                HandlerMapping mapping = (HandlerMapping)var2.next();
                HandlerExecutionChain handler = mapping.getHandler(request);
                if (handler != null) {
                    return handler;
                }
            }
        }

        return null;
    }
}
```

```java
package org.springframework.web.servlet;
public class HandlerExecutionChain {
    private static final Log logger = LogFactory.getLog(HandlerExecutionChain.class);
    private final Object handler;//handler方法
    @Nullable
    private HandlerInterceptor[] interceptors;
    @Nullable
    private List<HandlerInterceptor> interceptorList;//拦截链
    private int interceptorIndex;
}
```



### 1.2.2 HandlerInterceptor接口

preHandle 👉 Handler方法 👉 postHandle 👉 afterCompletion

```java
package org.springframework.web.servlet;
public interface HandlerInterceptor {
    //返回布尔类型
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler/*handler方法*/) throws Exception {
        return true;//如果为true可以继续流程，为false则中断流程
    }
	//Handler方法如果已经响应了Jsom，这里可以修改最终结果
    //response采用JavaEE方式做修改
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView/*Handler方法响应的ModelAndView如果响应的是json，该值为null*/) throws Exception {
    }
	//最终处理的方法→类似于finally→如果当前的preHandler执行结果为true，则一定可以执行到当前的afterCompletion方法
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```



![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image010-1622554259767.jpg)

### 1.2.3 HandlerInterceptor的配置

```java
@Component
public class CustomHandlerInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}
```



```xml
<!--application.xml-->    
<!--mvc:interceptors-->
<mvc:interceptors>
    <!--之前讲converter的时候也使用过bean子标签和ref子标签-->
    <!--<bean class="com.cskaoyan.interceptor.CustomHandlerInterceptor"/>-->
    <ref bean="customHandlerInterceptor"/><!--注册为组件后可以用bean子标签引用id-->
</mvc:interceptors>
```



### 1.2.4 作用范围

局部范围

```xml
<!--application.xml-->
<!--mvc:interceptors-->
<mvc:interceptors>

    <!--使用bean标签或ref标签的时候，并没有指定作用范围 👉 全局范围（所有的Handler方法） 👉 DispatcherServlet作用范围下的全局-->
    <!--之前讲converter的时候也使用过bean子标签和ref子标签-->
    <!--<bean class="com.cskaoyan.interceptor.CustomHandlerInterceptor"/>-->
    <!--<ref bean="customHandlerInterceptor"/>-->
    <mvc:interceptor>
        <!--path属性：指定当前interceptor的作用范围
                bean标签或ref标签：interceptor是谁

                path属性的语法：
                    /hello/** 所有以hello作为开头的请求url
                    /hello/* 通配一级目录 /hello/abc
                    /hello   特定的请求
                /user/** 👉 针对于user相关的请求 ← 可对应经过窄化请求的特定的Controller里的Handler方法
            -->
        <mvc:mapping path="/hello/**"/>
        <ref bean="customHandlerInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```



### 1.2.5 多个interceptor的执行顺序

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image018-1622554259767.jpg)![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image020-1622554259768.jpg)

preHandler1

preHandler1

preHandler1

hello world

postHandler3

postHandler2

postHandler1

afterCompletion3

afterCompletion2

afterCompletion1



### 1.2.6 如果HandlerInterceptor的preHandle返回值为false

如果preHandle返回值为true，则继续流程。并且当前interceptor的afterCompletion一定可以执行到；

如果preHandle返回值为false，则中断流程。

#### 1.2.6.1  preHandle1返回值为false

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image024-1622554259768.jpg)

#### 1.2.6.2  preHandle2返回值为false

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image026-1622554259768.jpg)

#### 1.2.6.3  preHandle3返回值为false

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image028-1622554259768.jpg)

#### 1.2.6.4  提问

如果我有5个HandlerInterceptor

返回值为true 👉 12345 54321 54321

如果只有3的preHandle返回值为false 👉 123 21

如果只有2和4的preHandle为false 👉 12 1

 

#### 1.2.6.5  练习

/hello请求 👉 把所有的user信息都以json数据的形式响应（只有完成登录才能够访问其他的请求）

/login请求 👉 登录操作 👉 如果登录成功 👉 /hello请求就可以访问

 

HandlerInterceptor 👉 作用范围是什么？preHandle方法中做什么业务

# 2    Validator

Hibernate-validator 校验器 👉 参数校验

 

之前针对每个参数都要写代码 👉 校验逻辑繁琐 👉 多次反复使用 👉 controller、service

 

将校验逻辑和Model绑定 👉 JavaBean

 

校验的是请求参数 👉 JavaBean来接收请求参数 👉 成员变量

 

校验逻辑 👉 JavaBean中的成员变量

## 2.1   搭建一个login的业务场景

```java
@RestController
public class UserController {

    //localhost:8080/login?username=xxx&password=xxx
    //username的长度至少6位
    //password的长度在6到10位
    @RequestMapping("login")
    public BaseRespVo login(User user){
        /*String username = user.getUsername();
        if (username == null || username.length() < 6) {
            return BaseRespVo.fail("username长度至少是6位");
        }
        String password = user.getPassword();
        if (password == null || password.length() < 6 || password.length() > 10) {
            return BaseRespVo.fail("password的长度在6到8位");
        }*/
        return BaseRespVo.ok();
    }
}
```



## 2.2   引入依赖

hibernate-validator

```xml
<!--pom.xml-->        
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.1.7.Final</version>
</dependency>
```



## 2.3   注册组件

```xml
<!--applicatiom.xml-->
<mvc:annotation-driven validator="validator"/>
<!--看源码会发现这个FactoryBean本质上是一个Validator，和其他FactoryBean有区别-->
<bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
    <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
</bean>
```



## 2.4   校验注解

```java
@Data
public class User {
    @Length(min = 6)//校验功能的注解直接绑定在成员变量上
    String username;
    @Length(min = 6, max = 10)
    String password;
} 
```

```java
    //localhost:8080/login?username=xxx&password=xxx
    //username的长度至少6位
    //password的长度在6到10位
    @RequestMapping("login")
    //public BaseRespVo login(@Valid User user){//@Valid或@Validated注解告知SpringMVC请求参数需要校验
    public BaseRespVo login(@Validated User user, BindingResult bindingResult){
       
        return BaseRespVo.ok();
    }
```



## 2.5   常见的注解

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image040-1622554259769.jpg)

## 2.6   处理校验结果

给到的页面是400 👉 Json

而当前我们并没有进入到Handler方法中

 

要获得校验结果并且要进入到Handler方法中 👉 形参中增加一个BindingResult（参数校验的结果）

```java
public class ValidUtil {
    public static BaseRespVo valid(BindingResult bindingResult){
        //拿到没有校验成功的成员变量 👉 哪一个请求参数没有通过校验
        FieldError fieldError = bindingResult.getFieldError();

        //成员变量名 👉 请求参数名
        String field = fieldError.getField();
        //哪一个请求参数对应的值没有通过校验
        Object rejectedValue = fieldError.getRejectedValue();

        //没有校验通过提供的默认的消息
        String defaultMessage = fieldError.getDefaultMessage();

        String message = "请求参数" + field + "因为" + rejectedValue + "没有通过校验;" + defaultMessage;
        return BaseRespVo.fail(message);
    }
}
```

```java
@RequestMapping("login")
//public BaseRespVo login(@Valid User user){
public BaseRespVo login(@Validated User user, BindingResult bindingResult){
    //可以根据校验结果做个性化的处理
    if (bindingResult.hasFieldErrors()) { //校验结果中有否有错误
        return ValidUtil.valid(bindingResult);
    }
    return BaseRespVo.ok();
}
```



## 2.7   default-message

```java
@Data
public class User {
    @NotNull
    @Length(min = 6)
    String username;
    @Length(min = 6, max = 10,message = "length must between 6 and 10")
    String password;//检验注解中都可以使用message属性修改默认的消息

    @Min(18)
    @Max(70)
    Integer age;
}
```



## 2.8   提供外部的配置文件

提供消息 👉 MessageSource

```java
//@Length(min = 6, max = 10,message = "length must between 6 and 10")
@Length(min = 6, max = 10, message = "{user.password}")//使用大括号来引用key
String password;
```

```xml
<!--application.xml-->    
<mvc:annotation-driven validator="validator"/>

<bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
    <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
    <property name="validationMessageSource" ref="messageSource"/>
</bean>
<!--上面validator引用这里的的messageSource的id-->
<bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    <property name="basename" value="classpath:valid"/>
    <property name="defaultEncoding" value="utf-8"/>
</bean>
```



# 3    国际化i18n

针对于同一个key在不同的地区下是不同的message

hello 

中文 你好

泰文 萨瓦迪卡 

 

Locale 👉 地区信息

```xml
<!--application.xml-->
<!--Locale管理 👉 组件id也是一个固定值-->
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver">
    <!--cookieName:cookie中的哪一个name维护locale信息-->
    <property name="cookieName" value="language"/>
    <!--默认的locale-->
    <property name="defaultLocale" value="en_US"/>
</bean>
```

```java
    @RequestMapping("locale/get")
    public BaseRespVo getLocale(Locale locale){ //写在形参中，获得的是默认的locale信息
        return BaseRespVo.ok(message);//配置了cookieName:local信息和cookieName所对应的value相关
    }
```



## 3.1   Locale信息能做什么

MessageSource 👉 国际化的配置文件properties

 

新增多个配置文件，每个配置文件 对应一个locale信息，将多个配置文件当成是一组配置文件

```
IDEA中resources文件加会以这种形式么显示
文件名后面跟着locale信息
▽Resource Bundle 'param' 
	param_en_US.properties
	param_th_TH.properties
	param_zh_CN.properties
```

```java
@Autowired
MessageSource messageSource;

@RequestMapping("locale/get")
public BaseRespVo getLocale(Locale locale){ //写在形参中，获得的是默认的locale信息
    //这里根据传入的locale会找到对应locale的properties的user.password的值
    String message = messageSource.getMessage("user.password", /*Object[]类型*/null, locale);
    return BaseRespVo.ok(message);//找到不同的locale对应的配置文件按照key来获取对应的value
}
```

占位符的用法

```properties
# param_en_US.properties
user.password={0} length must between 6 and 10 {1}
```

```java
    @Autowired
    MessageSource messageSource;

    @RequestMapping("locale/get")
    public BaseRespVo getLocale(Locale locale){ //写在形参中，获得的是默认的locale信息

        //第一个参数：String 👉 配置文件key
        //第二个参数：为占位符提供值 👉 配置文件中的value里的占位符
        //第三个参数：locale信息
        String[] value = {"枫哥","牛批"};//对应properties文件中的{0}和{1}占位符，注意从0开始
        String message = messageSource.getMessage("user.password", value, locale);

        return BaseRespVo.ok(message);
    }
```



## 3.2   我们做的message和locale也可以直接给到validator使用

```java
@Data
public class User {
    //@Length(min = 6, max = 10,message = "length must between 6 and 10")
    @Length(min = 6, max = 10, message = "{user.password}")
    String password;//可以根据locale获得不同的locale对应的配置文件中的value
}
```



# 4    JavaConfig

## 4.1   使用xml配置

将一个容器变为两容器 WebApplicationContext 👉 父容器、子容器

**父容器不能使用子容器中的组件，子容器可以使用父容器中的组件**

**为什么要分家？**

**不分开行不行？** **行**

**分开也行？** **行**

 

**应用程序启动之后，第一次访问请求的时候，稍微慢一点，如果组件比较多，启动过程会比较慢**

 

**让和SpringMVC****没有直接关系的组件** **先注册**

 

**ContextLoaderListener**

### 4.1.1  web.xml

```xml
<!--web.xml-->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<context-param>
    <param-name>contextConfigLocation</param-name><!--父容器-->
    <param-value>classpath:application.xml</param-value>
</context-param>

<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name><!--子容器，可以使用父容器组件-->
        <param-value>classpath:application-mvc.xml</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```



### 4.1.2 spring配置文件

```xml
<!--application.xml-->
<!--排除掉@Controller注解对应的组件-->
<context:component-scan base-package="com.cskaoyan">
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

<bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/j30_db?useUnicode=true&amp;characterEncoding=utf-8"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="datasource"/>
</bean>

<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    <property name="basePackage" value="com.cskaoyan.mapper"/>
</bean>

<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="datasource"/>
</bean>
<tx:annotation-driven transaction-manager="txManager"/>
```



### 4.1.3 springmvc的配置文件

```xml
<!--application-mvc.xml-->
<context:component-scan base-package="com.cskaoyan.controller"/>
<mvc:annotation-driven/>
```



## 4.2   JavaConfig

干掉配置文件

web.xml 👉 加载Spring配置文件、加载SpringMVC配置文件、servlet-mapping

application.xml 👉 扫描包、组件注册

application-mvc.xml 👉 扫描包、mvc:annotation-driven、mvc的相关配置

### 4.2.1 web.xml 👉 AACDSI

```java
/**
 * 相当于web.xml 👉 AACDSI 嗷嗷吃到死
 */
public class ApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    //加载Spring配置类
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfiguration.class};
    }
    //加载SpringMVC配置类
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{MvcConfiguration.class};
    }
    //配置DispatcherServlet的servlet-mapping
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```



#### 4.2.1.1  characterEncodingFilter

```java
public class ApplicationInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override//Spring默认的编码过滤器
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("utf-8");
        filter.setForceEncoding(true);
        return new Filter[]{filter};
        //return new Filter[]{new CharacterEncodingFilter("UTF-8", true)};//可以直接这么写
        //不用设置urlmapping，也没法设置，默认是/*
    }
}
```



### 4.2.2 Spring配置类

```java
@Configuration
@ComponentScan(value = "com.cskaoyan",
        excludeFilters = @ComponentScan.Filter(value = {Controller.class, EnableWebMvc.class}))
        //excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class))
		//这两中写法都可以，因为不屑type的默认类型就是FilterType.ANNOTATION
@EnableTransactionManagement
public class SpringConfiguration {//其余注册组件和spring的javaConfig一致

    @Bean
    public DruidDataSource druidDataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/j30_db?useUnicode=true&characterEncoding=utf-8");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource) {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer() {
        MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
        mapperScannerConfigurer.setSqlSessionFactoryBeanName("sqlSessionFactory");
        mapperScannerConfigurer.setBasePackage("com.cskaoyan.mapper");
        return mapperScannerConfigurer;
    }
    @Bean
    public DataSourceTransactionManager dataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
        //return new DataSourceTransactionManager(dataSource);//或这么写
    }
}
```



### 4.2.3 SpringMVC配置类

```java
@ComponentScan("com.cskaoyan.controller")
@EnableWebMvc//相当于<mvc:annotation-driven/>
public class MvcConfiguration implements WebMvcConfigurer {//该接口提供了mvc标签的功能
}
```

相当于

```xml
<!--application-mvc.xml-->
<context:component-scan base-package="com.cskaoyan.controller"/>
<mvc:annotation-driven/>
```



#### 4.2.3.1  MultipartResolver

```java
@Bean
public CommonsMultipartResolver multipartResolver() {//组件id是固定值，我们采用方法名作为默认的组件id了
    CommonsMultipartResolver commonsMultipartResolver = new CommonsMultipartResolver();
    commonsMultipartResolver.setMaxUploadSize(512000000);
    return commonsMultipartResolver;
}
```



#### 4.2.3.2  LocaleResolver

```java
@Bean
public CookieLocaleResolver localeResolver(){//组件id为固定值
    CookieLocaleResolver cookieLocaleResolver = new CookieLocaleResolver();
    cookieLocaleResolver.setCookieName("language");
    cookieLocaleResolver.setDefaultLocale(Locale.SIMPLIFIED_CHINESE);
    return cookieLocaleResolver;
}
```



#### 4.2.3.3  mvc:resources

静态资源映射： mapping location

```java
//mvc:resources
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    //       mapping属性                                location属性
    registry.addResourceHandler("/pic/**").addResourceLocations("file:d://stone/spring/");
    registry.addResourceHandler("/pic2/**").addResourceLocations("/");
    registry.addResourceHandler("/pic3/**").addResourceLocations("classpath:/picture/");

}
```



#### 4.2.3.4  mvc:interceptors

```java
//mvc:interceptors
//作用范围什么 interceptor是谁
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new CustomHandlerInterceptor());//作用范围全局
    registry.addInterceptor(new CustomHandlerInterceptor2()).addPathPatterns("/hello/**");//局部范围
}
```



#### 4.2.3.5  converter

formattingConversionServiceFactoryBean → converters（set）

mvc:annotation-driven conversion-service

```java
//自定义转换器
@Override
public void addFormatters(FormatterRegistry registry) {
    registry.addConverter(new String2DateConverter());
}
//自定义转换器2//这种方法很繁琐，不常用
@Autowired
ConfigurableConversionService conversionService;//取出来
@PostConstruct
public void addConverter(){
    conversionService.addConverter(new String2DateConverter());//增加上
}
@Primary//由于又放了一个相同类型的ConfigurableConversionService实例，这个注解将使新加的具有优先被使用权
@Bean
public ConfigurableConversionService conversionService(){
    return conversionService;//放回去
}
```

 

#### 4.2.3.6  validator

```java
@Override
public Validator getValidator() {
    //注意看源码这个LocalValidatorFactoryBean是implements Validator
    //这个FactoryBean本质上是一个Validator，和其他FactoryBean不同
    LocalValidatorFactoryBean localValidatorFactoryBean = new LocalValidatorFactoryBean();
    localValidatorFactoryBean.setProviderClass(HibernateValidator.class);
    return localValidatorFactoryBean;
}
```

 



# 1    SpringBoot

快速启动（搭建）一个Spring应用

 

**更方便的引入一些其他的框架**

**更轻量、更灵活**

**约定大于配置** **→** **默认值**

**不需要外部的JavaEE****容器，内置了JavaEE****容器（环境的统一）**

**可以以Jar****包的方式运行 java -jar xxx.jar**

 

**jy** **和女朋友去逛街**

**看到一点点** **，我要喝一点点** **→** **一点点**

**我要喝一点点** **→** **一点点**

**如果我没告诉买什么牌子的奶茶，就买一点点**

**我要喝奶茶** **→** **一点点**

**我要喝喜茶** **→** **喜茶** **（指定了，以指定的为准）**

 

**给到一个默认值（注册默认组件），如果我们指定了，默认值失效**

 

 

Rod Johnson Spring → JavaEE

Google → Android 2.3、4.0

 

S轻量级的开源框架、配置地狱、魔鬼

 

SpringBoot更轻量 

 

# 2    搭建一个SpringBoot应用

## 2.1   官网搭建

start.spring.io

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image002-1622638493050.jpg)

## 2.2   idea

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image004-1622638493051.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image006-1622638493051.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image008-1622638493051.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image010-1622638493051.jpg)

# 3    SpringBoot应用

## 3.1   启动类

```java
@SpringBootApplication//包含了扫描包的配置：启动类所在的包目录
public class Demo1Application {
    public static void main(String[] args) {
        SpringApplication.run(Demo1Application.class, args);
    }
}
```



## 3.2   pom.xml

```xml
<!--pom.xml-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId><!--固定写法-->
    <version>2.1.5.RELEASE</version><!--使用的SpringBoot应用的版本-->
    <relativePath/> <!-- lookup parent from repository -->
</parent>
<groupId>com.cskaoyan</groupId><!--创建SpringBoot应用时选择的信息-->
<artifactId>demo</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>demo</name>
<description>Demo project for Spring Boot</description>
<properties>
    <java.version>1.8</java.version>
</properties>
```



当我们引入依赖的时候，有些依赖没有写版本号，给到了一个默认的版本号，如果你指定了版本号，以你指定的为准

```xml
<!--spring-boot-starter-parent-2.1.5.RELEASE.pom-->  
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.1.5.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>
  <artifactId>spring-boot-starter-parent</artifactId>
  <packaging>pom</packaging>
  <name>Spring Boot Starter Parent</name>
  <description>Parent pom providing dependency and plugin management for applications
		built with Maven</description>
  <url>https://projects.spring.io/spring-boot/#/spring-boot-starter-parent</url>
```

```xml
<!--spring-boot-dependencies-2.1.5.RELEASE.pom-->
<mysql.version>8.0.16</mysql.version><!--这是默认的版本号-->
...
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>${mysql.version}</version>
    <exclusions>
        <exclusion>
            <artifactId>protobuf-java</artifactId>
            <groupId>com.google.protobuf</groupId>
        </exclusion>
    </exclusions>
</dependency>
```



## 3.3   starter依赖

1、 引入该框架所支持的依赖

2、 引入自动配置（约定大于配置）的依赖 → 自动注册默认的组件

org.springframework.boot:spring-boot.autoconfigure.2.1.5.RELEASE

xxx-autoconfigurer

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image022-1622638493052.jpg)

starter依赖会带来一个特性：当我们想要使用一个框架的时候，基本上我们只需要引入它的starter依赖，我们就已经配置好了这个框架，也有一些框架需要做极少的配置

# 4    约定大于配置

自动配置默认值 → 默认值 默认的组件 → JavaConfig

## 4.1   配置文件中的值

springboot配置文件application.properties

### 4.1.1 @Value

之前使用@Value能够获得properties配置文件中的值

```java
@RestController
public class FileController {

    @Value("${file.path}")//获得配置文件中的值
    String filePath;

    @RequestMapping("file/upload")
    public BaseRespVo fileUpload(){
        return BaseRespVo.ok(filePath);
    }
}
```

```java
@Configuration
public class DataSourceConfiguration {

    @Value("${db.driverClassName}")//获得配置文件中的值，完成组件注册
    String driverClassName;
    @Value("${db.url}")
    String url;
    @Value("${db.username}")
    String username;
    @Value("${db.password}")
    String password;

    @Bean
    public DruidDataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```



### 4.1.2 @ConfigurationProperties

从配置文件中取出值 → 根据成员变量名

```properties
# application.properties
file.path=d://stone/spring/
db.driverClassName=com.mysql.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/j30_db?useUnicode=true&characterEncoding=utf-8
db.username=root
db.password=123456
```

```java
/**
 * @ConfigurationProperties注解要和组件注册功能的注解一起使用
 * 使用到了set方法 → 需要提供set
 * @ConfigurationProperties注解的prefix属性值 + 成员变量名 == 配置文件中的key
 */
@Data
@Configuration
@ConfigurationProperties(prefix = "db")//这里很奇怪会飘红，提示没有注册组件或者@EnableConfigurationProperties引用，但是不影响运行，因为@Configuration或者RestController等就是注册组件
public class DataSourceConfiguration2 {

    String driverClassName;
    String url;
    String username;
    String password;
    String size;

    @Bean
    public DruidDataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```



### 4.1.3 @EnableConfigurationProperties

引入一个参数类，在参数类中使用@ConfigurationProperties

```java
@Data
@ConfigurationProperties(prefix = "db")
public class DataSourceProperties {//单独写一个参数类→提供来源于配置文件中的值
    String driverClassName;
    String url;
    String username;
    String password;
}
```

 

引入参数类

```java
/**
 * 在配置类中不直接写 参数
 * 而是提供一个参数类对象 → 参数类中封装这些对象
 */
@Configuration//引入参数类（包含@ConfigurationProperties注解的类）
@EnableConfigurationProperties(DataSourceProperties.class)
public class DataSourceConfiguration3 {

    DataSourceProperties properties;//提供成员变量以及有参构造方法

    public DataSourceConfiguration3(DataSourceProperties properties) {
        this.properties = properties;
    }

    @Bean
    public DruidDataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(properties.getDriverClassName());
        dataSource.setUrl(properties.getUrl());
        dataSource.setUsername(properties.getUsername());
        dataSource.setPassword(properties.getPassword());
        return dataSource;
    }
}
```



### 4.1.4 配置的提示

@ConfigurationProperties（prefix）

```xml
<!--pom.xml--> <!--引入依赖之后，重新run一下应用程序-->      
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```



如果不行的话，打开build重新build一下

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image038-1622638493055.jpg)

## 4.2   约定大于配置原理

SpringBoot核心 → 默认组件

 

starter依赖中包含了xxx-autoconfigure → 提供了自动配置类

### 4.2.1 找到自动配置类

xxx-autoconfigurer依赖中/META-INF/spring.factories, `AutoConfiguration=List<String>`

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image040-1622638493056.jpg)

```properties
# spring-boot-autoconfigure-2.5.0.jar!/META-INF/spring.factories
# 自动配置类
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
...
```



### 4.2.2 启动类

```java
@SpringBootApplication
public class Demo1Application {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

```java
package org.springframework.boot.autoconfigure;
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration//这里启动了自动配置
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {}
```

```java
package org.springframework.boot.autoconfigure;
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})//筛选自动配置类
public @interface EnableAutoConfiguration {
    String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

    Class<?>[] exclude() default {};

    String[] excludeName() default {};
}
```

```java
package org.springframework.boot.autoconfigure;
public class AutoConfigurationImportSelector implements DeferredImportSelector, BeanClassLoaderAware, ResourceLoaderAware, BeanFactoryAware, EnvironmentAware, Ordered {
	...
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return NO_IMPORTS;
        } else {
            AutoConfigurationImportSelector.AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(annotationMetadata);//这个this.get方法返回了一个xxxEntry对象
            return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
            //将Collection转换为String[]  -- xxxEntry的getConfigurations()方法返回的是一个集合
        }
    }
    ...
    protected AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
        if (!this.isEnabled(annotationMetadata)) {
            return EMPTY_ENTRY;
        } else {
            AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            //看这里返回了一个配置的字符串集合
            List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
            configurations = this.removeDuplicates(configurations);
            Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
            this.checkExcludedClasses(configurations, exclusions);
            configurations.removeAll(exclusions);
            configurations = this.getConfigurationClassFilter().filter(configurations);
            this.fireAutoConfigurationImportEvents(configurations, exclusions);
            return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
        }
    }
    ...
       protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
        //看这个SpringFactoriesLoader.loadFactoryNames
        List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
        Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
        return configurations;
    }
}
```

```java
package org.springframework.core.io.support;
public final class SpringFactoriesLoader {
    ...
        public static List<String> loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader) {
        ClassLoader classLoaderToUse = classLoader;
        if (classLoader == null) {
            classLoaderToUse = SpringFactoriesLoader.class.getClassLoader();
        }

        String factoryTypeName = factoryType.getName();
        return (List)loadSpringFactories(classLoaderToUse).getOrDefault(factoryTypeName, Collections.emptyList());
        //这个loadSpringFactories(classLoaderToUse)是一个Map<String, List<String>>
    }
    
    private static Map<String, List<String>> loadSpringFactories(ClassLoader classLoader) {
        Map<String, List<String>> result = (Map)cache.get(classLoader);
        if (result != null) {
            return result;
        } else {
            HashMap result = new HashMap();
				 //加载这个文件，最终响应一个 Map<String, List<String>>
            try {//我们最终需要的EnableAutoConfiguration这个key对应的字符串list
                Enumeration urls = classLoader.getResources("META-INF/spring.factories");
                ...
            }
        }
    }
}
```



### 4.2.3 自动配置类

JavaConfig风格去写的配置类

 

@ConditionalOnXXX 当我满足XXX条件的时候生效

@ConditionalOnMissingXXX 当满足不包含XXX条件的时候生效

 **非常重要**

```properties
# .../spring-boot-autoconfigure-2.5.0.jar!/META-INF/spring.factories
# Auto Configure
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
```

```java
package org.springframework.boot.autoconfigure.jdbc;
@Configuration(
    proxyBeanMethods = false
)
//此注解要求当你包含一些类的时候生效→当你引入一些依赖的时候→通常是starter依赖
@ConditionalOnClass({JdbcTemplate.class, TransactionManager.class})
@AutoConfigureOrder(2147483647)
@EnableConfigurationProperties({DataSourceProperties.class})
public class DataSourceTransactionManagerAutoConfiguration {
    public DataSourceTransactionManagerAutoConfiguration() {
    }
    
    @Configuration(
        proxyBeanMethods = false
    )
    //此注解要求容器中要注册一个DataSource组件
    @ConditionalOnSingleCandidate(DataSource.class)
    static class JdbcTransactionManagerConfiguration {
        JdbcTransactionManagerConfiguration() {
        }

        @Bean
        //此注解：当容器中没有这个组件的时候生效→我们自己没有注册的时候→生效后做了什么事情→注册了一个默认的组件
        @ConditionalOnMissingBean({TransactionManager.class})
        DataSourceTransactionManager transactionManager(Environment environment, DataSource dataSource, ObjectProvider<TransactionManagerCustomizers> transactionManagerCustomizers) {
            DataSourceTransactionManager transactionManager = this.createTransactionManager(environment, dataSource);
            transactionManagerCustomizers.ifAvailable((customizers) -> {
                customizers.customize(transactionManager);
            });//当我们没有注册组件的时候→注册一个默认组件，当我们自己注册，就不提供默认组件了
            return transactionManager;
        }

        private DataSourceTransactionManager createTransactionManager(Environment environment, DataSource dataSource) {
            return (DataSourceTransactionManager)((Boolean)environment.getProperty("spring.dao.exceptiontranslation.enabled", Boolean.class, Boolean.TRUE) ? new JdbcTransactionManager(dataSource) : new DataSourceTransactionManager(dataSource));
        }
    }
}
```



### 4.2.4 web的自动配置类

```properties
# .../spring-boot-autoconfigure-2.5.0.jar!/META-INF/spring.factories
# Auto Configure
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
```

```java
package org.springframework.boot.autoconfigure.web.servlet;
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
@ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})
@ConditionalOnMissingBean({WebMvcConfigurationSupport.class})
@AutoConfigureOrder(-2147483638)
@AutoConfigureAfter({DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class})
public class WebMvcAutoConfiguration {
    ...
    @Configuration(
        proxyBeanMethods = false
    )
    @Import({WebMvcAutoConfiguration.EnableWebMvcConfiguration.class})
        //这里引入了三个参数类
    @EnableConfigurationProperties({WebMvcProperties.class, ResourceProperties.class, WebProperties.class})
    @Order(0)
    public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {
        private static final Log logger = LogFactory.getLog(WebMvcConfigurer.class);
        private final Resources resourceProperties;
        private final WebMvcProperties mvcProperties;
        private final ListableBeanFactory beanFactory;
        private final ObjectProvider<HttpMessageConverters> messageConvertersProvider;
        private final ObjectProvider<DispatcherServletPath> dispatcherServletPath;
        private final ObjectProvider<ServletRegistrationBean<?>> servletRegistrations;
        private final WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer resourceHandlerRegistrationCustomizer;
        private ServletContext servletContext;
		//这里使用了引入的三个参数类
        public WebMvcAutoConfigurationAdapter(ResourceProperties resourceProperties, WebProperties webProperties, WebMvcProperties mvcProperties, ListableBeanFactory beanFactory, ObjectProvider<HttpMessageConverters> messageConvertersProvider, ObjectProvider<WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider, ObjectProvider<DispatcherServletPath> dispatcherServletPath, ObjectProvider<ServletRegistrationBean<?>> servletRegistrations) {
            this.resourceProperties = (Resources)(resourceProperties.hasBeenCustomized() ? resourceProperties : webProperties.getResources());
            this.mvcProperties = mvcProperties;
            this.beanFactory = beanFactory;
            this.messageConvertersProvider = messageConvertersProvider;
            this.resourceHandlerRegistrationCustomizer = (WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer)resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
            this.dispatcherServletPath = dispatcherServletPath;
            this.servletRegistrations = servletRegistrations;
            this.mvcProperties.checkConfiguration();
        }
        ...
    }
    ...
        //这个关于注册Formatters的方法能联想到SpringMVC注册日期Converter
    public void addFormatters(FormatterRegistry registry) {
        //关注这里调用了addBeans方法 做的事
        ApplicationConversionService.addBeans(registry, this.beanFactory);
    }
    //这个方法做静态资源映射配置
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        if (!this.resourceProperties.isAddMappings()) {
            logger.debug("Default resource handling disabled");
        } else {
            this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
            //getStaticPathPattern()相当于xml或SpringMvcConfiguration配置类时配置的mapping
            //addResourceLocations相当于xml或SpringMvcConfiguration配置类时配置的Location
            this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
                registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                if (this.servletContext != null) {
                    ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                    registration.addResourceLocations(new Resource[]{resource});
                }

            });
        }
    }
    ...
}
```

```java
package org.springframework.boot.convert;
public class ApplicationConversionService extends FormattingConversionService {
    ...
     public static void addBeans(FormatterRegistry registry, ListableBeanFactory beanFactory) {
        Set<Object> beans = new LinkedHashSet();
        beans.addAll(beanFactory.getBeansOfType(GenericConverter.class).values());
        //这里找到所有的Converter组件，添加到set里
        beans.addAll(beanFactory.getBeansOfType(Converter.class).values());
        beans.addAll(beanFactory.getBeansOfType(Printer.class).values());
        beans.addAll(beanFactory.getBeansOfType(Parser.class).values());
        Iterator var3 = beans.iterator();
		//这里用iterator取做遍历
        while(var3.hasNext()) {
            Object bean = var3.next();
            if (bean instanceof GenericConverter) {
                registry.addConverter((GenericConverter)bean);
            } else if (bean instanceof Converter) {
                //如果是Converter类型的组件 →  registry.addConverter
                registry.addConverter((Converter)bean);
            } else if (bean instanceof Formatter) {
                registry.addFormatter((Formatter)bean);
            } else if (bean instanceof Printer) {
                registry.addPrinter((Printer)bean);
            } else if (bean instanceof Parser) {
                registry.addParser((Parser)bean);
            }
        }

    }
}
```

由此得出：

SpringBoot中要使用Converter，只需要注册到容器中即可

 

静态资源映射的配置

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image070-1622638493081.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image072-1622638493081.jpg)

### 4.2.5 springboot的默认配置

**/META-INF/(xxx-)spring-configuration-metadata.json**

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image074-1622638493081.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image076-1622638493081.jpg)

# 5    SpringBoot的配置文件

## 5.1   tomcat相关的配置

端口号: **server.port**

context-path: 上下文路径 **server.servlet.context-path**

## 5.2   配置文件的格式

application.properties

application-xxx.properties

 

application.yml

application-xxx.yml

## 5.3   y(a)ml文件的语法

表达的含义和properties是一样的 → key=value

 

properties 👉 yml

1、 遇到了点 👉 冒号、换行、空格缩进

2、 遇到等于 👉 冒号、一个空格

3、 同一级要对齐

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image078-1622638493081.jpg)

## 5.4   提供其他类型的值

### 5.4.1 properties语法

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image080-1622638493081.jpg)

### 5.4.2 yml语法

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image082-1622638493081.jpg)

## 5.5   多配置文件

引入多个配置文件

application-xxx.properties

application-xxx.yml

 

激活配置文件

主配置文件application.properties(yml) 👉 主配置文件中选择激活分配置文件

分配置文件application-xxx.properties(yml)

### 5.5.1 分流

将应用程序部署在不同的服务器alpha\beta\sigma

file.location=d:/alpha

file.location=d:/beta

file.location=d:/sigma

同一个配置项，放入到不同的配置文件中 👉 选择激活哪一个配置文件

选择

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image084-1622638493081.jpg)

### 5.5.2 解耦

不同的功能的配置放在不同的配置文件中

 

web👉 application-web.yml

datasource 👉 application-db.yml

rocketmq 👉 application-mq.yml

ydy 👉 application-ydy.yml

 

激活多个配置文件 👉 spring.profiles.active

### 5.5.3 yml可以表达多个配置文件

一个yml配置文件当多个配置文件用

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image086-1622638493081.jpg)

### 5.5.4 配置文件中的占位符

file.location=d:/stone/spring

file.jpg.location

file.png.location

file.xml.location

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image088-1622638493082.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image090-1622638493081.jpg)

## 5.6   web整合

spring-boot-starter-web

 

额外的配置：JavaConfig的配置类

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image092.jpg)





Interceptor还是按照springMVC的方法配置

SpringBoot没有编码问题，不用自己做编码设置

```properties
# application.properties 可以配，但是没必要，参见自动配置类下的json文件，里面已经配置了默认值了
spring.http.encoding.enabled=true
spring.http.encoding.force=true
spring.http.encoding.charset=UTF-8
```



## 5.7   mybatis

mybatis-spring-boot-starter

mysql-connector-java 5.1.47

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image094.jpg)

### 5.7.1 datasource

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image096.jpg)

### 5.7.2 mapper

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image098.jpg)

### 5.7.3 Mybatis的相关配置

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image100.jpg)

以mybatis作为前缀

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image102.jpg)

 

 

### 

# 附录

## packaging=war

Facets 为web配置到src\main\webapp

在pom.xml里添加`<packaging>`标签IDEA会自动把webapp目录变色同时生成Project Structure的Facets和Artifacts设置，并且在maven依赖发生变化时会自动重新打包Artifacts,项目会编译到target目录下artifact-version，自动在WEB-INF/lib下打包更新依赖。

而手动添加的artifact在maven依赖变化后不会自动打包artifact依赖，需要自己在Project Structure→Artifacts→Available Elements下的Artifacts下手动点击put新的依赖到lib目录中。

```
<!--pom.xml-->
<packaging>war</packaging>
```

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image036-1622211001074.jpg)

## postman

[Download Postman | Try Postman for Free](https://www.postman.com/downloads/)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image038-1622211001074.jpg)

![img](C:\Users\AnoxiC2010\Documents\GitHub\Hello-World\Java\Spring-notes.assets\clip_image040-1622211001074.jpg)

Cookie、Json请求





@Validated @Valid  BindingResult 必须放在校验参数后面，中间不能相隔其他参数

放在前面，报这个异常，中间有其他参数则校验失败也不会进入Handler方法

 java.lang.IllegalStateException: An Errors/BindingResult argument is expected to be declared immediately after the model attribute, the @RequestBody or the @RequestPart arguments to which they apply: public com.cskaoyan.bean.vo.BaseRespVo com.cskaoyan.controller.UserController.login(org.springframework.validation.BindingResult,com.cskaoyan.bean.LoginUserBO,javax.servlet.http.HttpSession)





mybatiss index binding 异常

 

 



 

 

