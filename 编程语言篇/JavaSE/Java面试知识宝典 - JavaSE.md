### 第一章 内容介绍

无论是在学习，还是在面试的时候，我们难免会遇到这样的问题：xxx的底层原理、xxx是怎么实现的、除了如此，还能怎样？诸如此类，很多时候我们会去各种贴吧、博客、技术论坛、甚至是花钱去买一些面试心得与题目，去寻找、去背诵。

这不是一个好方法！

所以，我们整理了此份《知识宝典》，不仅仅是为了自己能够实时快速的巩固学习，也是为了和我们一样困惑的朋友解惑。

本篇知识点的整合是按照面试官的提问套路顺序来整理，往往是通过一个知识点来引出层层知识点提问。

经历一场，仿佛大战了一场！相信你们会有收获的

### 第二章 JavaSE基础

- Java语言的特点
- JDK、JVM、JRE的关系是什么，全称是什么，作用是什么
  - 什么是字节码？使用字节码的好处？Java跨平台的原理？字节码怎么生成的？
- Oracle JDK 和OpenJDK的对比
- Java和其他语言的对比
- 什么是Java程序的主类
- 为什么说Java语言”编译和解释并存“
- 字符型常量和字符串常量的区别
  - 字符串类型的底层是什么？
  - String类型可以继承吗？
  - `String s = "hello",s = s+"world";` 这段代码执行之后，原始的String类型变类没有？为什么？
  - 请你谈一谈final关键字？
  - String、StringBuffer、StringBuilder的联系和区别
    - 什么时候用“+”元算符进行字符串连接比调用“StringBuilder/StringBuffer”的append方法连接字符串性能更好
- 了解自增和自减吗？
  - i++是线程安全的吗？为什么?
  - 如何实现线程安全？还有没有更好的方法？
  - 了解Java中的原子类吗？你在工作中或者项目中用到过原子类吗？
    - 原子类是怎么实现的？
    - 如果你自己实现原子类？你会怎么做？
    - 知道CAS吗？
    - CAS的底层原理是什么？
    - 谈谈你对UnSafe的理解
    - CAS的缺点
    - 为啥AtomicInteger使用CAS而不是synchronized
    - 知道ABA问题吗？
      - 怎么规避ABA问题
    - 原子引用更新的原理是什么
  - 那你知道JMM吗？
    - JMM的规定是什么？
  - 那你知道volatile关键字吗？
    - volatile关键字有哪些特性
    - volatile和synchronized的区别知道吗？
    - 知道单例设计模式吗？
      - 如何设计单例模式，你有几种方法？
      - 单理模式中的饿汉式一定是线程安全的吗？怎么解决
    - 你使用过volatile关键字吗？
      - 为什么要使用volatile关键字
    - 知道volatile关键字的底层实现吗？
- continue、break和return的区别是是什么？
- Java泛型知道吗？什么是类型擦除？介绍一下常用的通配符？
- == 和 equals的区别
- hashCode()和equals()
  - hashCode()方法介绍
  - 为什么要有hashCode()
  - 为什么重写hashCode()必须要重写equals()方法
  - 为什么两个对象有相同的hashCode值，他们也不一定是相等的
- Java中的基本数据类型？对应的包装类？为什么要有包装类？占用多少字节？
  - 了解自动拆箱和装箱机制吗？
  - `short s1 = 1;s1 = s1+1;`有错吗？为什么？`s1+=1`有错吗？为什么？
  - 知道常量池吗？
    - 为什么需要常量池？
    - 常量池的设计模式知道吗？
- 什么是方法的返回值？返回值在类的方法的作用是什么？
  - 为什么Java中只有值传递
  - 重载和重写的区别
  - 为什么函数不能根据返回值类型来区分重载
  - 深拷贝和重写的区别
  - 方法的四种类型
  - 当一个对象作为参数传递到一个方法之后，此方法可以改变对象的属性？并返回变化后的结果，那么这里到底是值传递还是引用传递？请画图说明原理！
