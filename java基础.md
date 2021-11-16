# Java 的基本程序设计结构

## 1 . java 应用程序的基本结构

```java
public class TestBase{
	public static void main(String[] args) {
		System.out.println("java");
	}
}
```

1. 关键字 public 称为访问修饰符（access modifier), 这些修饰符用于控制程序的其他部分对这段代码的访问级別。
2. 关键字 class 表明 Java 程序中的全部内容都包含在类中。 这里， 只需要将类作为一个加载程序逻辑的容器， 程序逻辑定义了应用程序的行为。Java 应用程序中的全部内容都必须放置在类中。
3. 关键字 class 后面紧跟类名。 Java 中定义类名的规则很宽松。 名字必须以字母开头， 后面可以跟字母和数字的任意组合。长度基本上没有限制。但是不能使用 Java 保留字（例如，public 或 class) 作为类名 。
4. 标准的命名规范为（类名 TestBase 就遵循了这个规范）：类名是以大写字母开头的名词。 如果名字由多个单词组成， 每个单词的第一个字母都应该大写（这种在一个单词中间使用大写字母的方式称为驼峰命名法。以其自身为例， 应该写成 CamelCase)。
5. 源代码的文件名必须与公共类的名字相同，并用.java 作为扩展名。因此，存储这段源代码的文件名必须为 TestBase.java。
6. 为了代码能够执行， 在类的源文件中必须包含一个 main方法。当然，也可以将用户自定义的方法添加到类中，并且在 main 方法中调用它们；根据 Java 语言规范， main 方法必须声明为 public。
7. Java 中任何方法的代码都用“ {” 开始，用”}“结束。
8. Java 中的所有函數都属于某个类的方法。 因此， Java 中的 main 方法必须有一个外壳类。Java 中的 main 方法必须是静态的。与 C/C++—
   样， 关键字 void 表示这个方法没有返回值， 所不同的是 main 方法没有为操作系统返回“ 退出代码” 。 如果 main 方法正常退出， 那么 Java 应用程序的退出代码为 0,表示成功地运行了程序。 如果希望在终止程序时返回其他的代码， 那就需要调用 System.exit 方法。
9. 在 Java 中， 每个句子必须用分号结束。特别需要说明，回车不是语句的结束标志，因此， 如果需要可以将一条语句写在多行上。
10. 点号（ • ）用于调用方法。Java 使用的通用语法是object.method(parameters)，这等价于函数调用。
11. 在这个示例中， 调用了 println 方法并传递给它一个字符串参数。 这个方法将传递给它的字符串参数显示在控制台上。System.out 还有一个 print 方法， 它在输出之后不换行。

## 2. 注 释

```java
/**
 * 文档注释
	@author senbin 指定java程序的作者
	@version 0.1  指定源文件的版本
 */

public class HelloWorld{
	//Java应用程序的执行入口是main()方法
	public static void main(String[] args){
		/* 多行注释
			输出 hello world
		 */
		System.out.println("Hello World！");  //单行注释

	}
}

/*
注释内容可以被JDK提供的工具 javadoc 所解析，生成一套以网页文件形
式体现的该程序的说明文档。
	javadoc -encoding utf-8 -d mydoc -author -version  HelloWorld.java

*/
	//一个源文件中最多只能有一个public类。其它类的个数不限
```

1. 最常用的方式是使用 //，其注释内容从 // 开始到本行结尾。
2. 也可以使用 /* 和 */ 将一段比较长的注释括起来。在 Java 中，/* */ 注释不能嵌套 „ 也就是说， 不能简单地把代码用 /* 和 */ 括起来作为注释， 因为这段代码本身可能也包含一个 */ ,
3. 第 3 种注释可以用来自动地生成文档。这种注释以 /** 开始， 以 */ 结束。

## 3. 数 据 类 型

Java 是 -种强类型语言。这就意味着必须为每一个变量声明一种类型: 在 Java 中， -共有 8种基本类型 （ primitive type), 其中有 4 种整型、2 种浮点类型、 1 种用于表示 Unicode 编码的字符单元的字符类型 char和 1 种用于表示真值的 boolean 类型。

### 3.1整型

整型用于表示没有小数部分的数值， 它允许是负数。Java 提供了 4 种整型。在通常情况下， int 类型最常用。byte 和 short 类型主要用于特定的应用场合， 例如， 底层的文件处理或者需要控制占用存储空间量的大数组。

| byte  | 1 字节 | -128 ~ 127                                                |
| :---- | ------ | --------------------------------------------------------- |
| short | 2 字节 | -32 768 - 32 767                                          |
| int   | 4字节  | -2 147 483 648 - 2 147 483 647 (正好超过 20 亿)           |
| long  | 8 字节 | -9 223 372 036 854 775 808  ~ - 9 223 372 036 854 775 807 |

