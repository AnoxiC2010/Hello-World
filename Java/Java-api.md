# Integer类

## static int parseInt(String s,  int radix)

```
parseInt
public static int parseInt(String s,
                           int radix)
                    throws NumberFormatException使用第二个参数指定的基数，将字符串参数解析为有符号的整数。除了第一个字符可以是用来表示负值的 ASCII 减号 '-' ('\u002D’)外，字符串中的字符必须都是指定基数的数字（通过 Character.digit(char, int) 是否返回一个负值确定）。返回得到的整数值。 
如果发生以下任意一种情况，则抛出一个 NumberFormatException 类型的异常： 

第一个参数为 null 或一个长度为零的字符串。 
基数小于 Character.MIN_RADIX 或者大于 Character.MAX_RADIX。 
假如字符串的长度超过 1，那么除了第一个字符可以是减号 '-' ('u002D’) 外，字符串中存在任意不是由指定基数的数字表示的字符。 
字符串表示的值不是 int 类型的值。 
示例： 

parseInt("0", 10) 返回 0
parseInt("473", 10) 返回 473
parseInt("-0", 10) 返回 0
parseInt("-FF", 16) 返回 -255
parseInt("1100110", 2) 返回 102
parseInt("2147483647", 10) 返回 2147483647
parseInt("-2147483648", 10) 返回 -2147483648
parseInt("2147483648", 10) 抛出 NumberFormatException
parseInt("99", 8) 抛出 NumberFormatException
parseInt("Kona", 10) 抛出 NumberFormatException
parseInt("Kona", 27) 返回 411787

参数：
s - 包含要解析的整数表示形式的 String
radix - 解析 s 时使用的基数。 
返回：
使用指定基数的字符串参数表示的整数。 
抛出： 
NumberFormatException - 如果 String 不包含可解析的 int。
```



static Intege decode(String nm) 

```
decode
public static Integer decode(String nm)
                      throws NumberFormatException将 String 解码为 Integer。接受通过以下语法给出的十进制、十六进制和八进制数字： 
DecodableString: 
Signopt DecimalNumeral 
Signopt 0x HexDigits 
Signopt 0X HexDigits 
Signopt # HexDigits 
Signopt 0 OctalDigits 

Sign: 
- 
Java Language Specification 的第 §3.10.1 节中有 DecimalNumeral、HexDigits 和 OctalDigits 的定义。 
跟在（可选）负号和/或基数说明符（“0x”、“0X”、“#”或前导零）后面的字符序列是使用指示的基数（10、16 或 8）通过 Integer.parseInt 方法解析的。字符序列必须表示一个正值，否则会抛出 NumberFormatException。如果指定的 String 的第一个字符是减号，则对结果求反。String 中不允许出现空白字符。 


参数：
nm - 要解码的 String。 
返回：
保存 nm 所表示的 int 值的 Integer 对象。 
抛出： 
NumberFormatException - 如果 String 不包含可解析整数。
从以下版本开始： 
1.2 
另请参见：
parseInt(java.lang.String, int)

```





# ScriptEngineManager

计算表达式

使用Js脚本

```java
//[]{}要转化为()才能使用
@Test
public void test04() throws ScriptException {
    ScriptEngineManager em = new ScriptEngineManager();
    ScriptEngine javaScript = em.getEngineByName("JavaScript");
    String ep1 = "3*(5*(6-(1+2)))";
    String ep2 = "3*1+4";
    Object eval = javaScript.eval(ep1);
    System.out.println(eval);
}
```

也可使使用中缀表达式转化为后缀表达式，在通过栈计算后缀表达式

e.g.

```

```



# BitSet

[Java Bitset类 | 菜鸟教程 (runoob.com)](https://www.runoob.com/java/java-bitset-class.html)

凡是涉及到去重统计都可以用位图实现。因为每一个不同的数据只需要用二进制的一位存储即可，大大减小了统计所使用的存储空间

```
import java.util.Scanner;
import java.util.BitSet;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String line = scanner.next();
        //总共有128个字符。字需要用128位
        BitSet bitSet = new BitSet(128);
        for (char c : line.toCharArray()) {
            //判断字符c是否已出现
            if (!bitSet.get(c)) {
                //未出现就设置为已出现
                bitSet.set(c);
            }
        }
        //统计有多少字符已出现过
        System.out.println(bitSet.cardinality());
    }
}
```



# CountDownLatch

[多线程_牛客题霸_牛客网 (nowcoder.com)](https://www.nowcoder.com/practice/cd99fbc6154d4074b4da0e74224a1582?tpId=37&&tqId=21272&rp=1&ru=/ta/huawei&qru=/ta/huawei/question-ranking)





# AtomicInteger