- 面向对象
  - 面向对象有哪些特征以及你对这些特征的理解
  - 访问权限修饰符 public、private、protected、以及不写（默认）的区别
  - 如何理解clone对象
    - 为什么需要用clone
    - new一个对象的过程和clone一个对象的过程
    - clone对象的使用
    - 深拷贝和浅拷贝，请谈一谈实现的方法
  - 面向对象和面向过程的区别
  - 构造器Contractor是否可以被Override？
  - 在Java中定义一个不做事且没有参数的构造方法的作用
  - 成员变量和局部变量的区别有哪些？
  - 如何创建对象？对象实体和对象引用有何区别？
  - 一个类的构造方法的作用是什么？如何一个类没有声明构造方法，该程序能正确执行吗？
  - Java构造方法的特征
  - 对象的相等和指向他们的引用相等，二者有何不同？
  - 在一个静态方法内部调用一个非静态成员为什么是非法的？
  - 静态方法和实例方法有何不同？
  - 常见关键字：static、final、this、super
  - 接口和抽象类的区别
  - 你知道Java中所有类的父类吗？
    - Object中有哪些方法？
  - 知道Java的序列化吗？序列化的作用是什么？如果有不想序列化，怎么办？
  - 获取键盘输入常用的两种方法
  - 构造方法有哪些特征
  - 静态方法和实例方法的不同
  - 在调用子类构造方法之前会先调用父类无参构造方法，为什么？
- 为什么Java只有值传递？
  - 你能画出Java值传递和值修改的原理图吗？
- 简述线程、程序、进程的基本概念。他们之间的关系是什么？
- 线程有哪些基本状态？线程状态转换鸽子调用了哪些方法，可以画图展示一下吗？
- Java中有没有goto语句？
  - Java常见的关键字和保留字是什么
- & 和 &&的区别
- Java中如何跳出多重嵌套循环
- char类型能不能存储一个中文汉字，为什么？
- 抽象的（abstract）方法是否可以同时是静态的（static），是否可同时是本地方法（native），是否可同时被synchronized
- switch可以作用的类型有哪些？分别是从哪个JDK版本开始支持的？

### 第三章 JavaSE 常用API库 (给出案例代码)

- String的常用方法你知道吗？
- StringBuilder、StringBuffer的常见方法？
- Math库的常用方法你知道吗？
- Date时间库的使用？日期类
- Scanner类
- Arrays类
- Random类
- Class类
- Object类
- 正则表表达式
- System类
- Collections类

### 第四章 JavaSE 高级 之 IO (画图和案例代码结合)

- Java中有几种类型的流？
- 请画出字符流和字节流的继承体系
- 字节流如何转为字符流
- 如何将一个Java对象序列化到文件里
- 字节流和字符流的区别
- 如何实现对象的克隆
- 什么是Java序列化，如何实现Java序列化
- 既然有了字节流，为什么还要有字符流？
- BIO、NIO、AIO有什么区别
  - BIO
  - NIO
  - AIO

### 第五章 JavaSE 高级 之 异常

- 请画出异常的继承体系图
- Error错误和Exception
- Throwable类常用的方法有哪些？
- try-catch-finally 的使用
  - JDK7之后 try-catch-finally的新特性是什么？
  - 在哪些情况下,finally不会执行？
- 请说出常见的ＯＯＭ异常
  - 在什么情况下会发生这样的异常？
- 请说出你在学习或者开发中遇到的异常
- 如何自定义异常
- 异常的作用是什么

### 第六章 JavaSE 高级 之 集合

- 请画出集合框架的继承体系图
- 说说List、Set、Map三者的区别？
- ArrayList和LinkedList的区别？
- 你知道RandomAccess接口吗？他的作用是什么？
- 你了解双向链表和双向循环链表吗？
- 集合的安全性问题？
  - 请问ArrayList、HashSet、HashMap是线程安全的吗？如果不是线程安全的集合怎么办？
- ArrayList底层怎么实现的？
  - 在什么样的场景下使用ArrayList
  - 它是线程安全的吗？为什么？
  - 如果我想使用线程安全的ArrayList，应该怎么做？
  - ArrayList的常用方法有哪些？
  - ArrayList的扩容机制是怎样的？
- 知道ArrayList和Vector的区别吗？为什么要用ArrayList取代Vector呢？
- HashMap和Hashtable的区别？
  - 知道HashMap的扩容机制吗？
  - 你知道HashMap的底层数据结构吗？不同版本的数据结构知道吗？
  - 为什么要用这样的结构实现？
  - 为什么HashMap总是使用2的幂作为哈希表的大小？
  - 知道扰动函数吗？作用是什么？
  - HashMap的长度为什么是2的幂次方？