1. 在 Java 中， 整型的范围与运行 Java 代码的机器无关。由于 Java 程序必须保证在所有机器 L都能够得到相同的运行结果， 所以各种数据类型的取值范围必须固定。
2. 长整型数值有一个后缀 L 或 1 ( 如 4000000000L )。 十六进制数值有一个前缀 Ox 或 0X ( 如OxCAFEL 八进制有一个前缀 0 , 例如， 010 对应八进制中的 8。 很显然， 八进制表示法比较容易混淆， 所以建议最好不要使用八进制常数。
3. 从 Java 7 开始， 加上前缀 0b 或 0B 就可以写二进制数。 例如，0b1001就是 9。另外，同样是从 Java 7 开始，还可以为数字字面量加下划线， 如用 1_000_000(或0b1111_0100_00丨0_0丨00_0000 )表示一百万。这些下划线只是为丫让人更易读。Java 编译器会去除这些下划线.
4. Java 没有任何无符号（unsigned) 形式的 int、 long、short 或 byte 类型。

### 3.2  浮点类型

浮点类型用于表示有小数部分的数值。在 Java 中有两种浮点类型

| float  | 4 字节 | 大约 ± 3.402 823 47E+38F (有效位数为 6 ~ 7 位）         |
| ------ | ------ | ------------------------------------------------------- |
| double | 8字节  | 大约 ± 1.797 693 134 862 315 70E+308 (有效位数为 15 位> |

1. double 表示这种类型的数值精度是 float 类型的两倍（有人称之为双精度数值)。绝大部分应用程序都采用 double 类型。
2. float 类型的数值有一个后缀 F 或 f (例如， 3.14F)。没有后缀 F 的浮点数值（如 3.14 ) 默认为 double 类型。当然，也可以在浮点数值后面添加后缀 D 或 d ( 例如，3.14D)。
3. 所有的浮点数值计算都遵循 IEEE 754 规范。具体来说，下面是用于表示溢出和出错情况的三个特殊的浮点数值：
   •正无穷大
   •负无穷大
   •NaN (不是一个数字）
   例如， 一正整数除以 0 的结果为正无穷大。计算 0/0 或者负数的平方根结果为 NaN。
4. 常量 Double_POSITIVE_INFINITY、 Double.NEGATIVEJNFINITY 和 Double.NaN( 以及相应的 Float 类型的常量） 分别表示这三个特殊的值， 但在实际应用中很少遇到。

### 3.3 char 类型

1. char 类型原本用于表示单个字符。如今， 有些 Unicode字符可以用一个 chai•值描述， 另外一些 Unicode 字符则需要两个 char 值。
2. char 类型的字面量值要用单引号括起来。 例如： W 是编码值为 65 所对应的字符常量。它与 "A" 不同，"A" 是包含一个字符 A 的字符串.
3. char 类型的值可以表示为十六进制值， 其范围从 \u0000 到 \Uffff。
4. 除了转义序列 \u 之外， 还有一些用于表示特殊字符的转义序列，所有这些转义序列都可以出现在加引号的字符字面量或字符串中。转
   义序列 \u 还可以出现在加引号的字符常量或字符串之外（而其他所有转义序列不可以）。如 \u005B 和 \u005D 是 [ 和 ] 的编码。

```java
public static void main(String\u005B\u00SD args)
```

 **Unicode** 转义序列会在解析代码之前得到处理。 更隐秘地， 一定要当心注释中的 \u。 注释// \u00A0 is a newline会产生一个语法错误， 因为读程序时 u00A0 会替换为一个换行符。

### 3.4 Unicode 和 char 类型

1.  码点 （ code point ) 是指与一个编码表中的某个字符对应的代码值。 在 Unicode 标准中，码点采用十六进制书写，并加上前缀 U+, 例如 U+0041 就是拉丁字母 A 的码点。
2. UTF-16 编码采用不同长度的编码表示所有 Unicode 码点。在基本的多语言级别中， 每个字符用 16 位表示，通常被称为代码单元（code unit); 而辅助字符采用一对连续的代码单元进行编码。 这样构成的编码值落人基本的多语言级别中空闲的 2048 字节内， 通常被称为替代区域（surrogate area) 。
3. 在 Java 中，char 类型描述了 UTF-16 编码中的一个代码单元。

### 3.5 boolean 类型

