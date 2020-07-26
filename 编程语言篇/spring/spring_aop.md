### 1、在Spring中AOP是如何实现的?

- 如果你的类是实现接口。spring使用jdk动态代理 (对**对象**进行代理)
- 如果没有实现接口，spring使用cglib生成对象子类 (对**类(字节码)**进行代理)

### 2.aop基本概念及相关术语

```
连接点(Joinpoint)：类中所有方法

切入点（Pointcut）：被抽取了共性功能的代码（一定是连接点）

通知（Advice）：抽取的共性功能组成的代码逻辑（方法）

目标对象（Target  Object）：有切入点方法的对象

AOP代理（AOP Proxy）：切入点所在的类对象执行切入点方法时，需将原始的共性功能（通知）加入，此时通过代理的形式创建类对象，并完成通知的加入

织入（Weaving）：代理对象把通知织入到目标对象的切入点方法中，是一个动作

切面（Aspect）：切入点与通知的匹配模式（绑定关系）
```

### 3.切入点

```
作用：用来匹配切入地方法
格式：execution(切入点表达式)
		
execution([方法的访问控制修饰符] 方法的返回值 包名.类名/接口名.方法名(参数))
		
注意：方法的访问控制修饰符可以省略，写方法名的时候要把包名和类名全部带上

通配符：*,..(表示任意多个包、参数)
	1、可以匹配返回值类型
	2、可以匹配包名
	3、可以匹配类名
	4、可以匹配方法名
	5、可以匹配参数
切入点配置方式
1、局部切入点：
	<aop:config>
	<aop:aspect ref="advice">
			<!-- 切入点表达式 把方法和包名类名都带上 -->
			<aop:before method="one" pointcut="execution(* com.spring.aop.demo.TargetObject.method1())" />
		</aop:aspect>
	</aop:config>
2、切面间共享切入点：
<aop:config>
		<!--配置切面  -->
		<!--配置通知与切入点之间的联系,把通知类中的方法和目标对象中的方法进行关联-->
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
		<aop:aspect ref="advice">
			<aop:before method="one" pointcut-ref="abc"/>
			<aop:before method="two" pointcut-ref="abc"/>
		</aop:aspect>
	</aop:config>
3、切面内部切入点：
<aop:config>
		<!--配置切面  -->
		<!--配置通知与切入点之间的联系,把通知类中的方法和目标对象中的方法进行关联-->
		
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<aop:before method="one" pointcut-ref="abc">
			<aop:before method="two" pointcut-ref="abc">
		</aop:aspect>
	</aop:config>
```

### 

### 4.SpringAOP简单实例

```
package com.spring.aop.demo;
/**
*@author 中二少年阿基 vg
* 目标对象，切入点
*/
public class TargetObject {
	//切入点
	public void method1() {
	//	System.out.println("共性的功能");
		System.out.println("1");
		System.out.println("2");
		
	}
	
	public void method2() {
	//	System.out.println("共性的功能");
		System.out.println("1");
		System.out.println("2");
			
		}
}

```

```
package com.spring.aop.demo;
/**
*@author 中二少年阿基 vg
*/

//通知类,抽取的目标对象中的共性的代码
public class MyAdvice {
	public void one() {
		System.out.println("共性功能");
	}
}

```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd">

	<aop:config>
		<!--配置切面  -->
		<!--配置通知与切入点之间的联系,把通知类中的方法和目标对象中的方法进行关联-->
		<aop:aspect ref="advice">
			<!-- 切入点表达式 把方法和包名类名都带上 -->
			<aop:before method="one" pointcut="execution(* com.spring.aop.demo.TargetObject.method1())" />
		</aop:aspect>
	</aop:config>
	
	<!-- 目标对象 -->
	<bean id="target" class="com.spring.aop.demo.TargetObject"></bean>
	<!-- 通知 -->
	<bean id="advice" class="com.spring.aop.demo.MyAdvice"></bean>