- HashSet如何检查重复？
  - HashSet的底层原理是什么？
- HashMap多线程操作死循环问题？
- ConcurrentHashMap和Hashtable的区别
  - ConcurrentHashMap是否有升级？如果有，你知道升级的原理吗？
  - ConcurrentHashMap底层如何实现的？请结合自己画图分析
- comparable和Comparator的区别
- Comparator定制重排序？请给出排序示例代码
  - compareTo方法
- 请写出集合框架底层数据结构总结以及1其操作的特点
  - List
    - ArrayList
    - Vector
    - LinkedList
  - Set
    - HashSet
    - LinkedHashSet
    - TreeSet
  - Map
    - HashMap
    - LinkedHash Map
    - Hashtable
    - TreeMap
- 如何选用集合
- 并发集合和普通集合了解吗？
- 数组和链表使用的常见了解吗？
- Map中的key和value可以为null吗？
- Collections的常用API方法

### 第七章 JavaSE 高级 之 多线程

- 简述线程、程序、进程的基本概念，以及他们的关系是什么？以及各自的优缺点？
- 说说并行与并发的区别？
  - 你的项目中有没有使用到并发？
- 为什么需要使用线程？
- 使用多线程可能带来的问题有哪些？
- 什么是上下文切换？
- 线程的基本状态以及状态图？请写出对应的Object方法

- 多线程有多少种实现方式？
  - 最常用的是那种？
  - Runnable和Callable接口的区别？
  - 为什么需要使用线程池
  - 执行execute()方法和submit()方法的区别是什么？
  - 知道线程池的实现原理吗？
  - JDK提供了哪些创建线程池的方法
  - 在实际工作中你是怎么使用线程池的？如何定义线程池的核心线程大小？
  - 知道ThreadPoolExecutor类吗？重要的参数知道吗？(给出代码实例)
    - ThreadPoolExecutor的饱和策略知道吗？
    - 能够手写一个线程池吗？
  - 生产上你如何设置合理线程池参数？
  - 知道线程池的启动策略吗？
  - 如何控制某个方法允许并发访问线程的个数？
  - 三个线程a、b、c并发允许，b、c需要a的线程是怎么实现的？
- 公平锁/非公平锁/可重入锁/递归锁/自旋锁谈谈你的理解？请手写一个自旋锁
- CountDownLatch/CyclicBarrier/Semaphore 使用过吗？

- 什么是线程死锁？如何避免死锁？请写出一个死锁的案例
- 知道阻塞队列吗？
  - 为什么用？有什么好处？
  - BlockingQueue的核心方法知道吗？
  - 阻塞队列的应用知道吗？
  - 你知道哪些阻塞队列？都是怎么实现的？
- 说说sleep()方法和wait()方法的区别和共同点
- 为什么调用start()方法会执行run()方法。为啥不直接调用run()方法
- synchronized 关键字
  - 自己使用到了synchronized了嘛？
  - synchronized 的三种使用方式知道嘛？
  - 知道 synchronized 的底层原理吗？
  - 说一下JDK1.6之后对synchronized 关键字底层做了哪些优化？可以介绍一下这些优化吗？
  - 除了synchronized ，你了解ReentrantLock的吗？
  - synchronized 和 ReentrantLock的区别你知道吗？
  - 你知道可重入锁和不可重入锁吗
- volatile关键字的理解
  - JMM内存模型了解吗？
  - 并发编程的三个重要特性volatile满足了哪些？
  - volatile和synchronized的区别你知道吗？
- 你知道 ThreadLocal吗？
  - 你能否给出ThreadLocal的案例代码吗？
  - 你知道ThreadLocal的底层原理吗？
  - 了解ThreadLocal的内存泄露问题吗？
- 你有没有使用jps和jstack定位死锁？请给出具体的截图流程和文字说明
- 知道Atomic原子类吗？请介绍一下Atomic原子类
- JUC包中的原子类是哪四类？
  - 讲讲各自原子类的使用？
  - 能不能讲一下AtomicInteger的原理？
- 知道AQS吗？
  - AQS原理是什么？
  - AQS对资源的共享方式
  - 请整理一下AQS组件总结
- 同一个类中的2个方法都加了同步锁，多个线程能同时访问同一个类中的这两个方法吗？
- 说出同步线程个线程调度相关的方法