boolean (布尔）类型有两个值： false 和 true, 用来判定逻辑条件整型值和布尔值之间不能进行相互转换。

## 4. 变量

```tex
   1. 在 Java 中， 每个变量都有一个类型 （ type)。 在声明变量时， 变量的类型位于变量名之前。
   2. 如果想要知道哪些 Unicode 字符属于 Java 中的“ 字母”， 可以使用 Character 类的isJavaldentifierStart 和 isJavaldentifierPart 方法来检查。
```

###    4.1变量初始化

1. 声明一个变量之后，必须用赋值语句对变量进行显式初始化， 千万不要使用未初始化的变量。

2. 在 Java 中， 变量的声明尽可能地靠近变量第一次使用的地方， 这是一种良好的程序编写风格。

3. 在 Java 中， 不区分变量的声明与定义。

   ```java
   int i;
   i=100;
   double d=23.2;
   final int NUMBER==100;
   ```

   

### 4.2 常量

1. 在 Java 中， 利用关键字 final 指示常量。
2. 关键字 final 表示这个变量只能被赋值一次。一旦被赋值之后， 就不能够再更改了。习惯上,常量名使用全大写。
3. 在 Java 中，经常希望某个常量可以在一个类中的多个方法中使用，通常将这些常量称为类常量。可以使用关键字 static fina丨设置一个类常量。
4. 需要注意， 类常量的定义位于 main 方法的外部。 因此， 在同一个类的其他方法中也可以使用这个常量。 而且， 如果一个常量被声明为 public， 那么其他类的方法也可以使用这个常。 
5. const 是 Java 保留的关键字， 但目前并没有使用。 在 Java 中， 必须使用 final定义常量。

```java
public class TestBase{
	public static final Double PI=3.14;
	public static void main(String[] args) {
		Double r=5.5;
		System.out.println("Areas:"+PI*r*r);
	}
}
```

## 5. 运 算 符

```java
		Double r=5.5;
		int i=10;
		System.out.println(i/0);
		//Exception in thread "main" java.lang.ArithmeticException: / by zero
		System.out.println(r/0);  //Infinity
```



1.  整数被 0 除将会产生一个异常， 而浮点数被 0 除将会得到无穷大或 NaN 结果。

2. 对于使用 strictfp关键字标记的方法必须使用严格的浮点计算来生成可再生的结果。 例如， 可以把 main 方法标记为

   ```java
   public static strictfp void main(String[] args)
   ```

   于是， 在 main 方法中的所有指令都将使用严格的浮点计算。如果将一个类标记为strictfp, 这个类中的所有方法都要使用严格的浮点计算。

### 5.1 数学函数与常量

1. 不必在数学方法名和常量名前添加前缀“Math”， 只要在源文件的顶部加上下面这行代码就可以了。

   ```java
   import static java.lang.Math.*;
   
   public class TestBase{
   	public static void main(String[] args) {
   		double x=5;
   		int i=10;
   
   		double y=Math.sqrt(x);  //平方根
   		double z=Math.pow(2,3);  //幂运算
   		System.out.println(-25%12);  //1
   		System.out.println(Math.floorMod(-25,12));  //11
   		System.out.println(Math.PI);  //3.141592653589793
   		System.out.println(Math.E);  //2.718281828459045
   		System.out.println(log(pow(E,2)));  //自然对数  2.0
   		System.out.println(log10(100));  //以 10 为底的对数  2.0
   		System.out.println(exp(3));  //20.085536923187668
   	}
   }
   ```

### 5.2 数值类型之间的转换

   ```java
   int i=10032;
   float j=i;
   ```

### 5.3 强制类型转换

   ```java
   double x=5.435;
   int y=(int) x;
   int z=(int) Math.round(x);
   ```

   1. 强制类型转换的语法格式是在圆括号中给出想要转换的目标类型，后面紧跟待转换的变量名。
   2. 强制类型转换通过截断小数部分将浮点值转换为整型。
   3. 当调用 round 的时候， 仍然需要使用强制类型转换（ int)。其原因是 round 方法返回的结果为 long 类型，由于存在信息丢失的可能性， 所以只有使用显式的强制类型转换才能够将 long 类型转换成 int 类型。
   4. 如果试图将一个数值从一种类型强制转换为另一种类型， 而又超出了目标类型的表示范围， 结果就会截断成一个完全不同的值。 例如，（byte) 300 的实际值为 44。

### 5.4  结合赋值和运算符

可以在赋值中使用二元运算符，这是一种很方便的简写形式。

```java
x+=5;
y*=8;
```

### 5.5 自增与自减运算符

1.  后缀” 形式: n++ 。
2.  前缀” 形式：++n。
3. 后缀和前缀形式都会使变量值加 1 或减 1。但用在表达式中时，二者就有区别了。前缀形式会先完成加 1; 而后缀形式会使用变量原来的值。

### 5.6  关系和 boolean 运算符

1. Java 包含丰富的关系运算符： 要检测相等性，可以使用两个等号==。另外可以使用！= 检测不相等。

2. 如果用 && 运算符合并两个表达式，expression1  && expression2  而且已经计算得到第一个表达式的真值为 false, 那么结果就不可能为 true。 因此， 第二个表达式就不必计算了。

3. 类似地， 如果第一个表达式为 true，expression1  || expression2 的值就自动为 true, 而无需计算第二个表达式。

