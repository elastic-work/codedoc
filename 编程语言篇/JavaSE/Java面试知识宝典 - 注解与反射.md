### 第一章 注解

- 什么是注解？注解的作用是什么
  
  ```markdown
  注解是JDK5.0 之后的新特性
  注解是一种描述元数据的数据 - 更加通俗的理解就是：注解是标签 定义不同的便签就具体不同的功能，但是标签的功能是人为赋予的
  
  在Annotation出现之前，工程师发现在项目中使用大量的XML来描述元数据
  
  假如你想为应用设置很多的常量或参数，这种情况下，XML是一个很好的选择，因为它不会同特定的代码相连。
  如果你想把某个方法声明为服务，那么使用Annotation会更好一些，
  因为这种情况下需要注解和方法紧密耦合起来，开发人员也必须认识到这点。
  ```
  
  - 注解的格式是什么
  
  ```java
  // 注解的格式很简单 一个@符号后面加一个注解的名称 
  // 如 @Override @Value等
  ```
  
  - 注解在哪里使用
  
  ```markdown
  注解可以使用在在类、方法、参数 、变量、构造器、以及包声明中的特殊修饰符
  它是JSR-175标准选择用来描述元数据的工具
  
  在我们开发学习的前期，我们会发现很多地方掺杂着XML和Annotation
  但是在后期学习到SpringBoot和SpringCloud的时候，已经见不到XML的痕迹了
  ```
  
- 注解有几个元注解
  
  - 分别是哪些元注解
  
  ```java
  // Java 中一共有四种元注解 
  // 在java.lang.annotation 包中提供了4种元注解
  
  // 注解是否包含在JavaDoc文档中
  @Document
  // 什么时候使用该注解
  @Retention
  // 注解用于什么地方
  @Target
  // 是否允许子类继承该注解
  @Inherited
  ```
  
- 如何自定义注解？

  - 使用**@interface**自定义注解的时候，自动继承了 java.lang.annotation.Annotataion 接口

  - 分析

    - @interface用来声明一个注解 格式 `public @interface MyAnno {}`
    - 其中的每一个方法实际上就是声明了一个配置参数
    - 方法的名称就是参数的名称
    - 返回值的类型就是参数的类型（返回值类型只能是基本类型，Class、String、enum）
    - 可以使用default来声明参数的默认值
    - 如果只有一个参数成员，一般参数名为 value
    - 注解元素必须要有值，我们定义注解元素的时候， 经常使用空字符串，0作为默认值

    ```java
    @Target({ElementType.TYPE, ElementType.METHOD})
    @Retention(RetentionPolicy.RUNTIME)
    public @interface MyAnno2 {
        // 注解的参数： 参数类型+参数名();
        String name() default "";
    
        int age() default 0;
    
        // 如果默认为 -1 ，就是不存在
        int id() default -1;
    
        String[] schools() default {};
    }
    ```

    ```java
    public class Test02 {
        // 注解可以显示赋值，也可以使用默认值
    
        @MyAnno2(name = "ic", age = 12)
        public static void main(String[] args) {
    
        }
    }
    
    ```

  - 如果注解只有一个内容，那么就只要 写 value，就可以直接放值，如果是多共个，value 就必须使用

### 第二章 反射

- 什么是反射

```markdown
主要是指程序可以访问、检测和修改它本身状态或行为的一种能力
Reflection（反射）是Java被视为动态语言的关键，
反射机制允许程序在执行期间借助于Reflection API 取得任何类的内部信息
并能直接操作任意对象的内部属性和方法
```

- 了解静态语言和动态语言吗？

```markdown
动态语言
是一类可以在运行时改变其结构的语言 如：Object-c、C#、JavaScript、PHP、Python

静态语言
与动态语言对应的，运行时结构不可以改变的语言，如 Java、C、C++
Java不是动态语言，但是Java可以称之为"准动态语言"，也就是Java有一定的动态性
```

- 反射的优点和缺点是什么？

