### JDK - 新特性 - Java8

#### 新特性列表

- Lambda表达式
- 函数式接口
- 方法引用和构造函数调用
- Stream流
- 在JDK7中的新特性 fork/join 框架和Stream流搭配使用
- 接口中的默认方法和静态方法
- Optional 容器
- 新时间日期API
- HashMap底层数据结构的优化

---

#### Lambda表达式

- Lambda表达式本质上是一段匿名内部类，也是一段可以传递的代码
- 在没有出现Lambda表达式之前，匿名内部类好大一堆代码
- 实例代码

```java
public class LambdaTest {
    public static void main(String[] args) {
        Comparator<Integer> cmt = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        };

        TreeSet<Integer> set = new TreeSet<>(cmt);
        System.out.println("===================");
        TreeSet<Integer> set2
            = new TreeSet<>((o1, o2) -> Integer.compare(o1, o2));

    }
}
```

- 注意：在使用Lambda表达式的时候，必须是函数式接口 - 也就是一个接口被`@FunctionalInterface`注解修饰，指仅含有一个抽象方法的接口 
- 尽管下面的比较器`Comparator`接口都多个方法，但是其父类（此处有争议，因为接口不继承于Object，但是其中有个映射一样的东西，实际上内部含有Object类的方法 ）

```java
@FunctionalInterface
public interface Comparator<T> {
    // 核心比较方法
    int compare(T o1, T o2);
    // 非目标方法
    boolean equals(Object obj);
    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }

    // 省略其他方法
}
```

Lmabda表达式的语法总结： () -> ();

| 前置                                       | 语法                                                 |
| ------------------------------------------ | ---------------------------------------------------- |
| 无参数无返回值                             | () -> System.out.println(“Hello WOrld”)              |
| 有一个参数无返回值                         | (x) -> System.out.println(x)                         |
| 有且只有一个参数无返回值                   | x -> System.out.println(x)                           |
| 有多个参数，有返回值，有多条lambda体语句   | (x，y) -> {System.out.println(“xxx”);return xxxx;}； |
| 有多个参数，有返回值，只有一条lambda体语句 | (x，y) -> xxxx                                       |

口诀：左右遇一省括号，左侧推断类型省

注：当一个接口中存在多个抽象方法时，如果使用lambda表达式，并不能智能匹配对应的抽象方法，因此引入了函数式接口的概念

#### 函数式接口

- 函数式接口的提出是为了给Lambda提供更好的支持
- 常见的函数式接口
  - 在JDK8之后提供和超级多的函数式接口 在`java.util.function`包下
  - Callable<T> 有返回值，可以抛出异常的多线程方法
  - Runnable 多线程
  - Consumer<T> 消费型接口，有参数无返回值
  - Supplier<T> 供给型接口，无参有返回值
  - Function<T,R> 函数式接口，有参有返回值
  - Predicate<T> 断言型接口，有参有返回值，返回值是boolean类型

#### 方法引用和构造函数调用

- 如果Lambda体中的内容有方法已经实现了，那么可以使用“方法引用”
- 也可以理解为方法引用时Lambda表达式的另外一种表现形式并且其语法比Lambda表达式更简单

- **方法引用**

  - 对象::实例方法名
  - 类::静态方法名
  - 类::实例方法名（Lambda参数列表中第一个参数是实例方法的调用者，第二个参数是实例方法的参数调用）

  ```java
  public class MethodInvokeTest {
      public static void main(String[] args) {
          test();
      }
  
      public static void test() {
          // lambda体中调用方法的参数列表与返回值类型，
          // 要与函数式接口中抽象方法的函数列表和返回值类型保持一致！
          // 若lambda参数列表中的第一个参数是实例方法的调用者，
          // 而第二个参数是实例方法的参数时，可以使用ClassName::method
          Consumer<Integer> con = (x) -> System.out.println(x);
          con.accept(100);
  
          // 方法引用 - 对象::实例方法
          Consumer<Integer> con2 = System.out::println;
          con.accept(200);
  
          // 方法引用 - 类命::静态方法名
          BiFunction<Integer, Integer, Integer> bifun = (x, y) -> Integer.compare(x, y);
          BiFunction<Integer, Integer, Integer> bifun2 = Integer::compare;
          Integer apply = bifun.apply(100, 200);
  
          // 方法引用
          BiFunction<String, String, Boolean> fun1 = (s1, s2) -> s1.equals(s2);
          BiFunction<String, String, Boolean> fun2 = String::equals;
          Boolean res = fun2.apply("Hello", "world");
      }
  }
  
  ```