4. Java 支持三元操作符？：，这个操作符有时很有用。如果条件为 true, 下面的表达式

   ```java
   condition ? expression1 : expression2
   ```

   就为第一个表达式的值，否则计算为第二个表达式的值。例如，

   ```java
   x < y ? x : y
   ```

   会返回 x 和 y 中较小的一个。

### 5.7 位运算符

1. 用在布尔值上时， & 和丨运算符也会得到一个布尔值。这些运算符与 && 和丨丨运算符很类似， 不过 & 和丨运算符不采用“ 短路” 方式来求值， 也就是说， 得到计算结果之前两个操作数都需要计算.

### 5.8 括号与运算符级别

1.  如果不使用圆括号， 就按照给出的运算符优先级次序进行计算。同一个级别的运算符按照从左到右的次序进行计算

### 5.9 枚举类型

枚举类型包括有限个命名的值。Size 类型的变量只能存储这个类型声明中给定的某个枚举值， 或者 null 值，null 表示这
个变量没有设置任何值。

```java
enum Size { SMALL, MEDIUM, LARGE, EXTRA.LARCE };
Size s = Size.MEDIUM;
```



## 6. 字符串

### 6.1 字符串基本方法

1. 从概念上讲， Java 字符串就是 Unicode 字符序列。每个用双引号括起来的字符串都是 String 类的一个实例.

2. String 类的 substring 方法可以从一个较大的字符串提取出一个子串。substring 方法的第二个参数是不想复制的第一个位置。

3. 与绝大多数的程序设计语言一样，Java 语言允许使用 + 号连接（拼接）两个字符串。

4. 当将一个字符串与一个非字符串的值进行拼接时，后者被转换成字符串。任何一个 Java 对象都可以转换成字符串。

5. 如果需要把多个字符串放在一起， 用一个定界符分隔，可以使用静态 join 方法。

6. 由于不能修改 Java 字符串中的字符， 所以在 Java 文档中将 String 类对象称为不可变字符串， 如同数字 3 永远是数字 3一
   样，字符串“Hello” 永远包含字符 H、 e、 l、 l 和 o 的代码单元序列， 而不能修改其中的任何一个字符。

7. 不可变字符串却有一个优点：编译器可以让字符串共享。想象将各种字符串存放在公共的存储池中。字符串变量指向存储池中相应的位置。 如果复制一个字符串变量， 原始字符串与复制的字符串共享相同的字符。

   ```java
   		String str1="";
   		String str2="good java";
   		String str3=str2.substring(0,4);
   		String str4=str2+str3;
   		int i=20;
   		String str5=str4+i;
   		String str6=String.join("|","A","B","C","D");
   		System.out.println(str6);  //A|B|C|D
   		String str7="bug";
   		str7=str7.substring(0,2)+"t";
   		System.out.println(str7);  //but
   ```

   

8. Java 将自动地进行垃圾回收。 如果一块内存不再使用了， 系统最终会将其回收。

9. 可以使用 equals 方法检测两个字符串是否相等。要想检测两个字符串是否相等，而不区分大小写， 可以使用 equalsIgnoreCase 方法。

10. 一定不要使用= 运算符检测两个字符串是否相等！ 这个运算符**只能够确定两个字串是否放置在同一个位置上**。当然， 如果字符串放置在同一个位置上， 它们必然相等。但是，完全有可能将内容相同的多个字符串的拷贝放置在不同的位置上。

11. 如果虚拟机始终将相同的字符串共享， 就可以使用= 运算符检测是否相等。但实际上只有字符串常量是共享的，而 + 或 substring 等操作产生的结果并不是共享的。 因此，**千万不要使甩== 运算符测试字符串的相等性， 以免在程序中出现糟糕的 bug**。 从表面上看， 这种bug 很像随机产生的间歇性错误。

12. 空串 "" 是长度为 0 的字符串。可以调用if (str.lengthQ（）== 0)或  if (str.equals(""))代码检查一个字符串是否为空。空串是一个 Java 对象， 有自己的串长度（0 ) 和内容（空）。

13. String 变量还可以存放一个特殊的值， 名为 null, 这表示目前没有任何对象与该变量关联。要检查一个字符串是否为 null, 要使用以下条件if (str == null)。

14. 有时要检查一个字符串既不是 null 也不为空串，这种情况下就需要使用以下条件：if (str7 != null && str7.length()!=0)

    ```java
    		System.out.println(str7.equals(str6));
    		System.out.println("Hello".equals(str5));
    		System.out.println("Hello".equalsIgnoreCase("helLo"));
    		System.out.println(str7.compareTo("bug")==0);
    		System.out.println(str7.length()==0);
    		System.out.println(str7.equals(""));
    		System.out.println(str7==null);
    		if (str7 != null && str7.length()!=0) {
    			System.out.println("不为空和null");
    		}
    ```