```markdown
优点：
可以实现动态创建对象和编译，体现出很大的灵活性

缺点：
反射破坏了封装，因为就算是私有的构造、属性和方法都可以通过反射获取
对性能有一定的影响，使用反射基本上就是一种解释操作
我们可以高速JVM，我们希望做什么并且希望它能满足我们的需求
这类操作总是慢于直接执行相同的操作
```

- 反射获取Class对象的几种方式了解吗？

```markdown
三种方式

第一种：
父类Object中有一个 getClass() 方法，所有的类都可以通过此获取反射对象
Object o = new Object();
Class c1 = o.getClass();
第二种：
任何数据类型（包括基本数据类型），都包有一个静态的 class 属性
Class c1 = int.class;
第三种：
通过Class类的静态方法 Class.forName("类的全限定名");
Class c1 = Class.forName("java.lang.String");
```

- Class类常用的方法知道吗？

```
java.lang.Class 代表一个类
java.lang.reflect.Method 代表类的方法
java.lang.reflect.Field 代表类的成员变量
java.lang.reflect.Constructor 代表类的构造器
```

| 方法名                                | 功能说明                                                  |
| ------------------------------------- | --------------------------------------------------------- |
| static class forName(String name)     | 返回指定类名name的Class对象                               |
| Object newInstance()                  | 调用缺省构造函数，返回一个Class对象的一个实例             |
| getName()                             | 返回此Class对象所表示的实体(类，接口，数组或者void)的名称 |
| Class getSuperClass()                 | 返回当前Class对象的父类的Class对象                        |
| Class[] getinterfaces()               | 返回当前Class对象的接口                                   |
| ClassLoader getClassLoader()          | 返回该类的类加载器                                        |
| Constructor[] getConstructors()       | 返回一个包含某些Constructor对象的数组                     |
| Method getMethod(String name,Class T) | 返回一个Method对象，此对象类型为paramType                 |
| Field[] getDeclareds()                | 返回Field对象的有一个数组                                 |

```java
// 理解Class类并获取Class实例
public class Test01 {
    public static void main(String[] args) throws Exception {
        // 通过反射获取Class对象
        Class<?> clazz = Class.forName("cn.icanci.reflection.User");
        Class<?> clazz2 = Class.forName("cn.icanci.reflection.User");
        Class<?> clazz3 = Class.forName("cn.icanci.reflection.User");
        // 一个类在内存中只有一个Class对象
        // 一个类被加载之后，类的整个结构都会被封装在Class对象中
        System.out.println(clazz == clazz2);
        System.out.println(clazz2 == clazz3);
        User user = (User) clazz.newInstance();
        user.setUsername("hello");
        System.out.println(user);
    }
}

class User {
    private String username;
    private int id;

    private int age;
	
    // 省略 getter 、 setter 、 toString ...
}

```

