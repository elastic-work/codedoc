### JDK - 新特性 - Java5

#### 新特性列表

-  泛型 

- 枚举
- 装箱拆箱
- 变长参数
- 注解
- foreach循环
- 静态导入
- 格式化
- 线程框架/数据结构
- Arrays工具类/StringBuilder/instrument

#### 泛型

-  所谓类型擦除指的就是Java源码中的范型信息只允许停留在编译前期，而编译后的字节码文件中将不再保留任何的范型信息。也就是说，范型信息在编译时将会被全部删除，其中范型类型的类型参数则会被替换为Object类型，并在实际使用时强制转换为指定的目标数据类型。而C++中的模板则会在编译时将模板类型中的类型参数根据所传递的指定数据类型生成相对应的目标代码。 
-  通配符类型：避免unchecked警告，问号表示任何类型都可以接受 

```java
public void printList(List<?> list, PrintStream out) throws IOException {
    for (Iterator<?> i = list.iterator(); i.hasNext(); ) {
        out.println(i.next().toString());
    }
}
```

-  限制类型 

```java

public class Test {   
    public static <A extends Number> double sum(Box<A> box1, Box<A> box2) {
        double total = 0;
        for (Iterator<A> i = box1.contents.iterator(); i.hasNext(); ) {
            total = total + i.next().doubleValue();
        }
        for (Iterator<A> i = box2.contents.iterator(); i.hasNext(); ) {
            total = total + i.next().doubleValue();
        }
        return total;
    }
}

class Box<T> {
    public List contents;

    public Box(T t) {
        contents = new ArrayList();
        // 一些操作
    }
}
```

#### 枚举

-  枚举类是一种特殊的类，可以有自己的成员变量，方法，可以实现一个或多个接口，也可以定义自己的构造器 
-  Java5新增了一个关键字 `enum` （与 `class`，`interface` 关键字的地位相同），用于定义枚举类 
- 所有的枚举类都必须继承于 **Enum**
- 枚举类不可以创建对象
-  一个java源文件最多定义一个 `public` 访问权限的枚举类，且Java源文件也必须和该枚举类的类名相同 
-  但枚举类不是普通的类，它与普通类有如下区别： 
  -  ①、枚举类可以实现一个或者多个接口，使用 `enum` 定义的枚举类默认继承了 `java.lang.Enum` 类，而不是默认继承 `Object` 类，因此枚举类不能显示继承其他父类。其中 `java.lang.Enum` 实现了 `java.lang.Serializable` 和 `java.lang.Comparable` 接口
  - ②、使用 `enum` 定义非抽象的枚举类默认会使用 `final` 修饰，因此枚举类不能派生子类
  - ③、枚举类的构造器只能使用 `private` 访问控制符，如果省略了构造器的访问控制符，则默认使用 `private` 修饰；如果强制使用访问控制符，则只能使用 `private` 修饰符
  - ④、枚举类的所有实例必须在第一行显示列出，否则这个枚举类永远不会产生实例。列出这些实例时，系统会自动添加 `public static final` 修饰，无需显示添加 

```java
public abstract class Enum<E extends Enum<E>>
    implements Comparable<E>, Serializable {

    // 枚举类实例的名字
    private final String name;

    // 返回枚举类的名字
    public final String name() {
        return name;
    }

    // 此枚举常数的序（它在枚举声明，其中初始常量被分配的零序位置）
    private final int ordinal;

    public final int ordinal() {
        return ordinal;
    }

    // 构造方法
    protected Enum(String name, int ordinal) {
        this.name = name;
        this.ordinal = ordinal;
    }

    // 返回名字
    public String toString() {
        return name;
    }

    public final boolean equals(Object other) {
        return this==other;
    }

    public final int hashCode() {
        return super.hashCode();
    }

    protected final Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException();
    }

    // 比较俩个对象
    public final int compareTo(E o) {
        Enum<?> other = (Enum<?>)o;
        Enum<E> self = this;
        if (self.getClass() != other.getClass() && // optimization
            self.getDeclaringClass() != other.getDeclaringClass())
            throw new ClassCastException();
        return self.ordinal - other.ordinal;
    }

    // 获取Class对象
    public final Class<E> getDeclaringClass() {
        Class<?> clazz = getClass();
        Class<?> zuper = clazz.getSuperclass();
        return (zuper == Enum.class) ? (Class<E>)clazz : (Class<E>)zuper;
    }

    // 返回指定枚举类型与所指定名称的枚举常量
    // 该方法用于返回指定枚举类中指定名称的值
    public static <T extends Enum<T>> T valueOf(Class<T> enumType,
                                                String name) {
        T result = enumType.enumConstantDirectory().get(name);
        if (result != null)
            return result;
        if (name == null)
            throw new NullPointerException("Name is null");
        throw new IllegalArgumentException(
            "No enum constant " + enumType.getCanonicalName() + "." + name);
    }

    protected final void finalize() { }

    private void readObject(ObjectInputStream in) throws IOException,
    ClassNotFoundException {
        throw new InvalidObjectException("can't deserialize enum");
    }

    private void readObjectNoData() throws ObjectStreamException {
        throw new InvalidObjectException("can't deserialize enum");
    }
}

```