- **构造器引用**

  - ClassName::new

  ```java
  public class MethodInvokeTest {
      public static void main(String[] args) {
          test2();
      }
  
      public static void test2() {
          // 构造方法引用 类名::new
          Supplier<User> sup = () -> new User();
          System.out.println(sup.get());
          Supplier<User> sup2 = User::new;
          System.out.println(sup2.get());
  
          // 构造方法引用 类名::new（带有一个参数）
          Function<Integer, User> fun = (x) -> new User(x);
          Function<Integer, User> fun2 = User::new;
          System.out.println(fun2.apply(100));
      }
  }
  ```

- **数组引用**

  - Type[] :: new

  ```java
  public class MethodInvokeTest {
      public static void main(String[] args) {
          test3();
      }
  
      public static void test3() {
          Function<Integer, String[]> fun = (x) -> new String[x];
          Function<Integer, String[]> fun2 = String[]::new;
          String[] apply = fun2.apply(4);
          Arrays.stream(apply).forEach(System.out::println);
      }
  }
  ```

#### Stream流

- Stream 操作的三个步骤
  - 创建stream
  - 中间操作（过滤、map）
  - 终止操作

- 创建stream

```java
public class StreamTest {
    public static void test() {

        // Stream的创建

        // 1.校验通过Collection系列集合提供的stream()或者paralleStream()
        ArrayList<String> list = new ArrayList<>();
        Stream<String> stream = list.stream();
        // 2.通过Arrays的静态方法Stream()获取数组流
        String[] str = new String[10];
        Stream<String> stream1 = Arrays.stream(str);
        // 3.通过Stream类中的静态方法of
        Stream<String> aa = Stream.of("aa", "bb", "cc");
        // 4.创建无限流
        // 迭代
        Stream<Integer> iterate = Stream.iterate(1, (x) -> x + 2);
        // 生成
        Stream.generate(() -> Math.random());
    }
}

```

- Stream的中间操作

```java
public class StreamTest {

    private static List<Emp> users = new ArrayList<>();

    public static void test2() {
        // 筛选 过滤 去重
        users.stream()
                .filter(e -> e.getAge() > 10)
                .limit(4)
                .skip(4)
                // 需要流中的元素重写hashcode和equals方法
                .distinct()
                .forEach(System.out::println);
        // 生成新的流 通过map映射
        users.stream()
                .map((e) -> e.getAge())
                .forEach(System.out::println);
        // 非自然排序
        users.stream()
                .sorted((e1, e2) -> e2.getAge() - e1.getAge())
                .forEach(System.out::println);
    }
}

class Emp {
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

- Stream的终止操作

```java
public class StreamTest {

    private static List<Emp> users = new ArrayList<>();

    public static void test3() {
        /**
         *      查找和匹配
         *          allMatch-检查是否匹配所有元素
         *          anyMatch-检查是否至少匹配一个元素
         *          noneMatch-检查是否没有匹配所有元素
         *          findFirst-返回第一个元素
         *          findAny-返回当前流中的任意元素
         *          count-返回流中元素的总个数
         *          max-返回流中最大值
         *          min-返回流中最小值
         */

        // 检查是否匹配元素
        boolean b = users.stream().allMatch((e) -> e.getAge() == 18);
        System.out.println(b);

        boolean b1 = users.stream().anyMatch((e) -> e.getAge() == 18);
        System.out.println(b1);

        boolean b2 = users.stream().noneMatch((e) -> e.getAge() == 18);
        System.out.println(b2);

        Optional<Emp> first = users.stream().findFirst();
        System.out.println(first.get());

        // 并行流
        Optional<Emp> any = users.parallelStream().findAny();
        System.out.println(any.get());

        long count = users.stream().count();
        System.out.println(count);

        Optional<Emp> max = users.stream().max((e1, e2) -> e1.getAge() - e2.getAge());
        System.out.println(max.get());

        Optional<Emp> min = users.stream().min((e1, e2) -> e1.getAge() - e2.getAge());
        System.out.println(min.get());
    }
}

