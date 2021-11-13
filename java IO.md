# Java I/O系统

## 1. 输入和输出

### 1 . 1  I n p u t S t r e a m 的类型

```tex
	InputStream 的作用是标志那些从不同起源地产生输入的类。这些起源地包括（每个都有一个相关的InputStream 子类）：
    (1) 字节数组
    (2) String 对象
    (3) 文件
    (4) “管道”，它的工作原理与现实生活中的管道类似：将一些东西置入一端，它们在另一端出来。 
    (5) 一系列其他流，以便我们将其统一收集到单独一个流内。
	(6) 其他起源地，如Internet 连接等。
	除此以外，FilterInputStream 也属于 InputStream 的一种类型，用它可为“破坏器”类提供一个基础类，以便将属性或者有用的接口同输入流连接到一起。
```



## 2. File类

1. java.io.File类：文件和文件目录路径的抽象表示形式，与平台无关
2. File 能新建、删除、重命名文件和目录，但 File 不能访问文件内容本身。如果需要访问文件内容本身，则需要使用输入/输出流。
3. 想要在Java程序中表示一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象，可能没有一个真实存在的文件或目录。
4. File对象可以作为参数传递给流的构造器

### 2.1 目录列表器

#### 	2.1.1	使用回调（策略模式）实现

```java
import java.io.*;
import java.util.regex.*;
import java.util.*;

public class TestIO{
	public static void main(String[] args) {
		File path=new File(".");
		String[] listFiles;
		if (args.length==0) {
			listFiles=path.list();
		}else{
			//list()会为每一个文件调用accept()方法
			listFiles=path.list(new DirFilter(args[0]));
		}
		//按字母顺序排序
		Arrays.sort(listFiles,String.CASE_INSENSITIVE_ORDER);
		for (String item:listFiles){
			System.out.println(item);
		}
	}
}

class DirFilter implements FilenameFilter{
	//FilenameFilter提供策略
	private Pattern pattern;
	public DirFilter(String regex){
		pattern=Pattern.compile(regex);
	}
	public boolean accept(File dir,String name){
		return pattern.matcher(name).matches();
	}
}
```

#### 2.1.2 使用匿名内部类

```java
public class TestIO{
	public static FilenameFilter filter(final String regex){
		//创建匿名内部类
		return new FilenameFilter(){
			private Pattern pattern=Pattern.compile(regex);
			public boolean accept(File dir,String name){
				return pattern.matcher(name).matches();
			}
		};
	}
	public static void main(String[] args) {
		File path=new File(".");
		String[] listFiles;
		if (args.length==0) {
			listFiles=path.list();
		}else{
			//list()会为每一个文件调用accept()方法
			listFiles=path.list(filter(args[0]));
		}
		//按字母顺序排序
		Arrays.sort(listFiles,String.CASE_INSENSITIVE_ORDER);
		for (String item:listFiles){
			System.out.println(item);
		}
	}
}
```