15. length 方法将返回采用 UTF-16 编码表示的给定字符串所需要的代码单元数量。要想得到实际的长度，即码点数量，可以调用：

    ```java
    int cpCount=str7.codePointCount(0,str7.length());
    System.out.println(cpCount);  //3
    ```

16. 调用 s.charAt(n) 将返回位置 n 的代码单元，n 介于 0 ~ s.length()-1 之间。要想得到第 i 个码点，应该使用下列语句.

	```java
    		char fisrt=str7.charAt(0);
    		char last=str7.charAt(2);
    		int index=str7.offsetByCodePoints(0,i);
    		System.out.println(index);  //2
    		int cp=str7.codePointAt(index);
    		System.out.println(cp);  //116
    ```

17. 更容易的办法是使用 codePoints 方法， 它会生成一个 int 值的“ 流”，每个 int 值对应一个码点。可以将它转换为一个数组，再完成遍历。反之， 要把一个码点数组转换为一个字符串， 可以使用构造函数.

    ```java
    int[] codePoints=str7.codePoints().toArray();
    String str8=new String(codePoints,0,codePoints.length);
    ```

18. 有些时候， 需要由较短的字符串构建字符串， 例如， 按键或来自文件中的单词。 采用字符串连接的方式达到此目的效率比较低。 每次连接字符串， 都会构建一个新的 String 对象，既耗时， 又浪费空间。 使用 StringBuildei•类就可以避免这个问题的发生。
 ```java
 		//如果需要用许多小段的字符串构建一个字符串， 那么应该按照下列步骤进行。 首先， 构建一个空的字符串构建器：
 		StringBuilder builder=new StringBuilder();
 		//当每次需要添加一部分内容时， 就调用 append 方法。
 		builder.append('A');  //appends a single character
 		builder.append(" builder");  //appends a string
 		//在需要构建字符串时就凋用 toString 方法， 将可以得到一个 String 对象， 其中包含了构建器中的字符序列。
 		String completedString=builder.toString();
 		System.out.println(completedString);  //A builder
 ```
### 6.2 String API



## 7  输入输出

### 7.1 读取输入

1. 要想通过控制台进行输人，首先需要构造一个 Scanner 对象，并与“ 标准输人流” System.in 关联。

2. Scanner 类定义在java.util 包中。 当使用的类不是定义在基本java.lang 包中时， 一定要使用import 指示字将相应的包加载进来。

   ```java
   import java.util.Scanner;
   
   public class TestBase{
   	public static void main(String[] args) {
   		Scanner in=new Scanner(System.in);
   		System.out.print("input you name:");
   		String name=in.nextLine();  // nextLine 方法将输入一行。
   		System.out.print("输入一个单词：");
   		String firstName=in.next();  //以空白符作为分隔符
   		System.out.print("输入一个整数：");
   		int number1=in.nextInt();  //读取一个整数
   		System.out.print("输入一个浮点数：");
   		double price=in.nextDouble();
   		System.out.println(name+"\t"+firstName+"\t"+number1+"\t"+price);
   		//boolean hasNext( ) 检测输人中是否还有其他单词。
           //boolean hasNextInt( )  boolean hasNextDouble( )  检测是否还有表示整数或浮点数的下一个字符序列。
   	}
   }
   ```

3.  因为输入是可见的， 所以 Scanner 类不适用于从控制台读取密码。Java SE 6 特别引入了 Console 类实现这个目的。采用 Console 对象处理输入不如采用 Scanner 方便。每次只能读取一行输入， 而没有能够读取一个单词或一个数值的方法。

   ```java
   import java.io.Console;
   
   public class TestBase{
   	public static void main(String[] args) {
   		//Scanner in=new Scanner(System.in);
   		Console cons=System.console();
   		String username=cons.readLine("Log as:");
   		char[] passwd=cons.readPassword("Password:");
   
   		System.out.println("username:"+username);
   		for (int i=0;i<passwd.length;i++){
   			System.out.print(passwd[i]);
   		}
   
   	}
   }
   
   ```

### 7.2 格式化输出

   1. 使用 SyStem.0Ut.print(x) 将数值 x 输出到控制台上。这条命令将以 x 对应的数据类型所允许的最大非 0 数字位数打印输出 X。

   2. 在 printf 中， 可以使用多个参数,每一个以 ％ 字符开始的格式说明符都用相应的参数替换。 格式说明符尾部的转换符将指示被格式化的数值类型：f 表示浮点数，s 表示字符串，d 表示十进制整数。

   3. 还可以给出控制格式化输出的各种标志。逗号标志增加了分组的分隔符。

   4. 可以使用静态的 String.format 方法创建一个格式化的字符串， 而不打印输出.

      ```java
      		double x=100.0/3.0;
      		System.out.print(x);  //33.333333333333336
      		//用 8 个字符的宽度和小数点后两个字符的精度打印 x。
      		System.out.printf("%8.2f",x);  //   33.33   打印输出3个空格和7 个字符
      		String name="java";
      		int age=20;
      		System.out.println();
      		System.out.printf("learn %s for %d years",name,age);
      
      		System.out.printf("%,.4f",10000.0/3.0);  //3,333.3333
      
      		String message=String.format("learn %s for %d years",name,age);
      ```