class Emp {
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

-  还有功能比较强大的两个终止操作 reduce和collect 

```java
// reduce 操作  reduce:(T identity,BinaryOperator)/reduce(BinaryOperator)-可以将流中元素反复结合起来，得到一个值

public class StreamTest {

    public static void main(String[] args) {
        test4();
    }

    public static void test4() {
        List<Integer> list = Arrays.asList(1, 2, 3, 5, 6, 7, 8, 9, 0);
        Integer reduce = list.stream().reduce(0, (x, y) -> x + y);
        System.out.println(reduce); // 41
    }
}
```

-  collect操作：Collect-将流转换为其他形式，接收一个Collection接口的实现，用于给Stream中元素做汇总的方法 

```java
public class StreamTest {

    private static List<Emp> users = new ArrayList<>();

    public static void test5() {
        // collect：收集操作
        List<Integer> collect = users.stream().map(Emp::getAge)
                .collect(Collectors.toList());
        collect.stream().forEach(System.out::println);
    }

```

- **并行流和串形流**
  - 在JDK8新的Stream包中针对集合的操作页提供了并行操作流和串型操作流
  - 并行流就是把内容切割为多个数据块，并且使用多个线程处理每个数据块的内容
  - Stream API  中声明可以通过 parallel() 与 sequential () 方法在并行流和串行流之间进行切换
  - JDK8使用的是fork/join框架进行并行操作

#### 在JDK7中的新特性 fork/join 框架和Stream流搭配使用

- Fork/Join 框架：就是在必要的情况下，将一个大任务，进行拆分(fork)成若干个小任务（拆到不可再拆时），再将一个个的小任务运算的结果进行 join 汇总。
- **关键字：递归分合、分而治之。**
- 采用 “工作窃取”模式（work-stealing）：当执行新的任务时它可以将其拆分分成更小的任务执行，并将小任务加到线程队列中，然后再从一个随机线程的队列中偷一个并把它放在自己的队列中相对于一般的线程池实现,fork/join框架的优势体现在对其中包含的任务的处理方式上.在一般的线程池中,如果一个线程正在执行的任务由于某些原因无法继续运行,那么该线程会处于等待状态.而在fork/join框架实现中,如果某个子问题由于等待另外一个子问题的完成而无法继续运行.那么处理该子问题的线程会主动寻找其他尚未运行的子问题来执行.这种方式减少了线程的等待时间,提高了性能.。 

- 要想使用Fork - Join，类必须继承 RecursiveAction（无返回值） 或者   RecursiveTask（有返回值） 

```java
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;
    private Long end;

    // 临界值
    private long temp = 1000L;

    public ForkJoinDemo(long start, long end) {
        this.start = start;
        this.end = end;
    }

    // 计算方法
    @Override
    protected Long compute() {
        Long sum = 0L;
        if ((end - start) < temp) {
            for (Long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        } else {
            // 分支合并计算
            long middle = (start + end) / 2;
            ForkJoinDemo forkJoin1 = new ForkJoinDemo(start, middle);
            // 拆分任务 把任务压入线程队列
            forkJoin1.fork();
            ForkJoinDemo forkJoin2 = new ForkJoinDemo(middle + 1, end);
            // 拆分任务 把任务压入线程队列
            forkJoin2.fork();
            return forkJoin1.join() + forkJoin2.join();
        }
    }
}

class ForkJoinTest {
    public static void main(String[] args) {
        test1();
        test2();
        test3();
    }

    /**
     * 普通程序员
     * <p>
     * sum = 500000000500000000 时间：7134 ms
     */
    public static void test1() {
        long start = System.currentTimeMillis();
        Long sum = 0L;
        for (Long i = 1L; i <= 10_0000_0000; i++) {
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum = " + sum + " 时间：" + (end - start) + " ms");
    }

    /**
     * 中级程序员
     * <p>
     * sum = 500000000500000000 时间：3859 ms
     */
    public static void test2() {
        long start = System.currentTimeMillis();
//        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinDemo forkJoinDemo = new ForkJoinDemo(1, 10_0000_0000);
//        forkJoinPool.execute(forkJoinDemo);
//        Long sum = 0L;

        Long sum = forkJoinDemo.compute();
//
//        try {
//            sum = forkJoinDemo.get();
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        } catch (ExecutionException e) {
//            e.printStackTrace();
//        }
        long end = System.currentTimeMillis();
        System.out.println("sum = " + sum + " 时间：" + (end - start) + " ms");

    }

    /**
     * 高级程序员
     * <p>
     * sum = 500000000500000000 时间：830 ms
     */
    public static void test3() {
        long start = System.currentTimeMillis();
        // Stream 并行流
        long sum = LongStream
                .rangeClosed(0L, 10_0000_0000L)
                .parallel()
                .reduce(0, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("sum = " + sum + " 时间：" + (end - start) + " ms");
    }
}

```

#### 接口中的默认方法和静态方法

- 在JDK8之前，接口中的方法不可以有方法体，但是在JDK8之后，可以有，但是非静态的方法必须使用default修饰
- 也可以有静态方法

```java
public interface Animal {
    void run();

    void eat();

    /**
     * 可以定义带有方法体的方法 但是如果不是静态方法，必须使用default修饰
     */
    default void hello(){
        System.out.println("Animal.hello");
    }

    /**
     * 也可定义含有方法体的静态方法 默认就是public
     */
    static void test(){
        System.out.println("Animal.test");
    }
}

```

#### Optional 容器

- Optional 是JDK8提供的新类，为了处理空指针异常

```markdown
Optional.of(T t); // 创建一个Optional实例
Optional.empty(); // 创建一个空的Optional实例
Optional.ofNullable(T t); // 若T不为null，创建一个Optional实例，否则创建一个空实例
isPresent();    // 判断是够包含值
orElse(T t);   //如果调用对象包含值，返回该值，否则返回T
orElseGet(Supplier s);  // 如果调用对象包含值，返回该值，否则返回s中获取的值
ap(Function f): // 如果有值对其处理，并返回处理后的Optional，否则返回Optional.empty();
flatMap(Function mapper);// 与map类似。返回值是Optional
总结：Optional.of(null)  会直接报NPE
```

- 代码测试

```java
public class OptionalTest {
    public static void main(String[] args) {

        // 创建对象，对象传入的对象不能为空，空就要抛出异常
        // Optional<Object> o = Optional.of(null);
        Optional<String> o = Optional.of("icanci");
        System.out.println(o.get());
        // 不为空 就返回true
        System.out.println(o.isPresent());
        // 可以传递null值进去
        Optional<Object> o1 = Optional.ofNullable(null);
        System.out.println(o1.isPresent());
        // orElse 兜底方法 如果对象为空，就返回设置的值 只能兜底一次
        Object hello = Optional.ofNullable(null).orElse("hello k");
        System.out.println(hello);

        System.out.println("---------------------------");
        // 另外一个奇怪的方法
        // 如果 ofNullable 中的参数对象为空，就执行后面的兜底方法 否则实行map中的判断方法
        // 返回值类型 根据 map中的函数式表达结果一致
        Boolean aNull = Optional.ofNullable("null").map(obj -> obj.equals("null")).orElse(false);
        System.out.println(aNull);
    }
}

```

#### 新时间日期API

- 在JDK8之前，使用的是Date、SimpleDateFormat、Calendar类
- 但是在多线程情况下是线程不安全的，所以JDK新的日期API出现，并且比之前更好操作
- 演示案例1

```java
public class LocalDateTest {
    public static void main(String[] args) {
        // -----------------------------------------------------------//
        // LocalDate 不包含具体时间的日期
        LocalDate now = LocalDate.now();
        // 打印今天的日期
        System.out.println(now);

        // 获取年月日 可以直接获取，不用
        int year = now.getYear();
        System.out.println(year);
        Month month = now.getMonth();
        System.out.println(month);
        int dayOfYear = now.getDayOfYear();
        System.out.println(dayOfYear);
        DayOfWeek dayOfWeek = now.getDayOfWeek();
        System.out.println(dayOfWeek);
        int dayOfMonth = now.getDayOfMonth();
        System.out.println(dayOfMonth);
        // 操作日期对象 加减年份
        // 返回的结果是新的 但是旧的还是旧的
        LocalDate localDate = now.plusYears(1);
        System.out.println(localDate.getYear());
        // 日期的比较
        System.out.println("isAfter:" + localDate.isAfter(now));
        System.out.println("isBefore:" + localDate.isBefore(now));

        // -----------------------------------------------------------//
        // 不包含日期的时间
        LocalTime localTime = LocalTime.now();
        System.out.println(localTime.getHour());

        // -----------------------------------------------------------//
        // 包含了日期和时间
        LocalDateTime localDateTime= LocalDateTime.now();
        System.out.println(localDateTime.getDayOfMonth());
    }
}

```

- 演示案例2

```java
public class LocalDateFormatTest {
    public static void main(String[] args) {
        // 获取当前时间
        LocalDateTime now = LocalDateTime.now();
        System.out.println(now);
        // 获取格式化对象
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String format = formatter.format(now);
        System.out.println(format);

        // 获取指定的时期对象
        LocalDateTime of = LocalDateTime.of(2020, 12, 8, 12, 35);
        String format1 = formatter.format(of);
        System.out.println(format1);

        // 计算日期时间差
        Duration between = Duration.between(now, of);
        long l = between.toDays();
        System.out.println(l);
        long l1 = between.toHours();
        System.out.println(l1);

    }
}

```

-  表示日期的LocalDate
- 表示时间的LocalTime
- 表示日期时间的LocalDateTime 

#### HashMap底层数据结构的优化

- 在JDK8之前，HashMap底层是**数组+链表**
- 在JDK8之后，HashMap底层是**数组+链表+红黑树**
- 具体的解析在 `Java 集合框架 - 哈希表 HashMap.md`  `Java 集合框架 - 哈希表 HashMap 在理解.md` `Java 集合框架 - HashMap 底层 红黑树深度解读.md`已经有讲解 此处不在演示