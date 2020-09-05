### JDK - 新特性 - Java7

#### 新特性列表

- suppress异常(`新语法`)
- 捕获多个异常(新语法)
- try-with-resources(新语法)
- JSR341-Expression Language Specification(新规范)
- JSR203-More New I/O APIs for the Java Platform(新规范)
- JSR292与InvokeDynamic
- 支持JDBC4.1规范
- Path接口、DirectoryStream、Files、WatchService
- jcmd
- fork/join framework
- Java Mission Control
- 增强泛型推断
- 数字字面量的改进 二进制和数字的下划线
- switch支持String类型

#### suppress异常(`新语法`)

```java
public class BaseException extends Exception {
    public BaseException(Throwable cause) {
        super(cause);
    }
}

public class ReadFile {
    public void read(String filename) throws BaseException {
        FileInputStream input = null;
        IOException readException = null;
        try {
            input = new FileInputStream(filename);
        } catch (IOException ex) {
            readException = ex;
        } finally {
            if (input != null) {
                try {
                    input.close();
                } catch (IOException ex) {
                    if (readException == null) {
                        readException = ex;
                    } else {
                        //使用java7的
                        readException.addSuppressed(ex);
                    }
                }
            }
            if (readException != null) {
                throw new BaseException(readException);
            }
        }
    }
}

```

#### 捕获多个异常(`新语法`)

```java
public void handle() {
    ExceptionThrower thrower = new ExceptionThrower();
    try {
        thrower.manyExceptions();
    } catch (ExceptionA | ExceptionB ab) {
        System.out.println(ab.getClass());
    } catch (ExceptionC c) {
    }
}
```

#### try-with-resources(`新语法`)

```java
/**
 * try-with-resource
 * 不需要使用finally来保证打开的流被正确关闭
 * 这是自动完成的。
 */
public class ResourceBasicUsage {
    public String readFile(String path) throws IOException {
        try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
            StringBuilder builder = new StringBuilder();
            String line = null;
            while ((line = reader.readLine()) != null) {
                builder.append(line);
                builder.append(String.format("%n"));
            }
            return builder.toString();
        }
    }
}
```

- 实现  AutoCloseable  接口

```java
public class CustomResource implements AutoCloseable {
    @Override
    public void close() throws Exception {
        System.out.println("释放资源");
    }

    public void useCustomResource() throws Exception{
        try(CustomResource resource = new CustomResource()){
            System.out.println(resource+" 使用资源");
        }
    }

    public static void main(String[] args) throws Exception{
        new CustomResource().useCustomResource();
    }
}

```

#### JSR341-Expression Language Specification(`新规范`)

```java
public class ELProcessor {
    public Object eval(String s) {
        return Double.valueOf(s);
    }
}

public class Demo {
    public static void main(String[] args) {
        ELProcessor el = new ELProcessor();
        assert (el.eval("Math.random()") instanceof Double);
        System.out.println("Demo.main");
    }
}

```

#### JSR203-More New I/O APIs for the Java Platform(`新规范`)

-  bytebuffer 

```java
public class ByteBufferUsage {
    public void useByteBuffer() {
        ByteBuffer buffer = ByteBuffer.allocate(32);
        buffer.put((byte) 1);
        buffer.put(new byte[3]);
        buffer.putChar('A');
        buffer.putFloat(0.0f);
        buffer.putLong(10, 100L);
        System.out.println(buffer.getChar(4));
        System.out.println(buffer.remaining());
    }

    public void byteOrder() {
        ByteBuffer buffer = ByteBuffer.allocate(4);
        buffer.putInt(1);
        buffer.order(ByteOrder.LITTLE_ENDIAN);
        //值为16777216
        buffer.getInt(0);
    }

    public void compact() {
        ByteBuffer buffer = ByteBuffer.allocate(32);
        buffer.put(new byte[16]);
        buffer.flip();
        buffer.getInt();
        buffer.compact();
        int pos = buffer.position();
    }

    public void viewBuffer() {
        ByteBuffer buffer = ByteBuffer.allocate(32);
        buffer.putInt(1);
        IntBuffer intBuffer = buffer.asIntBuffer();
        intBuffer.put(2);
        int value = buffer.getInt(); //值为2
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        ByteBufferUsage bbu = new ByteBufferUsage();
        bbu.useByteBuffer();
        bbu.byteOrder();
        bbu.compact();
        bbu.viewBuffer();
    }
}
```

