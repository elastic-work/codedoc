### JavaSE 常用API库 

#### String类

- 知道String拼接的底层实现吗？为什么String比StringBuilder和StringBuffer慢？

```markdown
通过对字节码反编译我们可以看到，String的底层拼接是使用的StringBuilder
但是在JDK5之前使用的是StringBuffer实现
在JDK5之后使用的是StringBuilder实现
每次拼接对象都要对数据进行转换成Stirng对象，因此方法慢了
```

#### StringBuilder、StringBuffer

- 哪个是线程不安全的？为什么？

```markdown
StringBuilder 是线程不安全的
StringBuffer 是线程安全的 其方法使用了 synchronized 修饰
```

#### Math库的常用方法你知道吗？

- Math.random(11.5)和Math.random(-11.5) 的值是多少？
  - Math.random(11.5) = 12
  - Math.random(-11.5) = -11 
  - 方法：+0.5 然后再四舍五入
- Math.random() 实际上调用的是 Random类的nextDouble()方法

#### Arrays类

- 封装了操作数组的方法，里面含有大量的重载方法

#### Random类

- 随机数生成类，核心底层的方法next()方法，在Random类中也使用了高斯分布，来保证得到的随机数是均匀随机数

#### Class类

- 里面包含很多关于反射的方法，
- 通过Class类的对象，可以得到当前类的所有方法、属性、构造器等信息

#### Object类

- 地位：所有类的直接父类或者间接父类
- 接口不是Object的子类
- 比较重要的方法
  - wait
  - toString
  - notify
  - equals

#### System类

- System.out.println() 的底层实现
- 在System中有一个 静态对象 InputStream in; OutputStream out;实际上就是调用的out对象的方法

#### Collections类

- 在Collections类中包含了大量的内部类，是为了弥补常用的集合框架如ArrayList、LinkedList、HashMap、HashSet等线程不安全的工具类，其中提供了所有的不安全的集合框架的安全实现，使用 synchroinized 修饰的方法