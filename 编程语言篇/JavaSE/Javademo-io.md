```
package com.vg.file.demo01;

import java.io.File;

/**
*@author 中二少年阿基 vg
*/
public abstract class FileTest {

	public static void main(String[] args) {
		// File(String pathname):根据一个路径得到File对象
		//java中一个 "\" 代表的是转义字符开始的标志,你要是想用"\"，就必须用俩个"\"表示
		File file1 = new File("J:\\a\\b.txt");
		
		//File(String parent,String child):根据一个目录和一个子文件/目录得到File对象
		//java中认为文件夹是一种特殊的文件，只不过文件夹中的内容是其他文件或文件夹，而文件中是数据
		File file2 = new File("J:\\a","b.txt");
		
		//File(File parent,String child):根据一个父File对象和一个子文件/目录得到file对象
		File file = new File("J:\\a");
		File file3 = new File(file,"b.txt");
	}

}
```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;

/**
*@author 中二少年阿基 vg
*/
public abstract class FileTest2 {

	public static void main(String[] args) throws Exception {
		//public boolean createNewFile():创建文件 如果文件存在这样子的文件就不创建了
		File file = new File("J:\\a\\c.txt");
		boolean rst = file.createNewFile();
		System.out.println(rst);
		//如果没有指明创建文件的路径，那么该文件是在项目路径下创建
		File file2 = new File("d.txt");
		boolean rst2 = file2.createNewFile();
		System.out.println(rst2);
		
		//如果路径不存在能不能创建? 不能，java.io.IOException:系统找不到指定路径
		//如果调用该createNewFile方法的时候路径必须存在
		File file3 = new File("J:\\aa\\c.txt");
		boolean rst3 = file3.createNewFile();
		System.out.println(rst3);
	}

}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;

/**
*@author 中二少年阿基 vg
*/
public abstract class FileTest3 {

	public static void main(String[] args) throws Exception {
		//public Boolean mkdir():创建文件夹如果存在这样的文件夹，就不创建了 make dirctory
		//该方法不能创建多层文件夹
		File file = new File("J:\\b");
		boolean rst = file.mkdir();
		System.out.println(rst);
		
		File file2 = new File("J:\\c\\d");
		boolean rst1 = file2.mkdir();
		System.out.println(rst1);
		//public Boolean mkdirs():创建文件夹，如果父文件不存在，会帮你创建出来
		//他是用来创建多层文件夹
		boolean rst3 = file2.mkdirs();
		System.out.println(rst3);
		
		File file3 = new File("bbbb2");
		file.mkdir();
		file3.createNewFile();//创建的是文件只不过这个文件没有后缀名
		//file3.mkdirs();
	}

}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;

/**
*@author 中二少年阿基 vg
*/
public abstract class FileTest4 {

	public static void main(String[] args) throws Exception {
		//删除功能
		//public boolean delete():可以用来删除文件和文件夹
		File file =new File("bbb2");
		boolean delete = file.delete();
		File file2 =new File("bbbb2");
		file2.delete();
		
		System.out.println(delete);
		//c下面还有文件夹没用删除成功
		//注意：delete删除的文件夹下面不能有文件或文件夹
		File file3 = new File("J:\\c");
		//File file3 = new File("J:\\c\\d");
		boolean delete2 = file3.delete();
		System.out.println(delete2);
		
		File file5 = new File("J:\\a\\b.txt");
		//File file3 = new File("J:\\c\\d");
		boolean delete3 = file5.delete();
		System.out.println(delete3);
		
	}

}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;

/**
*@author 中二少年阿基 vg
*/
public abstract class FileTest5 {

	public static void main(String[] args) throws Exception {
		//重命名功能
		//总结：如果File改名的路径是在同一个文件夹下就是改名
		//如果File改名的路径不在同一个文件夹下就是剪贴
		File file = new File("a");
		File file2 = new File("b");
		File file3 = new File("c.txt");
		
		file.mkdir();
		file2.mkdir();
		file3.createNewFile();
		
		//把a和b改成aa和bb
		File file4 = new File("aa");
		file.renameTo(file4);
		File file5 = new File("bb");
		file2.renameTo(file5);
		file3.renameTo(new File("cc.txt"));
		//移动文件
		File file6 = new File("cc.txt");
		file6.renameTo(new File("J:\\a\\c.txt"));
	}
}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;

/**
 * 判断功能
*@author 中二少年阿基 vg
*/
public abstract class FileTest6 {

	public static void main(String[] args) throws Exception {
		File file = new File("a");
		File file2 = new File("b");
		File file3 = new File("c.txt");
		//public boolean isDirectory(): 判断是否是目录
		boolean rst = file.isDirectory();
		System.out.println(rst);
		//public boolean isFile(): 判断是否是文件
		 rst = file3.isFile();
		System.out.println(rst);
		//public boolean exists(): 判断是否存在
		rst  = file.exists();
		System.out.println(rst);
		//public boolean canRead():判断是否可读
		rst = file3.canRead();
		System.out.println(rst);
		//public boolean canWrite(): 判断是否可写
		rst=file3.canWrite();
		System.out.println(rst);
		//public boolean isHidden():判断是否隐藏
		rst=file3.isHidden();
		System.out.println(rst);
	}
}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;
import java.util.Date;

/**
 * 判断功能
*@author 中二少年阿基 vg
*/
public abstract class FileTest7 {

