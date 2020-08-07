什么是并发与并行

并行：指俩个或多个事件在同一个时刻发生（同时发生）
并发：指俩个或多个事件在同一时间段内发生。

什么是程序、进程、线程

```
程序：程序是指令和数据的有序集合，其本身没有任何运行的含义，是一个静态的概念。
进程：执行程序的一次执行过程、它是一个动态的概念。是系统资源分配的单位。一个进程中可以包含若干个线程。
线程：又称轻量级进程。进程中的一条执行路径，也是CPU的基本调度单位。

一个进程由一个或多个线程组成，彼此间完成不同的工作，同时执行，称为多线程。
```

线程的组成

```
1、CPU时间片：os会为每个线程分配执行时间。
2、运行数据：
	堆空间：存储线程需要使用的对象，多个线程可以共享堆中的对象。
	栈空间：存储线程需要使用的局部变量，每个线程拥有独立的栈。
3、线程的逻辑代码
```

线程的特点

```
1、线程抢占式执行
	效率高
	可防止单一线程长时间独占CPU
2、在单核CPU中，宏观上同时执行，微观上顺序执行
```

线程的状态

```

```

线程创建

```
1、继承Thread类
2、实现Runnable接口
3、实现Callable接口
4、线程池
```

获取和修改线程名称

```
1、获取线程ID和线程名称
	在Thread的子类中调用this.getID()或this.getName()
	使用Thread.currentThread().getId()和Thread.currentThread().getName()
2、修改线程名称
	调用线程对象的setName()方法
	使用线程子类的构造方法赋值
```

线程的生命周期

```
1、新建
	new关键字创建了一个线程之后，该线程就处于新建状态
	JVM为线程分配内存，初始化成员变量值
2、就绪
	当线程对象调用了start()方法之后，该线程处于就绪状态
	JVM为线程创建方法栈和程序计数器，等待线程调度器调度
3、运行
	就绪状态的线程获得CPU资源，开始运行run()方法，该进程进入运行状态
4、阻塞
	当发生如下情况时，线程就会进入阻塞状态
		线程调用了sleep()方法主动放弃所占用的处理器资源
		线程调用了一个阻塞式IO方法，在该方法返回之前，该线程被阻塞
		线程试图获得一个同步锁（同步监听器），但该同步锁正被其他线程所持有。
		线程在等待某个通知（notify）
		程序调用了线程的suspend()方法将该线程挂起。但这个方法容易导致死锁，应尽量避免该方法
5、死亡
	线程会以如下3种方式结束，结束后就处于死亡状态：
		run()或call()方法执行完成，线程正常结束。
		线程抛出一个未捕获的Exception或Error.
		调用该线程stop()方法来结束该线程，该方法容易导致死锁，不推荐。
```

继承Thread类

```
/**
*  创建自定义线程类
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

public class MyThread extends Thread{
	@Override
    public void run() {
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
        
		for(int i = 0;i<100;i++) {
			//获取线程id this.getId()和线程名称this.getName()
        	System.out.println("线程id是："+this.getId()+" myThread执行了 "+"线程名称是："+this.getName()+" 当前执行时间:  "+str);
        }
    }
}

```

```
/**
*	创建测试类
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

public class TestMyThread {

	public static void main(String[] args) {
		//1、创建自定义线程实例
		MyThread mythread = new MyThread();
		//2.启动线程
		mythread.start();
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
        
		//3.在main主线程打印信息
		//获得线程id 和线程名称
		//Thread.currentThread().getId()和Thread.currentThread().getName()
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getId()+" "+"main的线程执行了 "+Thread.currentThread().getName()+" "+str);
		}

	}

}
```

实现Runnable接口

```
/**
*自定义类实现Runnable接口
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

public class MyRunnable implements Runnable{

	@Override
	public void run() {
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		        
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
		
	}

}
```

```
/**
* 测试类
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

public class TestMyRunnable {
	public static void main(String[] args) throws InterruptedException{
		
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		//实现runnable接口
		//通过thread类执行myRunnable类
		Thread thread = new Thread(new MyRunnable(),"MyRunnable");
		thread.start();
		//在main主线程打印信息
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
		
	}
}
```

实现Callable接口

```
FutureTask实现RunnableFuture接口
RunnableFuture接口继承Future接口和Runnable接口

Future接口：
判断任务是否完成：isDone()
能够中断任务：cancell()
能够获取任务执行结果：get()
```