### 7.3  文件输入与输出

1. 要想对文件进行读取， 就需要一个用 File 对象构造一个 Scanner 对象.

2. 要想写入文件， 就需要构造一个 PrintWriter 对象。在构造器中， 只需要提供文件名.

   ```java
   import java.util.Scanner;
   import java.io.PrintWriter;
   import java.nio.file.Paths;
   import java.io.*;
   
   public class TestBase{
   	public static void main(String[] args){
   		try{
   			Scanner in=new Scanner(Paths.get("test.txt"),"UTF-8");
   			if (in.hasNextLine()) {
   				System.out.println(in.nextLine());
   			}
   
   			Writer out=new PrintWriter("a.txt","UTF-8");
   		}catch (IOException e) {
   			e.printStackTrace();
   		}
   
   ```

3. 启动路径就是命令解释器的当前路後。 然而， 如果使用集成开发环境， 那么启动路径将由 IDE 控制。 可以使用下面的调用方式找到路径的位置：

   ```java
   String dir = System.getPropertyC'user.dir"):
   ```

4. 当采用命令行方式启动一个程序时， 可以利用 Shell 的重定向语法将任意文件关联到 System.in 和 System.out ，这样， 就不必担心处理 IOException 异常了。

   ```shell
   java MyProg < rayfile.txt > output.txt
   ```

## 8 控 制 流 程

### 8.1 块作用域

1. 块（即复合语句）是指由一对大括号括起来的若干条简单的 Java 语句。块确定了变量的作用域。一个块可以嵌套在另一个块中。
2. 不能在嵌套的两个块中声明同名的变量。
3. 使用块 （有时称为复合语句 ）可以在 Java 程序结构中原本只能放置一条 （ 简单）语句的地方放置多条语句。

### 8.2 条件语句

```java
		int i=66;
		if (i<60) {
			System.out.println("C");
		}else if(i<80){
			System.out.println("B");
		}else {
			System.out.println("A");
		}
```

### 8.3  while循环

```java
		int i=66;
		while (i>60){
			System.out.println(i);
			i--;
		}

		do {
			System.out.println(i);
			i--;
		}while(i>60);

		for (int j=0; j<10; j++) {
			System.out.println(j);
		}

		for (double x=0; x!=10; x+=0.1) {
			//可能永远不会结束。 由于舍入的误差， 最终可能得不到精确值。
			System.out.println(x);
			if (x>11) {
				break;
			}
			/*
			9.99999999999998
			10.09999999999998
			*/
		}
```

###  8.4  for循 环

1. 尽管 Java 允许在 for 循环的各个部分放置任何表达式， 但有一条不成文的规则： for 语句的 3 个部分应该对同一个计数器变量进行初始化、 检测和更新。
2.  如果在 for 语句内部定义一个变量， 这个变量就不能在循环体之外使用。
3. 在循环中，检测两个浮点数是否相等需要格外小心。
4. 可以在各自独立的不同 for 循环中定义同名的变量

### 8.5  多重选择：switch 语句

```java
		int code=2;
		switch (code){
			case 0:
				System.out.println("exit!");
				break;
			case 1:
				System.out.println("start...");
				break;
			case 2:
				System.out.println("run...");
				break;
			default:
				System.out.println("wait...");
				break;
		}
```

1. switch 语句将从与选项值相匹配的 case 标签处开始执行直到遇到 break 语句， 或者执行到switch i吾句的结束处为止。 如果没有相匹配的 case 标签， 而有 default 子句， 就执行这个子句。
2. case 标签可以是：
   - 类型为 char、byte、 short 或 int 的常量表达式。
   - 枚举常量。
   - 从 Java SE 7 开始， case 标签还可以是字符串字面量。

### 8.6 中断控制流程语句

1. Java 还提供了一种带标签的 break 语句， 用于跳出多重嵌套的循环语句。标签必须放在希望跳出的最外层循环之前， 并且必须紧跟一个冒号。
2. 对于任何使用break 语句的代码都需要检测循环是正常结束， 还是由 break 跳出。
3. 事实上，可以将标签应用到任何语句中， 甚至可以应用到 if语句或者块语句中;
4. 因此， 如果希望使用一条 goto 语句， 并将一个标签放在想要跳到的语句块之前， 就可以使用 break 语句！ 当然， 并不提倡使用这种方式。 另外需要注意， 只能跳出语句块，而不能跳入语句块。
5. 还有一个 continue 语句。 与 break 语句一样， 它将中断正常的控制流程。continue语句将控制转移到最内层循环的首部。
6. 还有一种带标签的 continue 语句，将跳到与标签匹配的循环首部。