#### Autoboxing与Unboxing 自动拆箱装箱

-  将primitive类型转换成对应的wrapper类型：Boolean、Byte、Short、Character、Integer、Long、Float、Double 
- 为啥出现自动装箱拆箱，因为集合框架中的泛型只能使用对象类型
- 在一些业务场景中就是int返回的是0，如果不存在默认返回也是0，此时就矛盾了。出现Integer之后，默认值是null

#### 可变参数

- 在一些方法中，其参数不确定，又不好使用数组，因此可变参数出现
- 可变参数在使用的时候，只能放在参数列表的末尾
- 注意： 避免带有变长参数的方法重载 

```java
public class Client {
    /**
     * 简单折扣计算
     *
     * @param price
     * @param discount
     */
    public void calPrice(int price, int discount) {
        float knockdownPrice = price * discount / 100.0F;
        System.out.println("简单折扣后的价格是： " + formateCurrency(knockdownPrice));
    }
 
    /**
     * 复杂折扣计算：折上折
     *
     * @param price
     * @param discounts
     */
    public void calPrice(int price, int... discounts) {
        float knockdownPrice = price * 2;
        for (int discount : discounts) {
            knockdownPrice = knockdownPrice * discount / 100;
        }
        System.out.println("复杂折扣运算后的价格是: " + formateCurrency(knockdownPrice));
    }
 
    /**
     * 格式化成本的货币形式
     *
     * @param price
     * @return
     */
    private String formateCurrency(float price) {
        return NumberFormat.getCurrencyInstance().format(price / 100);
    }
 
    public static void main(String[] args) {
        Client client = new Client();
        client.calPrice(49900, 75);
    }
}
```

-  当传入的参数为（49900,75）时，调用了第一个方法，而不是第二个方法，重载变长参数时，会使编译器无法判断应调用哪个方法，只能根据默认的调用。 

#### 注解

- 注解的出现可以说是促进了框架的升级和改造，现在SpringBoot已经完全使用注解开发，已经省略了XML配置的方式
- 对于注解的讲解，在 `注解和反射篇`有完整讲解

#### 增强for循环

- 做得到的事情
  - 简化集合和数组的读取
- 做不到的事情
  - 遍历同时获取index
  - 集合逗号拼接时去掉最后一个
  - 遍历的同时删除元素 
- 增强for循环是语法糖 ，底层使用迭代器实现

#### 静态导入

- 可以静态导入一个包，然后其包下的静态常量和方法可以直接调用

```java
import static java.lang.Math.*;
public class Test3 {
    public static void main(String[] args) {
        System.out.println(PI);
        System.out.println(max(2, 5));
    }
}

```

#### 格式化

- 出现了类似C语言一样的格式化，方便Java程序输出结果，而不是去拼接字符串

```java
/**
 * java.text.DateFormat
 * java.text.SimpleDateFormat
 * java.text.MessageFormat
 * java.text.NumberFormat
 * java.text.ChoiceFormat
 * java.text.DecimalFormat
 */
public class FormatTester {
    public static void printf() {
        //printf
        String filename = "this is a file";
        try {
            File file = new File(filename);
            FileReader fileReader = new FileReader(file);
            BufferedReader reader = new BufferedReader(fileReader);
            String line;
            int i = 1;
            while ((line = reader.readLine()) != null) {
                System.out.printf("Line %d: %s%n", i++, line);
            }
        } catch (Exception e) {
            System.err.printf("Unable to open file named '%s': %s",
                    filename, e.getMessage());
        }
    }

    public static void stringFormat() {
        // Format a string containing a date.
        Calendar c = new GregorianCalendar(1995, MAY, 23);
        String s = String.format("Duke's Birthday: %1$tm %1$te,%1$tY", c);
        // -> s == "Duke's Birthday: May 23, 1995"
        System.out.println(s);
    }

    public static void formatter() {
        StringBuilder sb = new StringBuilder();
        // Send all output to the Appendable object sb
        Formatter formatter = new Formatter(sb, Locale.US);
        // Explicit argument indices may be used to re-order output.
        formatter.format("%4$2s %3$2s %2$2s %1$2s", "a", "b", "c", "d");
        // -> " d  c  b  a"
        // Optional locale as the first argument can be used to get
        // locale-specific formatting of numbers.  The precision and width can be
        // given to round and align the value.
        formatter.format(Locale.FRANCE, "e = %+10.4f", Math.E);
        // -> "e =    +2,7183"
        // The '(' numeric flag may be used to format negative numbers with
        // parentheses rather than a minus sign.  Group separators are
        // automatically inserted.
        formatter.format("Amount gained or lost since last statement: $ %(,.2f",
                6217.58);
        // -> "Amount gained or lost since last statement: $ (6,217.58)"
    }

    public static void messageFormat() {
        String msg = "欢迎光临，当前（{0}）等待的业务受理的顾客有{1}位，请排号办理业务！";
        MessageFormat mf = new MessageFormat(msg);
        String fmsg = mf.format(new Object[]{new Date(), 35});
        System.out.println(fmsg);
    }

    public static void dateFormat() {
        String str = "2010-1-10 17:39:21";
        SimpleDateFormat format = new SimpleDateFormat("yyyyMMddHHmmss");
        try {
            System.out.println(format.format(format.parse(str)));
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        formatter();
        stringFormat();
        messageFormat();
        dateFormat();
        printf();
    }
}
```