-  filechannel 

```Java
public class FileChannelUsage {
    public void openAndWrite() throws IOException {
        FileChannel channel = FileChannel.open(Paths.get("my.txt"), StandardOpenOption.CREATE, StandardOpenOption.WRITE);
        ByteBuffer buffer = ByteBuffer.allocate(64);
        buffer.putChar('A').flip();
        channel.write(buffer);
    }

    public void readWriteAbsolute() throws IOException {
        FileChannel channel = FileChannel.open(Paths.get("absolute.txt"), StandardOpenOption.READ, StandardOpenOption.CREATE, StandardOpenOption.WRITE);
        ByteBuffer writeBuffer = ByteBuffer.allocate(4).putChar('A').putChar('B');
        writeBuffer.flip();
        channel.write(writeBuffer, 1024);
        ByteBuffer readBuffer = ByteBuffer.allocate(2);
        channel.read(readBuffer, 1026);
        readBuffer.flip();
        char result = readBuffer.getChar(); //值为'B'
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws IOException {
        FileChannelUsage fcu = new FileChannelUsage();
        fcu.openAndWrite();
        fcu.readWriteAbsolute();
    }
}
```

#### JSR292与InvokeDynamic

-  JSR 292: Supporting Dynamically Typed Languages on the JavaTM Platform，支持在JVM上运行动态类型语言。在字节码层面支持了InvokeDynamic。 
-  方法句柄MethodHandle 

```java
public class WorkUnit<T> {
}

public class QueueReaderTask implements Runnable {
    public void setQueue(BlockingQueue<WorkUnit<String>> lbq) {
    }

    @Override
    public void run() {

    }
}

public class ThreadPoolManager {
    private final ScheduledExecutorService stpe = Executors
            .newScheduledThreadPool(2);
    private final BlockingQueue<WorkUnit<String>> lbq;

    public ThreadPoolManager(BlockingQueue<WorkUnit<String>> lbq_) {
        lbq = lbq_;
    }

    public ScheduledFuture<?> run(QueueReaderTask msgReader) {
        msgReader.setQueue(lbq);
        return stpe.scheduleAtFixedRate(msgReader, 10, 10, TimeUnit.MILLISECONDS);
    }

    private void cancel(final ScheduledFuture<?> hndl) {
        stpe.schedule(new Runnable() {
            public void run() {
                hndl.cancel(true);
            }
        }, 10, TimeUnit.MILLISECONDS);
    }

    /**
     * 使用传统的反射api
     */
    public Method makeReflective() {
        Method meth = null;
        try {
            Class<?>[] argTypes = new Class[]{ScheduledFuture.class};
            meth = ThreadPoolManager.class.getDeclaredMethod("cancel", argTypes);
            meth.setAccessible(true);
        } catch (IllegalArgumentException | NoSuchMethodException
                | SecurityException e) {
            e.printStackTrace();
        }
        return meth;
    }

    /**
     * 使用代理类
     *
     * @return
     */
    public CancelProxy makeProxy() {
        return new CancelProxy();
    }

    /**
     * 使用Java7的新api，MethodHandle
     * invoke virtual 动态绑定后调用 obj.xxx
     * invoke special 静态绑定后调用 super.xxx
     *
     * @return
     */
    public MethodHandle makeMh() {
        MethodHandle mh;
        MethodType desc = MethodType.methodType(void.class, ScheduledFuture.class);
        try {
            mh = MethodHandles.lookup().findVirtual(ThreadPoolManager.class,
                    "cancel", desc);
        } catch (NoSuchMethodException | IllegalAccessException e) {
            throw (AssertionError) new AssertionError().initCause(e);
        }
        return mh;
    }

    public static class CancelProxy {
        private CancelProxy() {
        }

        public void invoke(ThreadPoolManager mae_, ScheduledFuture<?> hndl_) {
            mae_.cancel(hndl_);
        }
    }
}
```

