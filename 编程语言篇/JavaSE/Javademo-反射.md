java反射机制API

```
java.lang.Class类和java.lang.reflect包

Java语言提供了反射机制，通过反射机制能够动态读取一个类的信息；能够在运行时动态加载类，而不是在编译期

反射可以应用于框架开发，它能够从配置文件中读取配置信息动态加载类、创建对象，以及调用方法和成员变量。
```

```
package com.javase.reflect.demo01;
/**
* Person类
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:58:57
*/
public class Person {
	//构造方法
	public Person() {
	
	}
	
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	//成员边量
	private String name;
	private int age;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
	public void eat() {
		System.out.println("吃饭：。。。。。");
	}
	@Override
	public String toString() {
		return "Person ";
	}
}

```

```
package com.javase.reflect.demo01;
/**
* Person2
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:58:57
*/
public class Person2 {
	//构造方法
	public Person2() {
	
	}
	
	public Person2(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	//成员边量
	private String name;
	private int age;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
	public void eat() {
		System.out.println("吃饭：。。。。。");
	}

	@Override
	public String toString() {
		return "Person2 ";
	}
	
}

```

```
package com.javase.reflect.demo01;
/**
 * 在一次程序的运行过程中，通过同一个类创建的对象得到的字节码文件对象是同一个
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:59:15
*/

public class Test {
	public static void main(String[] args) throws Exception {
		Person p = new Person();
		Person p2 = new Person();
		//Object getClass()
		
		Class class1 = p.getClass(); 
		Class class2 = p.getClass(); 
		Class class3 = p2.getClass(); 
		System.out.println(class1); 
		System.out.println(class2);
		System.out.println(class1==class2); 
		System.out.println(class3);
		
		
		//类型。class
		Class class4 = Person.class;
		System.out.println(class4==class2);
		//类型: 引用数据类型还可以是基本数据类型
		Class class5 = int.class;
		
		//Class.forName("类型地址（大名）")
		Class class6=Class.forName("com.javase.reflect.demo01.Person");
		System.out.println(class5==class6);
		System.out.println(class4==class6);
	}
}

```

```
package com.javase.reflect.demo01;

import java.io.BufferedReader;
import java.io.FileReader;
import java.lang.reflect.Constructor;

/**
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午5:15:32
*/
public class Test2 {

	public static void main(String[] args) throws Exception {
		
		//读取配置文件中的类的信息
		FileReader f = new FileReader("aa.txt");
		BufferedReader br = new BufferedReader(f);
		String str = br.readLine();
		System.out.println(str);
		Person p = new Person();
		// 用字节码文件对象创建一个类的对象
		Class clazz = Class.forName(str);
		// 得到字节码文件对象的构造器对象
		Constructor[] constructors = clazz.getConstructors();
		System.out.println(constructors.length);
		System.out.println(constructors[0]);
		System.out.println(constructors[1]);
		Constructor c=constructors[0];
		//用构造器对象常见类的对象
		Object obj = c.newInstance();
		System.out.println(obj);
		//Person2 p2 = (Person2) obj;
		Person p2 = (Person) obj;
		p2.eat();
		p.eat();
	}

}

```

demo02

```
package com.javase.reflect.demo02;
/**
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:58:57
*/
public class Person {
	//构造方法
	protected Person() {
	
	}
	
	public Person(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	 Person(String name) {
		super();
		this.name = name;
	}

	private Person(int age) {
		super();
		this.age = age;
	}
	

	//成员边量
	private String name;
	private int age;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	
	public void eat() {
		System.out.println("吃饭：。。。。。");
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}

	private String method(String name) {
		System.out.println(name);
		return "method";
	}
	public String method1(String name) {
		System.out.println(name);
		return "method";
	}
	
}

```

```
package com.javase.reflect.demo02;

import java.io.BufferedReader;
import java.io.FileReader;
import java.lang.reflect.Constructor;

/**
 * 得到字节码文件中的构造对象
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:59:15
*/

public class Test {
	public static void main(String[] args) throws Exception {
		//读取配置文件中的类的信息
		FileReader f = new FileReader("demo02.txt");
		BufferedReader br = new BufferedReader(f);
		String str = br.readLine();
		//1.得到字节码文件对象
		Class clazz = Class.forName(str);
		//2.得到字节码文件对象中的构造器对象
		//getConstructors() 得到公共的构造器
		//getDeclaredConstructors() 得到所有的构造器
		//Constructor[] constructors = clazz.getDeclaredConstructors();
		
		//得到指定构造器
		/*Constructor constructor = clazz.getConstructor(String.class,int.class);
		System.out.println(constructor);
		Object newInstance = constructor.newInstance("vg",21);
		System.out.println(newInstance);*/
		
		
		/*Constructor constructor = clazz.getDeclaredConstructor(String.class);
		Object newInstance = constructor.newInstance("vg");
		System.out.println(newInstance);*/
		
		Constructor constructor = clazz.getDeclaredConstructor(int.class);
		//私有的构造器可以得到，但是不能创建对象’
		//暴力访问
		constructor.setAccessible(true);
		Object newInstance = constructor.newInstance(21);
		System.out.println(newInstance);
	}
}

```

```
package com.javase.reflect.demo02;

import java.io.BufferedReader;
import java.io.FileReader;
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

/**
 * 得到字节码文件中的方法对象
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:59:15
*/

public class Test2 {
	public static void main(String[] args) throws Exception {
		//读取配置文件中的类的信息
		FileReader f = new FileReader("demo02.txt");
		BufferedReader br = new BufferedReader(f);
		String str = br.readLine();
		//1.得到字节码文件对象
		Class clazz = Class.forName(str);
		//2.得到字节码文件对象中的所有method对象
		//不带Declared得到所有public 修饰的method 包括从父类继承过来的public 的method方法
		//Method[] methods = clazz.getMethods();
		//带Declared的得到当前类中的所有方法  
		
		/*Method[] methods = clazz.getDeclaredMethods();
		for(Method method:methods) {
			System.out.println(method);
		}*/
		
		//得到特定的方法
		Method method = clazz.getDeclaredMethod("method",String.class);
		System.out.println(method);
		//调用方法
		//传统调用方法：对象。方法（实参） 反射的调用方法：方法。invoke（对象，实参）
		//创建方法所在类的对象
		
		/*Constructor constructor = clazz.getConstructor();
		Object newInstance = constructor.newInstance();*/
		
		//字节码文件对象为我们提功了一个便捷创建类对象的方法
		Object newInstance = clazz.newInstance();//底层依然调用类的无参构造器创建对象
		//反射调用
		method.setAccessible(true);
		//Object rst = method。invoke(newInstance);
		Object rst = method.invoke(newInstance,"haha");
		System.out.println(rst);
	}
}

```

```
package com.javase.reflect.demo02;

import java.io.BufferedReader;
import java.io.FileReader;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

/**
 * 得到字节码文件中的Field
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月29日下午3:59:15
*/

public class Test3 {
	public static void main(String[] args) throws Exception {
		//读取配置文件中的类的信息
		FileReader f = new FileReader("demo02.txt");
		BufferedReader br = new BufferedReader(f);
		String str = br.readLine();
		//1.得到字节码文件对象
		Class clazz = Class.forName(str);
		//得到指定的field
		Field name = clazz.getDeclaredField("name");
		name.setAccessible(true);
		//传统的属性调用方式 对象。属性名   属性对象。setXXX(对象，"value")
		Object obj = clazz.newInstance();
		name.set(obj,"vg");
		System.out.println(obj);
	}
}

```

