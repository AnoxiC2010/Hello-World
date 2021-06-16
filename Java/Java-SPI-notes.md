# SPI

修改配置文件即热更新

定义接口和实现类

```java
//全类名 com.cskaoyan.spi.Pay
public interface Pay {
    void pay(double amount);
}
//全类名 com.cskaoyan.spi.AliPay
public class AliPay implements Pay {
    @Override
    public void pay(double amount) {
        System.out.println("阿里支付 " + amount);
    }
}
//全类名 com.cskaoyan.spi.WeChatPay
public class WeChatPay implements Pay {
    @Override
    public void pay(double amount) {
        System.out.println("微信支付 " + amount);
    }
}
```

配置文件

/resources/META-INF/Services包下

创建文本文件，名称为接口全类名 com.cskaoyan.spi.Pay

文件内容为具体的实现类比如 com.cskaoyan.spi.WeChatPay



测试运行

```java
public class PayTest {
    public static void main(String[] args) {
        ServiceLoader<Pay> load = ServiceLoader.load(Pay.class);
        //这样
        load.forEach(service -> service.pay(100));
        //或这样
        Iterator<Pay> iterator = load.iterator();
        while (iterator.hasNext()) {
            iterator.next().pay(100);
        }
    }
}
```



