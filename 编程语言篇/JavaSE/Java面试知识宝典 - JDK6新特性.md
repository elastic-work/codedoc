### JDK - 新特性 - Java6

#### 特性列表

- JSR223脚本引擎
- JSR199--Java Compiler API
- JSR269--Pluggable Annotation Processing API
- 支持JDBC4.0规范
- JAX-WS 2.0规范

#### JSR223脚本引擎

- 基本使用

```java
public class Test1 {
    public void greet() throws ScriptException {
        ScriptEngineManager manager = new ScriptEngineManager();
        //支持通过名称、文件扩展名、MIMEtype查找
        ScriptEngine engine = manager.getEngineByName("JavaScript");
        // engine = manager.getEngineByExtension("js");
        // engine = manager.getEngineByMimeType("text/javascript");
        if (engine == null) {
            throw new RuntimeException("找不到JavaScript语言执行引擎。");
        }
        engine.eval("print('Hello!');");
    }

    public static void main(String[] args) {
        try {
            new Test1().greet();
        } catch (ScriptException ex) {
            Logger.getLogger(Test1.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
}
```
#### JSR199--Java Compiler API

```java
public class JavaCompilerAPICompiler {
    public void compile(Path src, Path output) throws IOException {
        JavaCompiler compiler = ToolProvider.getSystemJavaCompiler();
        try (StandardJavaFileManager fileManager = compiler.getStandardFileManager(null, null, null)) {
            Iterable<? extends JavaFileObject> compilationUnits = fileManager.getJavaFileObjects(src.toFile());
            Iterable<String> options = Arrays.asList("-d", output.toString());
            JavaCompiler.CompilationTask task = compiler.getTask(null, fileManager, null, options, null, compilationUnits);
            boolean result = task.call();
        }
    }
}
```

#### JSR269--Pluggable Annotation Processing API

一部分是进行注解处理的javax.annotation.processing，另一部分是对程序的静态结构进行建模的javax.lang.model

#### 其他

- 支持JDBC4.0规范
- JAX-WS 2.0规范(`包括JAXB 2.0`)
- 轻量级HttpServer

**推荐阅读：** https://blog.csdn.net/peterwin1987/article/details/7560637 