</beans>
```

```
package com.spring.aop.demo;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
/**
* 测试类
*/
public class Test {
	public static void main(String[] args) {
		ApplicationContext vg = new ClassPathXmlApplicationContext("applicationContext.xml");
		Object bean = vg.getBean("target");
		TargetObject to = (TargetObject) bean;
		to.method1();
		System.out.println("--------------");
		to.method2();
	}
}

```

```
总结AOP的工作流程和运行流程
AOP工作流程：
	1、制作目标类（功能类）
	2、制作通知类
	3、配置切面
AOP运行流程：
	1、Spring监控配置文件中切入点对应方法执行
	2、发现执行，则立即生成切入点所在类对象的代理对象
	3、织入
织入分为3个种类：编译期织入、装载时织入、运行时织入。Spring是运行时织入。
	编译期织入：执行效率高，不够灵活
	装载时织入：稍微灵活，可以在JVM装载目标类对象和声明类对象的时候进行合并，效率已经不够高
	运行时织入：最灵活，通过配置文件进行目标类对象和通知进行合并的，效率低
```

### 5、AOP通知

```
通知类型就是共性代码是在目标对象方法的代码的前面还是后面执行的类型就是通知类型。
类型：位置的意思
AOP的通知类型共5种
	1、before:前置通知(应用：各种校验)
	在方法执行前执行 
	该方法中出现了异常，不会影响前置通知的执行
	2、after:后通知(应用：清理现场)
	方法执行完毕后执行，无论方法中是否出现异常都会执行
	3、afterReturning:返回后通知(应用：常规数据处理)
	方法正常返回后执行，如果方法中抛出异常，无法执行
	4、afterThrowing:抛出异常后通知(应用：包装异常信息)
	方法抛出异常后执行，如果方法没有抛出异常，无法执行
	5、around:环绕通知(应用：十分强大，可以做任何事情)
方法执行前后分别执行，如果方法中有异常，末尾的通知就不执行了
```

```
跟上SpringAOP简单实例的<aop:config></aop:config>
1、before:前置通知(应用：各种校验)
	在方法执行前执行 
	该方法中出现了异常，不会影响前置通知的执行
	<aop:config>
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<aop:before method="before" pointcut-ref="abc" />	
			
		</aop:aspect>
	</aop:config>
```

```
2、after:后通知(应用：清理现场)
	方法执行完毕后执行，无论方法中是否出现异常都会执行
	<aop:config>
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<aop:before method="before" pointcut-ref="abc" />	
			<aop:after method="after" pointcut-ref="abc" />	
		</aop:aspect>
	</aop:config>
```

```
3、afterReturning:返回后通知(应用：常规数据处理)
	方法正常返回后执行，如果方法中抛出异常，无法执行
	<aop:config>
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<!-- <aop:before method="before" pointcut-ref="abc" />	
			<aop:after method="after" pointcut-ref="abc" />	 -->
			<aop:after-returning method="afterReturning" pointcut-ref="abc" />
		</aop:aspect>
	</aop:config>
```

```
4、afterThrowing:抛出异常后通知(应用：包装异常信息)
	方法抛出异常后执行，如果方法没有抛出异常，无法执行
	<aop:config>
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<!-- <aop:before method="before" pointcut-ref="abc" />	
			<aop:after method="after" pointcut-ref="abc" />	 -->
			<!-- <aop:after-returning method="afterReturning" pointcut-ref="abc" /> -->
			<aop:after-throwing method="afterThrowing" pointcut-ref="abc" />
		</aop:aspect>
	</aop:config>
```

```
5、around:环绕通知(应用：十分强大，可以做任何事情)
方法执行前后分别执行，如果方法中有异常，末尾的通知就不执行了
	<aop:config>
		<aop:aspect ref="advice">
		<aop:pointcut  expression="execution(* com.spring.aop.demo.TargetObject.method1())" id="abc"/>
			<!-- <aop:before method="before" pointcut-ref="abc" />	
			<aop:after method="after" pointcut-ref="abc" />	 -->
			<!-- <aop:after-returning method="afterReturning" pointcut-ref="abc" /> -->
			<!-- <aop:after-throwing method="afterThrowing" pointcut-ref="abc" /> -->
			<aop:around method="around" pointcut-ref="abc" />
		</aop:aspect>
	</aop:config>