#### 支持JDBC4.1规范

- abort方法

```java
public class AbortConnection {
    public void abortConnection() throws SQLException {
        Connection connection = DriverManager
                .getConnection("jdbc:mysql:///test");
        ThreadPoolExecutor executor = new DebugExecutorService(2, 10, 60,
                TimeUnit.SECONDS, new LinkedBlockingQueue<Runnable>());
        connection.abort(executor);
        executor.shutdown();
        try {
            executor.awaitTermination(5, TimeUnit.MINUTES);
            System.out.println(executor.getCompletedTaskCount());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private static class DebugExecutorService extends ThreadPoolExecutor {
        public DebugExecutorService(int corePoolSize, int maximumPoolSize,
                                    long keepAliveTime, TimeUnit unit,
                                    BlockingQueue<Runnable> workQueue) {
            super(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue);
        }

        public void beforeExecute(Thread t, Runnable r) {
            System.out.println("清理任务：" + r.getClass());
            super.beforeExecute(t, r);
        }
    }

    public static void main(String[] args) {
        AbortConnection ca = new AbortConnection();
        try {
            ca.abortConnection();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

- 自动关闭

```java
public class SetSchema {
    public void setSchema() throws SQLException {
        try (Connection connection = DriverManager
                .getConnection("jdbc:derby:///test")) {
            connection.setSchema("DEMO_SCHEMA");
            try (Statement stmt = connection.createStatement();
                 ResultSet rs = stmt.executeQuery("SELECT * FROM author")) {
                while (rs.next()) {
                    System.out.println(rs.getString("name"));
                }
            }
        }
    }