```
/**
* 实现接口Callable类
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.Callable;

public class MyCallable implements Callable<String> {

	@Override
	public String call() throws Exception {
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		        
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
		return "MyCallable接口实现完成!";
	}

}

```

```
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Future;
import java.util.concurrent.FutureTask;

public class TestMyCallable {
/**
* 测试类
*/
	public static void main(String[] args) throws Exception{
		//3.实现Callable接口
		//3.1创建FutureTask实例，创建MyCallable实例
		FutureTask<String> task = new FutureTask<String>(new MyCallable());
		//3.2创建Thread实例，执行FutureTask
		Thread thread = new Thread(task,"MyCallable");
		thread.start();
		//3.3在主线程打印信息
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
		//3.4获取并打印MyCallable执行结果
		try {
			String result = task.get();
			System.out.println("MyCallable执行结果是"+result);
		}catch(ExecutionException e) {
			e.printStackTrace();
		}
	}

}

```

线程池-Excutor

```
Executor接口：
	声明了executor(Runnable)方法，执行任务代码
ExecutorService接口：
	继承Executor接口，声明方法：submit、invokeAll、invokeAny以及shutDown等
AbstracExecutorService抽象类：
	实现ExecutorService接口，基本实现ExecutorService中声明的所有方法
ScheduledExecutorService：
    继承ExecutorService接口，声明定时执行任务方法
ThreadPoolExcutor（线程池实现类，实现了AbstracExecutorService）通过这个获取线程：
	继承类AbstracExecutorService，实现execute、submit、shutdown、shutdownNow方法
ScheduleThreadPoolExcutor：
	继承了ThreadPoolExcutor类，实现了ScheduledExecutorService接口并实现其中的方法

Executors(封装类)方便帮我们创建常用的四大线程池。提供快速创建线程池的方法
```

```
/**
*  创建自定义实现
*/
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;

public class MyRunnable implements Runnable{

	@Override
	public void run() {
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		        
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
		
	}

}

```

```
/**
*  创建测试类
*/
```

线程安全问题

```
package com.vg.thread;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;


public class TestExcutor {
	public static void main(String[] args) {
		//4使用线程池创建线程
		//4.1使用Executors获取线程池对象
		ExecutorService executorService = Executors.newFixedThreadPool(10);
		//4.2通过线程池对象获取线程并执行MyRunnable实例
		executorService .execute(new MyRunnable());
		//4.3主线程打印信息
		//通过正则式来设置输出的时间格式
		SimpleDateFormat d = new SimpleDateFormat("yyyy年MM月dd日  HH:mm:ss");
		String str = d.format(new Date());
		        
		for(int i = 0;i<100;i++) {
			System.out.println(Thread.currentThread().getName()+Thread.currentThread().getId()+str);
		}
	}
}

```

实现接口和继承Thread类比较

```
1、接口更适合多个相同的程序代码的线程共享同一个资源。
2、接口可以避免Java的单继承的局限性。
3、接口代码可以被多个线程共享，代码和线程独立。
4、线程池只能放入实现Runnable或Callable接口的线程，不能直接放入继承Thread的类
```

Runnable和Callable接口比较

```
相同点：
	1、都是接口
	2、都可以用来编写多线程
	3、都需要调用Thread.start()启动线程
不同点：
	1、实现Callable接口的线程池能返回
	2、Callable接口的call()方法允许抛出异常，而Runnable接口的run()方法的不允许抛异常
	3、实现Callable接口的线程可以调用Futuer.cancel取消执行，而实现Runnable接口的线程不能
注意点：
	callable接口支持返回执行结果，此时需要调用FutureTask.get()方法实现，此方法会阻塞主线程直到获取‘将来’结果；当不调用此方法时，主线程不会阻塞！
```

线程安全问题

什么是线程安全

```
如果有多个线程同时运行同一个实现了Runnable接口的类，程序每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的；反之，则是线程不安全的。
```

问题演示

```
/**
* 创建售票线程类
*/
package com.vg.thread.demo02;

public class Ticket implements Runnable{
	
	private int ticketNum = 100; 	//电影票数据量，默认100张
	@Override
	public void run() {
		while(true) {
			if(ticketNum>0) {//判断是否有票
				//有票，让线程睡眠1000ms
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				//打印当前售出的票数字和线程名
				String name = Thread.currentThread().getName();
				System.out.println("线程："+name+"销售电影票："+ticketNum);
				//票数减一
				ticketNum--;
				
			}
		}
	}
	
}

```