- 类的加载 ClassLoader

  - 类的加载 类的连接 类的初始化 因为类只加载一次，所以类只有一个对象

  ```java
  public class Test03 {
      static {
          System.out.println("main类被初始化");
      }
  
      public static void main(String[] args) throws Exception {
          // 会主动引用的案例
          // 创建对象
          // Son son = new Son();
          // 通过反射
          // Class<?> clazz = Class.forName("cn.icanci.reflection.Son");
          // 不回产生类的引用的方法,此时子类没有被初始化
          // System.out.println(Son.b);
          // 初始化数组也不回
          // Son[] sons = new Son[5];
          // 常量不回引起子类和父类的初始化
          System.out.println(Son.M);
      }
  }
  
  class Father {
      static int b = 2;
  
      static {
          System.out.println("父类被加载");
      }
  }
  
  class Son extends Father {
      static {
          System.out.println("子类被加载");
          m = 200;
      }
  
      static int m = 100;
  
      static final int M = 1;
  }
  
  ```

  ```java
  public class Test05 {
      public static void main(String[] args) throws Exception {
          // 获取系统的类加载器
          ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
          System.out.println(systemClassLoader);
          // 获取系统的类加载器 -> 拓展类加载器
          ClassLoader parent = systemClassLoader.getParent();
          System.out.println(parent);
          // 获取拓展类的加载器 父类加载器 -> 根加载器（C/C++）  根加载器 无法打印
          ClassLoader root = parent.getParent();
          System.out.println(root);
  
          // 测试当前类是哪个加载器加载的
          ClassLoader classLoaderTest05 = Class.forName("cn.icanci.reflection.Test05").getClassLoader();
          System.out.println(classLoaderTest05);
          // 测试JDK内部类是哪个加载器加载的 根加载器 无法打印
          ClassLoader classLoaderObject = Class.forName("java.lang.Object").getClassLoader();
          System.out.println(classLoaderObject);
  
          // 如何获取系统类加载器可以加载的路径
          System.out.println(System.getProperty("java.class.path"));
          /**
           * 哪些类被加载
           *
           * D:\jdk\jdk1.8\jre\lib\charsets.jar;
           * D:\jdk\jdk1.8\jre\lib\deploy.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\access-bridge-64.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\cldrdata.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\dnsns.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\jaccess.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\jfxrt.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\localedata.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\nashorn.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\sunec.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\sunjce_provider.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\sunmscapi.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\sunpkcs11.jar;
           * D:\jdk\jdk1.8\jre\lib\ext\zipfs.jar;
           * D:\jdk\jdk1.8\jre\lib\javaws.jar;
           * D:\jdk\jdk1.8\jre\lib\jce.jar;
           * D:\jdk\jdk1.8\jre\lib\jfr.jar;
           * D:\jdk\jdk1.8\jre\lib\jfxswt.jar;
           * D:\jdk\jdk1.8\jre\lib\jsse.jar;
           * D:\jdk\jdk1.8\jre\lib\management-agent.jar;
           * D:\jdk\jdk1.8\jre\lib\plugin.jar;
           * D:\jdk\jdk1.8\jre\lib\resources.jar;
           * D:\jdk\jdk1.8\jre\lib\rt.jar;
           * E:\IdeaHome\maven\annotation\out\production\annotation;
           * D:\idea2020.1\IntelliJ IDEA 2020.1\lib\idea_rt.jar
           */
      }
  }
  
  ```

  

- 哪些类型有Class对象？

  - class：外部类，成员（成员内部类，静态内部类），局部内部类，匿名内部类
  - interface：接口
  - []：数组
  - enum：枚举
  - annotation：注解 @interface
  - primitive type：基本数据类型
  - void

- 获取运行时类的完整结构

  ```java
  public class Test06 {
      public static void main(String[] args) throws Exception {
          Class<?> clazz = Class.forName("cn.icanci.reflection.User");
          // 获得类的名字
          String name = clazz.getName();
          System.out.println(name);
          // 获得类的的简单名字
          System.out.println(clazz.getSimpleName());
  
          // 获得类的属性
          // 只能找到public属性
          Field[] fields = clazz.getFields();
          for (Field field : fields) {
              System.out.println(field);
          }
          // 能够找到所有的属性
          Field[] declaredFields = clazz.getDeclaredFields();
          for (Field declaredField : declaredFields) {
              System.out.println(declaredField);
          }
  
          // 获得指定属性 只能是 public 的否则会报错
  //        System.out.println(clazz.getField("username"));
          System.out.println(clazz.getDeclaredField("username"));
          // 获得类的方法，
          // 获得本类和父类的全部方法
          System.out.println("=========================================");
          Method[] methods = clazz.getMethods();
          for (Method method : methods) {
              System.out.println(method);
          }
          System.out.println("=========================================");
          // 获取本类的方法
          Method[] declaredMethods = clazz.getDeclaredMethods();
          for (Method declaredMethod : declaredMethods) {
              System.out.println(declaredMethod);
          }
  
          System.out.println("=========================================");
          // 获取指定的方法
          // 参数就是重载
          Method getUsername = clazz.getMethod("getUsername", null);
          Method setUsername = clazz.getMethod("setUsername", String.class);
          System.out.println(getUsername);
          System.out.println(setUsername);
  
          // 获得指定的构造器 只能获取共有构造器
          System.out.println("=========================================");
          Constructor<?>[] constructors = clazz.getConstructors();
          for (Constructor<?> constructor : constructors) {
              System.out.println(constructor);
          }
          System.out.println("=========================================");
          // 能获取共有构造器和私有构造器
          Constructor<?>[] declaredConstructors = clazz.getDeclaredConstructors();
          for (Constructor<?> declaredConstructor : declaredConstructors) {
              System.out.println("* " + declaredConstructor);
          }
          System.out.println("=========================================");
          // 获得指定的构造器
          Constructor<?> declaredConstructor = clazz.getDeclaredConstructor(int.class);
          System.out.println(declaredConstructor);
      }
  }
  
  ```

  

  - 通过反射获取运行时类的完整结构
    - Field
    - Method
    - Constructor
    - SuperClass
    - Interface
    - Annotation
  - 实现的全部接口
  -  所继承的父类
  - 全部的构造器
  - 全部的方法
  - 全部的Field
  - 注解
  - .... 其他