```

```
package com.spring.aop.demo;

import org.aspectj.lang.ProceedingJoinPoint;

/**
*@author 中二少年阿基 vg
*/

//通知类,抽取的目标对象中的共性的代码
public class MyAdvice {
	public void before() {
		System.out.println("before...");
	}
	public void after() {
		System.out.println("after...");
	}
	public void afterReturning() {
		System.out.println("afterReturning...");
	}
	public void afterThrowing() {
		System.out.println("afterThrowing...");
	}
	//当around通知方法被调用会传入pjp连接点这个对象，这个对象表示被拦截的方法，由Spring传过来
	public void around(ProceedingJoinPoint pjp) throws Throwable {
		System.out.println("around...start");
		//匹配的方法的调用/拦截的method1方法调用
		pjp.proceed();  //调用对象中被拦截的方法
		System.out.println("around...end");
	}
	public void one() {
		System.out.println("共性功能");
	}
	public void two() {
		System.out.println("共性功能1");
	}
}

```

### 6、AOP通知的相对顺序

```
AOP的通知类型存在5种，其中分为在方法前执行与方法后执行

方法执行前运行
before
around

方法执行后运行
after
afterReturning
around(after)


前置通知的执行顺序和在配置文件中配置的顺序一致
后置通知的执行顺序也和配置文件中的配置顺序一致
总而言言之，在同一个切面中，先配置先执行！

在不同的切面中，
前置通知的执行顺序和在配置文件中配置的顺序一致
后置通知的配置顺序和执行顺序相反，越是最后配置的越先执行，越先配置的越后执行

```

### 7、前置通知获取匹配方法参数

```

```

### 8、前置通知修改对象目标方法

```

```

### 9、后置通知获取对象方法的返回值

```

```

### 10、异步通知获取方法的异常对象

```

```

### 11、简单的注解开发

```
package com.spring.annotation.aop.demo;

import org.springframework.stereotype.Component;
/**
* 目标对象  切入点，连接点
*/
@Component("target")
public class TargetObject {
	public void method() {
		//System.out.println("target.....11111....");  //被抽取的共性功能
		System.out.println("target.....22222....");
	}
}

```

```
package com.spring.annotation.aop.demo;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;
/**
* 通知类  抽取的目标对象中的共性的代码
*/
@Component("advice")
@Aspect
public class MyAdvice {
	//前置通知他去和什么样的类什么样包,什么样包下什么样类什么样方法进行匹配
	@Before(value = "execution(* *..*.method())")
	public void before() {
		System.out.println("before.....11111....");
	}
}

```

```
/**
* 测试类
*/
public class Test {
	public static void main(String[] args) {
		ApplicationContext vg = new ClassPathXmlApplicationContext("applicationContext.xml");
		//得到目标对象 @Component("target")
		TargetObject target= (TargetObject) vg.getBean("target");
		target.method();
	}
}	

```

```
/**
* 注解开发AOP
* 打开注解的配置空间支持及aop空间的配置空间支持
*/
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	 xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">
	
    <!--  开启spring的注解的自动扫描功能 -->
    <context:component-scan base-package="com.spring.annotation.aop.demo"></context:component-scan>
    <!-- 把aop的注解功能打开 -->
    <aop:aspectj-autoproxy />
</beans>
```

### 12、不同种类的注解通知出现的顺序

```

```

### 13、相同种类的注解通知出现的顺序

```

```

### 14.jdk动态代理

```
/**
*目标接口类
*/
public interface Programma {
    void doHelloWorld(String name);
}

/**
*目标类
*/
public class ProgrammaImpl implements Programma{
    public void doHelloWorld(String name){
        System.out.println(name+"do Hello World");
    }
}
```

```
/**
* jdk实现方式
* 代理类，实现InvocationHandler接口
*/
public class MyInvocationHandler implements InvocationHandler {
    /**
    * 被代理的对象
    */
    private Object target;
    