```
package com.vg.thread.demo02;

public class TicketSaleMain {

	public static void main(String[] args) {
		//1、创建电影票对象
		Ticket ticket = new Ticket();
		//2、创建Thread对象，执行电影票售卖
		Thread thread = new Thread(ticket,"窗口1");
		Thread thread2 = new Thread(ticket,"窗口2");
		Thread thread3 = new Thread(ticket,"窗口3");
		Thread thread4 = new Thread(ticket,"窗口4");
		thread.start();
		thread2.start();
		thread3.start();
		thread4.start();
	}

}

```

```
问题分析：
	线程安全问题都是由全局变量及静态变量引起的；
	若每个线程安全对全局变量、静态变量只读，不写，一般来说，这个变量是安全的；
	若有多个线程同时执行读写操作，一般都需要考虑线程同步，否则的话可能影响线程安全；
可能是由于线程休眠期间，ticketNum没有减，然后造成打出同一个票
线程安全问题根本原因：
	1、多个线程在操作共享的数据
	2、操作共享数据的线程代码有多条
	3、多个线程对共享数据有写操作
```

问题解决——线程同步

```
	要解决以上线程问题，只要在某个线程修改共享资源的时候，其他线程不能修改该资源，等待修改完毕同步之后才能去抢夺CPU资源，完成对应的操作，保证了数据的同步性，解决了线程不安全的现象
	Java引入了七种线程同步机制：
		1、同步代码块
		2、同步方法
		3、同步锁
		4、特殊域变量
		5、局部变量
		6、阻塞队列
		7、原子变量
```

```
/**
* 加一个同步代码块，synchronized
*/
package com.vg.thread.demo02;

public class Ticket implements Runnable{
	
	private int ticketNum = 100; 	//电影票数据量，默认100张
	//什么类型都可以
	private Object obj = new  Object();
	@Override
	public void run() {
		while(true) {
			synchronized(obj) {
				if(ticketNum>0) {//判断是否有票
					//有票，让线程睡眠1000ms
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					//打印当前售出的票数字和线程名
					String name = Thread.currentThread().getName();
					System.out.println("线程："+name+"销售电影票："+ticketNum);
					//票数减一
					ticketNum--;
					
				}
			}
			
		}
	}
	
}
```

```
/**
* 加一个同步方法（使用synchronized修饰的方法就叫做同步方法，保证A线程执行该方法的时候，其他线程只能在方法外等待）
* 同步锁是谁？
*1、对于非static方法同步锁就是this 2、对于static()方法，同步锁就是当前方法所在类的字节码对象(类名.class)
*/
package com.vg.thread.demo02;

public class Ticket implements Runnable{
	
	private int ticketNum = 100; 	//电影票数据量，默认100张
	//什么类型都可以
	private Object obj = new  Object();
	@Override
	public void run() {
		while(true) {
			saleTickt();
		}
	}
	//synchronized加到的方法不是静态方法，这个锁对象调用当前方法的实例，这个实例就相当于上面的obj
	//是静态方法，当前方法所在的这个类的位置上，这里如果加静态就是Ticket等于obj
	private  synchronized void saleTickt() {
		if(ticketNum>0) {//判断是否有票
			//有票，让线程睡眠1000ms
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			//打印当前售出的票数字和线程名
			String name = Thread.currentThread().getName();
			System.out.println("线程："+name+"销售电影票："+ticketNum);
			//票数减一
			ticketNum--;
			
		}
	}
}

```

```
/**
*java.util.concurrent.locks.Lock机制提供了比synchronized代码块和synchronized方法更广泛的锁定操作，同步代码块/同步方法具有的功能Lock都有，除此之外更强大，更体现面向对象
*同步锁方法
*1、public void lock()：加同步锁
*2、public void unlock()：释放同步锁
*/

package com.vg.thread.demo02;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Ticket implements Runnable{
	
	private int ticketNum = 100; 	//电影票数据量，默认100张
	//什么类型都可以
	private Object obj = new  Object();
	
	private Lock lock = new ReentrantLock(true);//这个参数是否 true-公平锁，多个线程都公平拥有执行权
	@Override
	public void run() {
		while(true) {
			lock.lock();
			try {
				if(ticketNum>0) {//判断是否有票
					//有票，让线程睡眠1000ms
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					//打印当前售出的票数字和线程名
					String name = Thread.currentThread().getName();
					System.out.println("线程："+name+"销售电影票："+ticketNum);
					//票数减一
					ticketNum--;
					
				}
			}finally {
				lock.unlock();
			}
			
		}
	}
	
}

```