	public static void main(String[] args) throws Exception {
		//下面的是相对路径
		File file = new File("aa");
		File file2 = new File("bb");
		File file3 = new File("c.txt");
		File file6 = new File("cc.txt");
		//下面的是绝对路径，从磁盘的盘符位置开始
		File file4=new File("J:\\a\\c.txt");
		//public String getAbsolutePath():获取绝对路径
		String absolutePath = file.getAbsolutePath();
		System.out.println(absolutePath);
		System.out.println(file3.getAbsolutePath());
		//public String getPath():获取相对路径
		String path = file.getPath();
		System.out.println(path);
		System.out.println(file2.getPath());
		File file5  = new File("aa\\c.txt");
		System.out.println(file5.getPath());
		//public String getName():获取名称
		//不带路径只有文件或者文件夹的名称
		System.out.println(file.getName());
		System.out.println(file4.getName());
		//public long length():获取长度  字节数
		
		System.out.println(file6.length());
		System.out.println(file3.length());
		//文件夹的长度总是0
		System.out.println(file.length());
		//public long lastModified():获取最后一次的修改时间，毫秒值
		long time = file3.lastModified();
		Date d =new Date(time);
		System.out.println(d.toLocaleString());
	}
}

```

```
package com.vg.file.demo01;

import java.io.File;
import java.io.IOException;
import java.util.Date;

/**
 * 判断功能
*@author 中二少年阿基 vg
*/
public abstract class FileTest8 {

	public static void main(String[] args) throws Exception {
		
		
		File file = new File("I:\\02pdf文件");
		//public String[] list():获取指定目录下的所有文件和目录的文件名或者文件夹的名称相同返回的是String数组
//		String[] names = file.list();
//		for(String string:names) {
//			System.out.println(string);
//		}
		
		//public File[] listFiles():获取指定目录下的所有文件和目录的绝对路径或者返回文件夹的File数组
		File[] names = file.listFiles();
		for (File file2 : names) {
			System.out.println(file2.getAbsolutePath());
		}
	}
}

```

```
package com.vg.file.demo02;

import java.io.File;

/**
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月30日下午9:23:51
*/
public class Test {

	public static void main(String[] args) {
		//判断d:\javase\io流 目录下是否有后缀名为。png的文件，如果有，就输出此文件的绝对路径！
		
		//分析:封装对象、得到该目录下的所有文件名称、判断每个文件名是否以.png结尾，如果是就打印出来
		File file = new File("J:\\");
		
		//得到该路面下所有的文件名称
		String[] names=file.list();
		File[] listFiles = file.listFiles();
		//判断每个文件名称是否以。png结尾
		for (String string : names) {
			if(string.endsWith(".png")) {
				System.out.println(string);
			}
		}
		
		for (File file2 : listFiles) {
			String name = file2.getName();
			if(name.endsWith(".png")) {
				String path = file2.getAbsolutePath();
				System.out.println(path);
			}
		}
	}

}

```

```
package com.vg.file.demo02;

import java.io.File;

/**
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月30日下午9:46:03
*/
public class Test2 {

	public static void main(String[] args) {
		// 批量改文件的名字，把文件夹中类似于文件名，
		//"批量改名字的名字，把文件夹中类似于文件名"
		//分析:
		//得到所有文件的对象(把所有要改名字的文件编程File对象)
		//1。得到文件所在的目录
		//2.通过对这个目录进行File对象的包装，得到他下面所有的File对象  listFiles
		//遍历所有的文件对象，在遍历的过程中对文件进行改名 renameTo
		//改名字的步骤：得到原来文件的名字、对原来文件中的名字进行提取substring 截取之后拼接成新的名字  利用原来该文件所在的文件路径+新名字，构建一个新的File对象
		//原来的文件对象。renameTo(构造新的File对象)
		
		//文件所在目录
		File file =new File("J:\\b");
		//得到所有文件对象
		File[] files = file.listFiles();
		
		//遍历files，在遍历过程中改名
		for(File file2:files) {
			//得到文件名
			String name = file2.getName();
			//System.out.println(name);
			int start_ = name.indexOf("_");
			String first = name.substring(start_+1, start_+4);
			int end_ = name.lastIndexOf("_");
			String last = name.substring(end_+1);
			//新文件名字
			String newName = first+"_"+last;
			//对file2进行重名，就要创建一个新的File对象
			//File newFile = new File(newName);
			File newFile = new File(file,newName);
			file2.renameTo(newFile);
		}
		
		
	}

}

```

```
package com.vg.javase.output.demo;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.util.Arrays;

/**
 * 需求：通过io流往文件中写入一句话"hello io"
*@author 中二少年阿基 vg
*@date   创建事件: 2020年7月31日上午10:44:46
*/

//实现步骤 1、创建流对象（输出管道）
		//2、把数据变成字节数据
		//3、用管道传输数据到文件
public class FileOutPutStreamDemo01 {

	public static void main(String[] args) throws Exception {
		//FileOutputStream(File file) 创建一个向指定File对象表示的文件中写入数据的输出文件夹
		//FileOutputStream(String name) 创建一个向具有指定名称的文件中写入数据的输出文件夹
		FileOutputStream fos = new FileOutputStream("c.txt");
		//上面代码干了俩件事情:1、创建管道 2、把管道兑到了c.txt文件
		
		//数据
		String data = "hello IO";
		//把字符数据转换成字节数据
		//byte[] getBytes() 使用默认字符集将此String编码为byte序列
		//并将结果存储到一个新的byte数组中
		byte[] bytes = data.getBytes();
		//System.out.println(Arrays.toString(bytes));
		//通过管道把数据写进文件
		fos.write(bytes);
		//回收 关闭管道
		fos.close();
	}

}

```