```java
		int n=66;
		double sum=0;
		breakLabel1;
		while(n>0){
			for (int i=0; i<n; i++) {
				if (n==50) {
					continue;
				}
				sum+=n;
				n--;
				
				if (n%10==0){
					break breakLabel1;
				}
			}
		}
```

## 9 大 数 值

1. Biglnteger 类实现了任意精度的整数运算， BigDecimal 实现了任意精度的浮点数运算。
2. 使用静态的 valueOf 方法可以将普通的数值转换为大数值。
3. 不能使用人们熟悉的算术运算符（如：+ 和 *) 处理大数值。 而需要使用大数值类中的 add 和 multiply 方法。

```java
		BigInteger a=BigInteger.valueOf(1000);
		BigInteger b=BigInteger.valueOf(1200);
		BigInteger c=a.add(b);
		BigInteger d=a.multiply(b);
```

```java
import java.util.*;
import java.math.*;

public class TestBase{
	public static void main(String[] args){

		Scanner in=new Scanner(System.in);
		System.out.print("How many number do you need to draw?");
		int k=in.nextInt();
		System.out.print("How is the highest number you can draw?");
		int n=in.nextInt();

		BigInteger lotteryOdds=BigInteger.valueOf(1);
		for (int i=1; i<=k; i++) {
			lotteryOdds=lotteryOdds.multiply(BigInteger.valueOf(n-i+1)).divide(BigInteger.valueOf(i));
		}
		System.out.println("Your odds are 1 in "+lotteryOdds+" . Good lick");

	}
}
```

## 1 0  数 组

1. ​	数组是一种数据结构， 用来存储同一类型值的集合。通过一个整型下标可以访问数组中的每一个值。

2. 在声明数组变量时， 需要指出数组类型 （数据元素类型紧跟 [] ) 和数组变量的名字。 

   ```java
   int[] a;  //大多数 Java 应用程序员喜欢使用的风格， 因为它将类型 int[] ( 整型数组）与变量名分开了。
   int a[];
   int[] a = new int[100];
   for (int i=0;i<100;i++){
       a[i]=i+1;
   }
   ```

3.  应该使用 new 运算符创建数组。

4. 创建一个数字数组时， 所有元素都初始化为 0。boolean 数组的元素会初始化为 false, 对象数组的元素则初始化为一个特殊值 null, 这表示这些元素（还）未存放任何对象。

5. 要想获得数组中的元素个数，可以使用 array.length。

6. 一旦创建了数组， 就不能再改变它的大小（尽管可以改变每一个数组元素）0 如果经常需要在运行过程中扩展数组的大小， 就应该使用另一种数据结构—数组列表（array list) 。

7. Java 中的 [ ] 运算符被预定义为检查数组边界， 而且没有指针运算， 即不能通过 a 加1 得到数组的下一个元素。

### 10.1  for each 循环

1. Java 有一种功能很强的循环结构， 可以用来依次处理数组中的每个元素（其他类型的元素集合亦可）而不必为指定下标值而分心。

   ```java
   		for (int i=0; i<a.length; i++){
   			System.out.println(a[i]);
   		}
   		for (int el : a){
   			System.out.println(el);
   		}
   ```

2. for each 循环语句的循环变量将会遍历数组中的每个元素， 而不需要使用下标值。

### 10.2  数组初始化以及匿名数组

1. 在 Java 中， 提供了一种创建数组对象并同时赋予初始值的简化书写形式。在使用这种语句时，不需要调用 new。甚至还可以初始化一个匿名的数组：

   ```java
   		int [] smallPrimes={2,3,5,7,11,14};
   		new int[] {17, 19, 23, 29, 31, 37};  //匿名的数组
   		smallPrimes = new int[] { 17, 19, 23, 29, 31, 37 };
   
   		intD anonymous = { 17, 19, 23, 29, 31, 37 };
   		smallPrimes = anonymous;
   
   		new elementType[0];
   ```

2. 这种表示法将创建一个新数组并利用括号中提供的值进行初始化， 数组的大小就是初始值的个数。 使用这种语法形式可以在不创建新变量的情况下重新初始化一个数组。

3. 在 Java 中， 允许数组长度为 0。 在编写一个结果为数组的方法时， 如果碰巧结果为空， 则这种语法形式就显得非常有用。 此时可以创建一个长度为 0 的数组。注意， 数组长度为 0 与 null 不同。

### 10.3  数组拷贝

1. 在 Java 中， 允许将一个数组变量拷贝给另一个数组变量。 这时， 两个变量将引用同一个数组.

