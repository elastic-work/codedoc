### JDK - 新特性 - Java4

**写这么早的版本我也早考虑要不要加入进来，仔细一想，请知其所以然**

#### 新特性列表

- XML
- Logging API
- 断言
- Preferences API
- 链式异常处理
- 支持IPV6
- 支持正则表达式
- 引入 Image I/O API
- NIO

#### XML处理

-  针对XML处理的JavaTM API 已经被添加到Java 2平台。它通过一套标准的Java平台API提供对XML的基本处理的支持 

#### Logging API

-  解释：Logging API为程序提供了一种报告其行为的机制。它提供了一种在现场部署应用程序后打开和关闭日志消息的方法，极大地帮助了应用程序的维护。 、
- 推荐阅读： https://www.cnblogs.com/liaojie970/p/5582147.html 

#### 断言

- 解释：它是用于对程序进行调试的，对于执行结构的判断，而不是对业务流程的判断。可以理解为一个if()语句，满足条件才会执行，不满足就直接报错
- 语法：**assert condition**  或者 **assert condition : "提示信息"**  这里condition是一个必须为真(true)的表达式。如果表达式的结果为true，那么断言为真，则不会有任何行动；如果表达式为false，则断言失败，这时会抛出一个AssertionError。–asser condition:expr这里condition是一个必须为真(true)的表达式。冒号后跟的是一个表达式，通常用于断言失败后的提示信息，简而言之是一个传到AssertionError构造函数的值，如果断言失败，该值被转化为它对应的字符串，并显示出来。 
- 例子：**注意(JDK中默认是不开启断言的，需要设置一下VM参数：-enableassertions )**

```java
public class TestAssert {
    public static void main(String[] args) {
        int x = 10;
        System.out.println("Testing Assertion that x==100");
        assert x == 100 : "Out assertion failed!";
        System.out.println("Test passed!");
    }
}

// 报错信息如下
Testing Assertion that x==100
Exception in thread "main" java.lang.AssertionError: Out assertion failed!
	at cn.icanci.day0808.TestAssert.main(TestAssert.java:14)
```

#### Preferences API

- 解释：用于将首选项存储到特定于操作系统的后端。在Windows等操作系统上，首选项存储在操作系统级别的注册表中，对于非Windows环境，它们可以存储在其他注册表类存储中，也可以存储在简单的XML文件中 
- 例子

```java
public class TestAssert {
    public static void main(String[] args) {
        Preferences root = Preferences.userRoot();
        root.putInt("age", 10);
        // 这里的1是默认值，当没有获得age的值会返回它
        int fontSize = root.getInt("age", 1);
        System.out.println(fontSize);
    }
}

```

#### 链式异常处理

-  解释：链式异常允许将一个异常与另一个异常联系起来，即一个异常描述了另一个异常的原因。例如，考虑一种情况，即由于试图除以零而导致抛出ArithmeticException，但实际的异常原因是导致除数为零的I / O错误。该方法只会向调用者抛出ArithmeticException。所以调用者不会知道异常的真正原因 
- 例子

```java
public class ExceptionHandling {
    public static void main(String[] args) {
        try {
            //创建一个错误
            NumberFormatException ex =
                    new NumberFormatException("Exception");
            //设置错误的触发原因
            ex.initCause(new NullPointerException(
                    "This is actual cause of the exception"));
            //抛出错误并指明原因
            throw ex;
        } catch (NumberFormatException ex) {
            //在控制台打印错误
            System.out.println(ex);
            //获得错误的触发原因
            System.out.println(ex.getCause());
        }
    }
}
```

#### 支持IPV6

-  解释：JDK 1.4开始支持 Linux 和Solaris 平台上的 IPv6（JDK 1.5起加入了 Windows 平台上的支持） 

#### 支持正则表达式

- 推荐教程： https://www.runoob.com/java/java-regular-expressions.html 

#### 引入Imgae I/O API

- 解释：提供了一组用于操作存在本地文件的或者通过网络传输的图片的可插入式架构。它较之前的API在读取和保存图片方面总体上来看要更加灵活和强大。 
- 推荐阅读： https://www.jianshu.com/p/22bcb11109d0 

#### NIO

- 推荐阅读： https://blog.csdn.net/u011381576/article/details/79876754 