synchronized和Lock区别

```
1、synchronized是Java内置关键字，在jvm层面，Lock是个Java类；
2、synchronized无法判断是否获取锁的状态，Lock可以判断是否获取到锁；
3、synchronized会自动释放锁(a线程执行完同步代码释放锁；b代码执行过程中发生异常会释放锁），Lock需在finally中手动释放锁（unlock()释放锁），否则容易造成线程死锁
4、用synchronized关键字的俩个线程1和线程2，如果当前线程1获得锁，线程2线程等待。如果线程1阻塞，线程2则会一直等待下去，而Lock锁就不一定会等待下去，如果尝试获取不到锁，线程可以不用一直等待就结束了；
5、synchronized的锁可重入、不可中断、非公平，而Lock锁可重入、可判断、可公平(俩者皆可)
6、Lock锁适合大量同步的代码的同步问题，synchronized锁适合代码少量的同步问题
```

线程死锁

什么是线程死锁

```
	多线程以及多线程改善了系统资源的利用率并提高了系统的处理能力。然而，并发执行也带来了新的问题--死锁。所谓死锁是指多个线程因竞争资源而造成的一种僵局(互相等待)，若无外力支持，这些进程都将无法向前推进。
```

死锁产生的必要条件                                                                                                                                                                          

```
1、互斥条件
	进程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内某资源仅为一个进程所占有。此时若有其他进程请求该资源，则请求进程只能等待。
2、不可剥夺条件
	进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能由获得该资源的进程自己来释放（只能主动释放）
3、请求与保持条件
	进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源请求已被其他资源占有，此时请求进程被阻塞，但对自己已获得的资源保持不放
4、循环等待条件
	存在一种进程资源的循环等待链，链中每一个进程已获得的资源同时被链中下一个进程所请求。即存在一个处于等待状态的进程集合{P1，P2，···,Pn}，其中Pi等待的资源被Pi+1占有，Pn等待的资源被P0占有

```

死锁代码演示

```

```

死锁处理

```
1、预防死锁：通过设置某些限制条件，去破坏产生死锁的四个必要条件中的一个或几个条件，来防止死锁的发生
2、避免死锁：在资源的动态分配过程中，用某种方法去防止系统进入不安全状态，从而避免死锁的发生
3、检测死锁：允许系统在运行过程中发生死锁，但可设置检测机构及时检测死锁的发生，并采取适当措施加以清除。
4、解除死锁：当检测出死锁后，便采取适当措施将进程从死锁状态中解脱出来。
```

死锁预防

```
1、破坏“互斥”条件
	“互斥”条件是无法破坏的。因此，在死锁预防里主要是破坏其他几个必要条件，而不去涉及破坏“互斥”条件。
2、破坏“占有并等待”条件
	破坏“占有并等待”条件，申请其他资源。
	方法一：一次性分配资源，即创建进程时，要求它申请所需的全部资源，系统或满足其所有要求，或什么也不给它。
	方法二：要求每个进程提出新的资源申请前，释放其占有的资源。这样，一个进程在需要资源S时，须先把它先前占有的资源R释放掉，然后才能提出对S的申请，即使它可能很快又要用到R资源。
3、破坏“ 不可抢占”条件
	破坏“ 不可抢占”条件就是允许对资源进行抢夺。
	方法一：如果占有某些资源的一个进程进行进一步资源请求被拒绝，则该进程必须释放它最初占有的资源，如果有必要，可再次请求这些资源和另外的资源。
	方法二：如果一个进程请求当前被另一个进程占有的一个资源，则操作系统可以抢占另一个进程，要求它释放资源。只有在任意俩个进程的优先级都不相同的情况下，方法二才能预防死锁。
4、破坏“循环等待”条件
	破坏“循环等待”条件的一种方法，是将系统中所有资源统一编号，进程可在任何时候提出资源申请，但所有申请必须按照资源的编号顺序（升序）提出。这样子就能保证系统不出现死锁。
```