- 有了Class对象，能做什么？
  - 创建类的对象：调用Class对象的newInstance()方法
    - 类必须有一个无参数的构造器
    - 类的构造器的访问权限需要足够
    
  - 步骤：
    - 通过Class类的 getDeclaredConstruor(Class ... prarmeterTypes) 取得本类的指定形参类型的构造器
    - 向构造器的形参中传递一个对象参数进去，里面包含了构造器中所需的各个参数
    - 通过 Constantor实例化对象
    - 不能直接操作属性 需要设置 安全检测 `username.setAccessible(true);`
    
    ```java
    public class Test07 {
        public static void main(String[] args) throws Exception {
    
            // 获得Class对象
            Class<?> userClazz = Class.forName("cn.icanci.reflection.User");
            // 构建一个对象
            // 调用无参构造器
            // User user = (User) userClazz.newInstance();
            // System.out.println(user);
            // Constructor<?> userCons = userClazz.getConstructor(String.class, int.class, int.class);
            // User user2 = (User) userCons.newInstance("ic", 12, 34);
            // System.out.println(user2);
    
            // 通过反射调用普通方法
            User user3 = (User) userClazz.newInstance();
            // 通过反射获取以恶个方法
            Method setUsername = userClazz.getDeclaredMethod("setUsername", String.class);
            setUsername.setAccessible(true);
            setUsername.invoke(user3, "ic");
            System.out.println(user3.getUsername());
    
            // 通过反射操作属性
            User user4 = (User) userClazz.newInstance();
            Field username = userClazz.getDeclaredField("username");
            username.setAccessible(true);
            username.set(user4, "ic2");
            System.out.println(user4.getUsername());
        }
    }
    
    ```

  **调用指定的方法 Object invoke(Object obj,Object ... args)**

  - Object 对应原方法的返回值，若方法无返回值，此时返回null 
  - 若原方法为静态方法，此时形参 Object obj 可为 null
  - 若原方法列表为空，则 Object [] args 可以为null
  - 若原方法声明为 private，则需要在调用此invoke()方法之前，显示调用对象 的`setAccessible(true)` 才能访问 private方法和属性 。否则报错 