2. 如果希望将一个数组的所有值拷贝到一个新的数组中去，就要使用 Arrays 类的 copyOf 方法。第 2 个参数是新数组的长度。这个方法通常用来增加数组的大小。如果数组元素是数值型，那么多余的元素将被赋值为 0 ; 如果数组元素是布尔型， 则将赋值为 false。相反， 如果长度小于原始数组的长度， 则只拷贝最前面的数据元素。

   ```java
   		smallPrimes = new int[] { 17, 19, 23, 29, 31, 37 };
   		int[] luckNumbers=smallPrimes;
   		luckNumbers[3]=66;
   
   		int [] copyLuckNumbers=Arrays.copyOf(luckNumbers,10);
   ```

### 10.4  命令行参数

1. 每一个 Java 应用程序都有一个带 String arg[]参数的 main 方法。这个参数表明 main 方法将接收一个字符串数组， 也就是命令行参数 。

2. 在 Java 应用程序的 main 方法中， 程序名并没有存储在 args 数组中 。

   ```java
   		if (args.length==0 || args[0].equals("-h")) {
   			System.out.println("Hello,");
   		}else if(args[0].equals("-g")){
   			System.out.println("Goodbye,");
   		}
   		for (int i=1; i<args.length; i++) {
   			System.out.println(" "+args[i]);
   		}
   
   		//java TestBase -g senbin 36
   		//java TestBase -h senbin 36
   ```

### 10.5  数组排序

1. 要想对数值型数组进行排序， 可以使用 Arrays 类中的 sort 方法。这个方法使用了优化的快速排序算法。快速排序算法对于大多数数据集合来说都是效率比较
   高的。

   ```java
   		int[] a={3,21,65,43,21,13,78,54};
   		Arrays.sort(a);
   
   		int n=10000;
   		int[] numbers=new int[n];
   		for (int i=0; i<n; i++) {
   			numbers[i]=i+1;
   		}
   
   		int k=7;
   		int[] result=new int[k];
   
   		for (int j=0; j<k; j++) {
   			int r=(int) (Math.random()*n);
   			result[j]=numbers[r];  //关键在于每次抽取的都是下标， 而不是实际的值。
   			numbers[r]=numbers[n-1];  //下标指向包含尚未抽取过的数组元素。
   			n--;  //保证每个值只被抽一次
   		}
   		for (int r:result) {
   			System.out.print(r+" \t");
   		}  //5706 	6914 	7926 	5600 	2753 	815 	966 	
   		System.out.println();
   		Arrays.sort(result);
   		for (int r:result) {
   			System.out.print(r+" \t");
   		}  //815 	966 	2753 	5600 	5706 	6914 	7926 	
   		System.out.println();
   ```

### 10.6  多维数组

1. 在 Java 中， 声明一个二维数组相当简单。与一维数组一样， 在调用 new 对多维数组进行初始化之前不能使用它。一J1数组被初始化， 就可以利用两个方括号访问每个元素。

2.  for each 循环语句不能自动处理二维数组的每一个元素。它是按照行， 也就是一维教组处理的要想访问二维教组 a 的所有元素， 需要使用两个嵌套的循环，

3. 要想快速地打印一个二维数组的数据元素列表， 可以调用Arrays.deepToString()

   ```java
   		int[][] multiArrays;  //声明
   		multiArrays=new int[3][3];
   
   		int[][] multiArrays2={{1,2,3},{2,3,4},{3,4,5}};
   
   		for (int i=0; i<multiArrays.length; i++) {
   			for (int j=0; j<multiArrays[i].length; j++) {
   				multiArrays[i][j]=(i+1)*(j+1);
   			}
   		}
   
   		for (int[] a : multiArrays) {
   			for (int b : a) {
   				System.out.print(b+"\t");
   			}
   			System.out.println();
   		}
   
   		System.out.println(Arrays.deepToString(multiArrays));
   		//[[1, 2, 3], [2, 4, 6], [3, 6, 9]]
   ```

### 10.7  不规则数组

1. Java 实际上没有多维数组， 只有一维数组。 多维数组被解释为“ 数组的数组。”

2. 由于可以单独地存取数组的某一行， 所以可以让两行交换。

3. 还可以方便地构造一个“ 不规则” 数组， 即数组的每一行有不同的长度。

   ```java
   		final int N=9;
   		//首先需要分配一个具有所含行数的数组
   		int[][] multiArrays =new int[N][];
   
   		for (int i=0; i<N; i++) {
   			//分配这些行
   			multiArrays[i]=new int[i+1];
   		}
   
   		for (int i=0; i<multiArrays.length; i++) {
   			for (int j=0; j<multiArrays[i].length; j++) {
   				multiArrays[i][j]=i+j;
   			}
   		}
   
   		for (int[] a : multiArrays) {
   			for (int b : a) {
   				System.out.print(b+"\t");
   			}
   			System.out.println();
   		}
   
   ```

   