    public static void main(String[] args) {
        SetSchema ss = new SetSchema();
        try {
            ss.setSchema();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

- 自动映射

```java
public class UseSQLData {

    public void useSQLData() throws SQLException {
        try (Connection connection = DriverManager
                .getConnection("jdbc:mysql:///test")) {
            Map<String,Class<?>> typeMap = new HashMap<String,Class<?>>();
            typeMap.put("test.Book", Book.class);
            try (Statement stmt = connection.createStatement();
                 ResultSet rs = stmt.executeQuery("SELECT * FROM book")) {
                while (rs.next()) {
                    System.out.println(rs.getObject(1, Book.class));
                }
            }
        }
    }

    public static void main(String[] args) {
        UseSQLData usd = new UseSQLData();
        try {
            usd.useSQLData();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

#### Path接口(`重要接口更新`)

```java
public class PathUsage {
    public void usePath() {
        // get 方法的作用 路径串，或当加入形式的路径串，到串的序列转换Path 
        Path path1 = Paths.get("folder1", "sub1");
        Path path2 = Paths.get("folder2", "sub2");
        //folder1\sub1\folder2\sub2
        path1.resolve(path2);
        //folder1\folder2\sub2
        path1.resolveSibling(path2);
        //..\..\folder2\sub2
        path1.relativize(path2);
        //folder1
        path1.subpath(0, 1);
        //false
        path1.startsWith(path2);
        //false
        path1.endsWith(path2);
        //folder2\my.text
        Paths.get("folder1/./../folder2/my.text").normalize();
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        PathUsage usage = new PathUsage();
        usage.usePath();
    }
}

```

#### DirectoryStream

```java
public class ListFile {
    public void listFiles() throws IOException {
        Path path = Paths.get("");
        try (DirectoryStream<Path> stream = Files.newDirectoryStream(path, "*.*")) {
            for (Path entry : stream) {
                //使用entry
                System.out.println(entry);
            }
        }
    }

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws IOException {
        ListFile listFile = new ListFile();
        listFile.listFiles();
    }
}

```

#### Files

```java
public class FilesUtils {
    public void manipulateFiles() throws IOException {
        Path newFile = Files.createFile(Paths.get("new.txt").toAbsolutePath());
        List<String> content = new ArrayList<String>();
        content.add("Hello");
        content.add("World");
        Files.write(newFile, content, Charset.forName("UTF-8"));
        Files.size(newFile);
        byte[] bytes = Files.readAllBytes(newFile);
        ByteArrayOutputStream output = new ByteArrayOutputStream();
        Files.copy(newFile, output);
        Files.delete(newFile);
    }
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws IOException {
        FilesUtils fu = new FilesUtils();
        fu.manipulateFiles();
    }
}
```

#### WatchService

```java
public class WatchAndCalculate {
    public void calculate() throws IOException, InterruptedException {
        WatchService service = FileSystems.getDefault().newWatchService();
        Path path = Paths.get("").toAbsolutePath();
        path.register(service, StandardWatchEventKinds.ENTRY_CREATE);
        while (true) {
            WatchKey key = service.take();
            for (WatchEvent<?> event : key.pollEvents()) {
                Path createdPath = (Path) event.context();
                createdPath = path.resolve(createdPath);
                long size = Files.size(createdPath);
                System.out.println(createdPath + " ==> " + size);
            }
            key.reset();
        }
    }
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws Throwable {
        WatchAndCalculate wc = new WatchAndCalculate();
        wc.calculate();
    }
}
```

#### jcmd utility

- jcmd是为了替代jps出现了，包含了jps的大部分功能并新增了一些新的功能。

- jcmd -l 列出所有的Java虚拟机，针对每一个虚拟机可以使用help列出它们支持的命令。

![1597020279358](images/1597020279358.png)

- jcmd pid help 

![1597020502569](images/1597020502569.png)

-  jcmd pid VM.flags查看启动参数 

![1597020606573](images/1597020606573.png)

- cmd pid GC.heap_dump D:d.dump 导出堆信息
- jcmd pid GC.class_histogram查看系统中类的统计信息
- jcmd pid VM.system_properties查看系统属性内容
- jcmd pid Thread.print 打印线程栈
- jcmd pid VM.uptime 查看虚拟机启动时间
- jcmd pid PerfCounter.print 查看性能统计

#### fork/join

Java7提供的一个用于并行执行任务的框架，是一个把大任务分割成若干个小任务，最终汇总每个小任务结果后得到大任务结果的框架。

#### Java Mission Control

在JDK7u40里头提供了Java Mission Control，这个是从JRockit虚拟机里头迁移过来的类似JVisualVm的东东。

#### 增强泛型推断

- 之前

```java
Map<String, List<String>> map = new HashMap<String, List<String>>();   
```

- JDK7之后

```java
Map<String, List<String>> anagrams = new HashMap<>();
```

#### Binary Literals支持

- Java7前支持十进制（123）、八进制（0123）、十六进制（0X12AB）
- Java7添加二进制表示（0B11110001、0b11110001） 

#### Numeric Literals的下划线支持

- 如果一个数据很难查数，此时就可以使用数字的下划线支持，注意：只能放数字类型

```java
long num = 1000_000_000;
// 在编译时还需要把下划线去除
```

#### Strings in switch Statements

- 在switch中的参数增加 String类型 ，但是底层原理还是比较的是String的HashCode值，而HashCode值就是int类型

```java
public String generate(String name, String gender) {  
    String title = "";  
    switch (gender) {  
        case "男":  
            title = name + " 先生";  
            break;  
        case "女":  
            title = name + " 女士";  
            break;  
        default:  
            title = name;  
    }  
    return title;  
}
```