- 反射操作泛型
  - Java采用泛型擦除的机制来引入泛型，Java中的泛型仅仅是给编译器javac使用的，避免数据的安全性和免去强制类型转换的问题，但是一旦，编译完成，所有和泛型有关的类型会全部擦除
  - 为了通过反射操作这些类型，Java新增了ParameteriedType GenericArrayType TypeVarilable 和 WildcardType 几种类型标识不能被归一待Class类中但是和原始类型齐名的类型
  - ParameteriedType：表示一种参数化类型 比如 ：Collection<String>
  - GenericArrayType：表示一种元素类型是参数化类型或者剋下变量的数组类型
  - TypeVarilable：是各种类型变量的公共父接口
  - WildcardType ：代表一种通配符类型表达式

  ```java
  public class Test08 {
      public void test01(Map<String, User> map, List<User> list) {
          System.out.println("Test08.test01");
      }
  
      public Map<String, User> test02() {
          System.out.println("Test08.test02");
          return null;
      }
  
      public static void main(String[] args) throws Exception {
          Method test01 = Test08.class.getMethod("test01", Map.class, List.class);
          Type[] getGenericParameterTypes = test01.getGenericParameterTypes();
          for (Type getGenericParameterType : getGenericParameterTypes) {
              System.out.println("* " + getGenericParameterType);
              if (getGenericParameterType instanceof ParameterizedType) {
                  Type[] actualTypeArguments = ((ParameterizedType) getGenericParameterType).getActualTypeArguments();
                  for (Type actualTypeArgument : actualTypeArguments) {
                      System.out.println(actualTypeArgument);
                  }
              }
          }
          System.out.println("========================");
          Method test02 = Test08.class.getMethod("test02");
          Type genericReturnType = test02.getGenericReturnType();
          System.out.println(genericReturnType);
          if (genericReturnType instanceof ParameterizedType) {
              Type[] actualTypeArguments = ((ParameterizedType) genericReturnType).getActualTypeArguments();
              for (Type actualTypeArgument : actualTypeArguments) {
                  System.out.println(actualTypeArgument);
              }
          }
      }
  }
  
  ```

  

- 反射操作注解
  - getAnnotations
  - getAnnotation

  ```java
  public class Test12 {
      public static void main(String[] args) throws Exception {
          // 1. 获取对象
          Class<?> clazz = Class.forName("cn.icanci.reflection.Student2");
          // 2. 通过反射获取注解
          Annotation[] annotations = clazz.getAnnotations();
          for (Annotation annotation : annotations) {
              System.out.println(annotation);
          }
          // 获得 value的值
          Table table = clazz.getAnnotation(Table.class);
          String value = table.value();
          System.out.println(value);
          // 获得类指定的注解
          Field name = clazz.getDeclaredField("name");
          name.setAccessible(true);
          Column nameAnnotation = name.getAnnotation(Column.class);
          System.out.println(nameAnnotation.columnName());
          System.out.println(nameAnnotation.type());
          System.out.println(nameAnnotation.length());
  
          // 获得全部的注解的值
          System.out.println("=====================");
          Field[] declaredFields = clazz.getDeclaredFields();
          for (Field declaredField : declaredFields) {
              declaredField.setAccessible(true);
              Column annotation = declaredField.getAnnotation(Column.class);
              System.out.println(annotation.columnName());
              System.out.println(annotation.type());
              System.out.println(annotation.length());
          }
      }
  }
  
  @Table(value = "student2")
  class Student2 {
      @Column(columnName = "id", type = "int", length = "11")
      private int id;
      @Column(columnName = "age", type = "int", length = "12")
      private int age;
      @Column(columnName = "name", type = "varchar", length = "13")
      private String name;
  
      public Student2() {
      }
  
      public Student2(int id, int age, String name) {
          this.id = id;
          this.age = age;
          this.name = name;
      }
  
      // 省略 get/set/toString
  }
  
  /**
   * 类命的注解
   */
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  @interface Table {
      String value();
  }
  
  /**
   * 属性的注解
   */
  @Target(ElementType.FIELD)
  @Retention(RetentionPolicy.RUNTIME)
  @interface Column {
      String columnName();
  
      String type();
  
      String length();
  }
  ```

- 项目中哪里遇到了反射机制
  - JDBC中，利用反射动态加载了数据库驱动程序。
  - Web服务器中利用反射调用了Sevlet的服务方法。
  - Eclispe等开发工具利用反射动态刨析对象的类型与结构，动态提示对象的属性和方法。
  - 很多框架都用到反射机制，注入属性，调用方法，如Spring。
- 反射的作用
  - 在运行时判断任意一个对象所属的类
  - 在运行时构造任意一个类的对象
  - 在运行时判断任意一个类所具有的成员变量和方法
  - 在运行时调用任意一个对象的方法