    /**
    * 构造方法
    */
    public MyInvocationHandler(Object target){
        this.target = target ;
    }
    
    /**
    * 执行目标方法
    * proxy:被代理的对象(不用)
    * method：目标对象中的方法对象，jdk传过来
    * args: 是该方法对象的参数
    */
    public Object invoke(Object proxy,Method method,Object[] args) throws Throwable{
        //在目标对象方法前后可以追加功能
        System.out.println("invoke target method before:"+method.getName());//前置通知
        //执行目标对象的方法
        Object result = method.invoke(target,args);  //result是方法的返回值
        System.out.println("invoke target method after:");  //后置通知
        return result;
    }
}
```

```
/**
*  测试类
*/
public class TestInvocationHandler {
    
    public static void main(String[] args){
    	//创建目标对象
        Programma programma = new ProgrammaImpl();
        //jdk的动态代理，来实现代理上面的programma对象，并对他里面的doHelloWorld方法进行增强(追加)
        
        /**
        *newProxyInstance方法的三个参数是什么意思？
        *  1.loader:类加载器（类的字节码对象获得）写法是固定的
        *  2.interfaces得到该对象的所有实现接口的字节码对象的数组 写法是固定的
        *  3.需要一个实现了InvocationHandler接口的对象，这步需要自己手动实现，对目标对象方法功能的增强         *    就是在这个对象里面完成的
        */
        Programma programmaProxy = (Programma)Proxy.newProxyInstance(programma.getClass().getClassLoader(),programma.getClass().getInterfaces(),new MyInvocationHandler(programma));
        //使用代理对象执行目标方法
        programmaProxy.doHelloWorld("zxz");
    }
}
```

### 15.CGlib动态代理

```
<dependencies>
	<dependency>
		<groupId>org.ow2.asm</groupId>
		<artifactId>asm</artifactId>
		<version>5.2</version>
	</dependency>
	<dependency>
		<groupId>cglib</groupId>
		<artifactId>cglib</artifactId>
		<version>3.2.5</version>
	</dependency>
</dependencies>
```

```
/**
* CGlib实现动态代理
* 代理类，实现MethodInterceptor
*/

public class CGlibProxy implements MethodInterceptor{
    /**
    * 目标类
    */
    private Object target;
    
    /**
    *拦截方法
    */
    
    /**
    * object:代理对象
    * method：目标对象中的方法
    * objects: 目标对象方法的参数
    * methodProxy: 代理对象中的代理方法对象
    */
     public Object intercept(Object object,Method method,Object[] objects,MethodProxy methodProxy) throws Throwable{
        System.out.println("invoke target method before");
        //执行被拦截的方法，被增强的方法
        Object invoke = method.invoke(target,objects);
        System.out.println("invoke target method after");
        return invoke;
    }
    
    /**
    * 获取目标类的代理对象
    */
    public Object getCglibProxy(Object target){
        this.target = target;
        Enhancer enhancer = new Enhancer();
        //设置父类，使用指定的父类创建一个子类
        enhancer.setSuperclass(Programma.class);
        //设置回调
        enhancer.setCallback(this);
        //用代理类创建代理对象，并返回
        return enhancer.create();
    }
}
```

```
/**
* 测试类
* cglib动态代理的实现步骤
* 	1.生成字节码对象(空的)Enhancer
*	2.设定这个字节码对象的爹Programma(目标对象的类)
*	3.设置被增强的方法、回调方法、拦截方法
*	4.得到代理对象
*/
public class TestCGlib {
    public static void main(String[] args){
        CGlibProxy cGlibProxy = new CGlibProxy();
        Programma proxy = (Programma) cGlibProxy.getCglibProxy(new ProgrammaImpl());
        proxy.doHelloWorld("zxz");
    }
}
```

### 16、aop:aspectj-autoproxy 流程分析

```

```