#### 线程框架/数据结构

- 不抛出异常的方法

```java
public class BubbleSortThread extends Thread {
    private int[] numbers;
    public BubbleSortThread(int[] numbers) {
        setName("Simple Thread");
        setUncaughtExceptionHandler(
            new SimpleThreadExceptionHandler());
        this.numbers = numbers;
    }
    public void run() {
        int index = numbers.length;
        boolean finished = false;
        while (!finished) {
            index--;
            finished = true;
            for (int i=0; i<index; i++) {
                // Create error condition
                if (numbers[i+1] < 0) {
                    throw new IllegalArgumentException(
                        "Cannot pass negative numbers into this thread!");
                }
                if (numbers[i] > numbers[i+1]) {
                    // swap
                    int temp = numbers[i];
                    numbers[i] = numbers[i+1];
                    numbers[i+1] = temp;
                    finished = false;
                }
            }
        }    
    }
}
class SimpleThreadExceptionHandler implements
    Thread.UncaughtExceptionHandler {
    public void uncaughtException(Thread t, Throwable e) {
        System.err.printf("%s: %s at line %d of %s%n",
                          t.getName(), 
                          e.toString(),
                          e.getStackTrace()[0].getLineNumber(),
                          e.getStackTrace()[0].getFileName());
    }
}
```

- blocking queue 

```java
public class Producer extends Thread {
    private BlockingQueue q;
    private PrintStream out;
    public Producer(BlockingQueue q, PrintStream out) {
        setName("Producer");
        this.q = q;
        this.out = out;
    }
    public void run() {
        try {
            while (true) {
                q.put(produce());
            }
        } catch (InterruptedException e) {
            out.printf("%s interrupted: %s", getName(), e.getMessage());
        }
    }
    private String produce() {
        while (true) {
            double r = Math.random();
            // Only goes forward 1/10 of the time
            if ((r*100) < 10) {
                String s = String.format("Inserted at %tc", new Date());
                return s;
            }
        }
    }
}
```

- 线程池 

- 著名的JUC类库。
  - 每次提交任务时，如果线程数还没达到coreSize就创建新线程并绑定该任务。 所以第coreSize次提交任务后线程总数必达到coreSize，不会重用之前的空闲线程。
  - 线程数达到coreSize后，新增的任务就放到工作队列里，而线程池里的线程则努力的使用take()从工作队列里拉活来干。
  - 如果队列是个有界队列，又如果线程池里的线程不能及时将任务取走，工作队列可能会满掉，插入任务就会失败，此时线程池就会紧急的再创建新的临时线程来补救。
  - 临时线程使用poll(keepAliveTime，timeUnit)来从工作队列拉活，如果时候到了仍然两手空空没拉到活，表明它太闲了，就会被解雇掉。
  - 如果core线程数＋临时线程数 >maxSize，则不能再创建新的临时线程了，转头执行RejectExecutionHanlder。默认的AbortPolicy抛RejectedExecutionException异常，其他选择包括静默放弃当前任务(Discard)，放弃工作队列里最老的任务(DisacardOldest)，或由主线程来直接执行(CallerRuns)，或你自己发挥想象力写的一个。

#### Arrays工具类/StringBuilder/instrument

- Arrays

```java
  Arrays.sort(myArray);
  Arrays.toString(myArray)
  Arrays.binarySearch(myArray, 98)
  Arrays.deepToString(ticTacToe)
  Arrays.deepEquals(ticTacToe, ticTacToe3)
```

- Queue 避开集合的add/remove操作，使用offer、poll操作（不抛异常）

```java
Queue q = new LinkedList(); 采用它来实现queue
```

- Override返回类型
  - 支持协变返回
- 单线程StringBuilder
- 线程不安全，在单线程下替换string buffer提高性能
  - java.lang.Instrument