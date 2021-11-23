#  接口、lambda 表达式与内部类



## 1  接 口

### 1.1  接口概念

1. 接口（ interface) 技术， 这种技术主要用来描述类具有什么功能，而并不给出每个功能的具体实现。在 Java 程序设计语言中， 接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一格式进行定义。

2. 一个类可以实现（ implement) —个或多个接口，并在需要接口的地方， 随时使用实现了相应接口的对象。

3. 接口中的所有方法自动地属于 public。 因此， 在接口中声明方法时， 不必提供关键字public。不过，在实现接口时， 必须把方法声明为 public; 否则， 编译器将认为这个方法的访问属性是包可见性， 即类的默认访问属性， 之后编译器就会给出试图提供更严格的访问权限的警告信息。

4. 提供实例域和方法实现的任务应该由实现接口的那个类来完成。 因此， 可以将接口看成是没有实例域的抽象类 。

5. 为了让类实现一个接口， 通常需要下面两个步骤：

   - 将类声明为实现给定的接口。要将类声明为实现某个接口， 需要使用关键字 implements。
   - 对接口中的所有方法进行定义。

   ```java
   public class TestBase{
   	public static void main(String[] args){
   		Employee[] staff=new Employee[3];
   
   		staff[0]=new Employee("mane",20,1994,12,11);
   		staff[1]=new Employee("Alison",23,1993,10,5);
   		staff[2]=new Employee("Arnold",18,2000,4,12);
   
   		for (Employee e:staff){
   			e.raiseSalary(5);
   		}
   
   		Arrays.sort(staff);  //对一个员工数组排序
   
   		for (Employee e:staff){
   			System.out.println("name:"+e.getName()+",salary:"+e.getSalary()+",hireDay:"+e.getHireDay());
   		}
   
   	}
   }
   class Employee implements Comparable<Employee>{
   	...
   	public int compareTo(Employee other){
   		return Double.compare(salary,other.salary);
   	}
   }
   
   Compiling TestBase.java......
   ------Output------
   name:Arnold,salary:18.9,hireDay:2000-04-12
   name:mane,salary:21.0,hireDay:1994-12-11
   name:Alison,salary:24.15,hireDay:1993-10-05
   ```

   ```java
   java.lang.Comparable<T> 1.0
       • int compareTo(T other)
       用这个对象与 other 进行比较。 如果这个对象小于 other 则返回负值； 如果相等则返回0； 否则返回正值。
    java.util.Arrays 1.2
        static void sort(Object[] a )
       使用 mergesort 算法对数组 a 中的元素进行排序。 要求数组中的元素必须属于实现了Comparable 接口的类， 并且元素之间必须是可比较的。
   java.lang.lnteger 1.0
       • static int comparednt x , int y) 7
       如果 x < y 返回一个负整数；如果 x 和 y 相等， 则返回 0; 否则返回一个负整数。
   java.lang.Double 1.0
       • static int compare(double x , double y) 1.4
       如果 x < y 返回一个负数；如果 x 和 y 相等则返回 0; 否则返回一个负数        
   ```

   如 果 子 类 之 间 的 比 较 含 义 不 一 样， 那 就 属 于 不 同 类 对 象 的 非 法 比 较。 每 个compareTo 方 法 都 应 该 在 开 始 时 进 行 下 列 检 测：

   ```java
   if (getClass() != other.getClass()) throw new ClassCastException()；
   ```

   如 果 存 在 这 样 一 种 通 用 算 法， 它 能 够 对 两 个 不 同 的 子 类 对 象 进 行 比 较， 则 应 该 在 超类 中 提 供 一 个 compareTo 方 法， 并 将 这 个 方 法 声 明 为 final ,

### 1.2  接口的特性

```java
		x = new Comparable(. . .); // ERROR
		Comparable x; // OK
		x = new Employee(. . .); // OK provided Employee implements Comparable
		if (anObject instanceof Comparable) { . . . }

		public interface Movable{
			void move(double x,double y);
		}
		//以它为基础扩展一个叫做 Powered 的接口
		public interface Powered extends Movable {
			double milesPerGallon();
			double SPEED_LIMIT=120;    //public static常量
		}
		class Employee implements Cloneable, Comparable{}
```

1. 接口不是类，尤其不能使用 new 运算符实例化一个接口。
2. 尽管不能构造接口的对象，却能声明接口的变量。
3. 接口变量必须弓I用实现了接口的类对象。
4. 如同使用 instanceof 检查一个对象是否属于某个特定类一样， 也可以使用instance 检查一个对象是否实现了某个特定的接口。
5. 与可以建立类的继承关系一样，接口也可以被扩展。这里允许存在多条从具有较高通用性的接口到较高专用性的接口的链。
6. 虽然在接口中不能包含实例域或静态方法，但却可以包含常量。与接口中的方法都自动地被设置为 public一样，接口中的域将被自动设为 public static final。
7. 可以将接口方法标记为 public, 将域标记为 public static final。但 Java 语言规范却建议不要书写这些多余的关键字。
8. 有些接口只定义了常量， 而没有定义方法。然而，这样应用接口似乎有点偏离了接口概念的初衷， 最好不要这样使用它。
9. 尽管每个类只能够拥有一个超类， 但却可以实现多个接口。这就为定义类的行为提供了极大的灵活性。 

### 1.3  接口与抽象类

```java
		class Employee extends Person, Comparable // Error
		class Employee extends Person implements Comparable // OK
```

使用抽象类表示通用属性存在这样一个问题： 每个类只能扩展于一个类。但每个类可以像下面这样实现多个接口。Java 的设计者选择了不支持多继承，其主要原因是多继承会让语言本身变得非常复杂（如同 C++)， 效率也会降低 （如同 Eiffel)。实际上， 接口可以提供多重继承的大多数好处，同时还能避免多重继承的复杂性和低效性。

### 1.4  静态方法

```java
public interface Path
    public static Path get(String first, String... more) {
    	return Fi1eSystems.getDefault().getPath(first, more);
    }
...
 }
```

1. 通常的做法都是将静态方法放在伴随类中。 在标准库中， 你会看到成对出现的接口和实用工具类， 如 Collection/Collections 或 Path/Paths。

### 1.5  默认方法

```java
		public interface Comparable<T>{
			default int compareTo(T other){
				return 0;  //By default, all elements are the same
			}
		}
		public interface Collection{
			int size();  //一个抽象方法
			default boolean isEmpty(){
				return size()==0;
			}
			//实现 Collection 的程序员就不用操心实现 isEmpty 方法了。
		}
```

1. 可以为接口方法提供一个默认实现。 必须用 default 修饰符标记这样一个方法。
2. 在 Java SE 8 中， 可以把所有方法声明为默认方法， 这些默认方法什么也不做。
3. 默认方法可以调用任何其他方法。
4. 在 JavaAPI 中， 你会看到很多接口都有相应的伴随类， 这个伴随类中实现了相应接口 的部分或所有方法， 如 CoUection/AbstractCollectkm 或 MouseListener/MouseAdapter。在JavaSE 8 中， 这个技术已经过时。现在可以直接在接口中实现方法。
5. 默认方法的一"t•重要用法是“‘接口演化” （interface evolution)。

### 1.6  解决默认方法冲突

如果先在一个接口中将一个方法定义为默认方法， 然后又在超类或另一个接口中定义了同样的方法， 会发生什么情况？幸运的是， Java 的相应规则要简单得多。规则如下：

1. 超类优先。如果超类提供了一个具体方法， 同名而且有相同参数类型的默认方法会被忽略。这正是“ 类优先” 规则。 类优先” 规则可以确保与 Java SE 7 的兼容性。 如果为一个接口增加默认方法，这对于有这个默认方法之前能正常工作的代码不会有任何影响。
2. 接口冲突。 如果一个超接口提供了一个默认方法， 另一个接口提供了一个同名而且参数类型 （不论是否是默认参数）相同的方法， 必须覆盖这个方法来解决冲突。

**千万不要让一个默认方法重新定义 Object 类中的某个方法。**例如， 不能为 toString或 equals 定义默认方法， 尽管对于 List 之类的接口这可能很有吸引力， 由于“
类优先”规则， 这样的方法绝对无法超越 Object.toString 或 Objects.equals。

## 2  接口示 例

### 2.1  接口与回调

1. 回调（callback) 是一种常见的程序设计模式。在这种模式中， 可以指出某个特定事件发生时应该采取的动作。 

```java
import java.util.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.Timer;
//消 除 了 javax.swing.Timer 与 java.util.Timer 之间产生的二义性。

class TimePrinter implements ActionListener{
	public void actionPerformed(ActionEvent event){
		System.out.println("At the tone,the time is "+new Date());
		Toolkit.getDefaultToolkit().beep();  //获得默认的工具箱。发出一声铃响。
	}
}

public class TestBase2{
	public static void main(String[] args){
		ActionListener listener=new TimePrinter();
		Timer t=new Timer(10000,listener);  //构造一个定时器， 每隔 interval 毫秒通告 listener—次 ，
		t.start();  //启动定时器一旦启动成功， 定时器将调用监听器的 actionPerformed。
		JOptionPane.showMessageDialog(null,"Quit program");  //显示一个包含一条消息和 OK 按钮的对话框。
		System.exit(0);
	}
}
```

### 2.2  Comparator 接口

1. Comparator 接口包含很多方便的静态方法来创建比较器。 这些方法可以用于 lambda 表达式或方法引用。

2. 静态 comparing 方法取一个“ 键提取器” 函数， 它将类型 T 映射为一个可比较的类型( 如 String )。 对要比较的对象应用这个函数， 然后对返回的键完成比较。 例如， 假设有一个Person 对象数组，可以如下按名字对这些对象排序：

   ```java
   Arrays.sort(people, Comparator.comparing(Person::getName));
   ```

3. 可以把比较器与 thenComparing 方法串起来。

   ```java
   Arrays.sort(people,Comparator.comparing(Person::getlastName).thenConiparing(Person::getFirstName));
   ```

4. 这些方法有很多变体形式。 可以为 comparing 和 thenComparing 方法提取的键指定一个比较器。例如，可以如下根据人名长度完成排序：

   ```java
   Arrays.sort(people, Comparator.companng(Person::getName,
   	(s, t) -> Integer.compare(s.1ength(), t.lengthO)))；
   ```

5. 另外， comparing 和 thenComparing 方法都有变体形式，可以避免 int、 long 或 double 值的装箱。要完成前一个操作， 还有一种更容易的做法：

   ```java
   Arrays.sort(people, Comparator.comparinglnt(p -> p.getNameO -length()));
   ```

6. 如果键函数可以返回 null, 可 能 就 要 用 到 nullsFirst 和 nullsLast 适配器。 这些静态方法会修改现有的比较器， 从而在遇到 null 值时不会抛出异常， 而是将这个值标记为小于或大于正常值。  例如， 假设一个人没有中名时 getMiddleName 会返回一个 null, 就 可 以 使 用

   ```java
   Comparator.comparing(Person::getMiddleName(), Comparator.nullsFirst(…))。
   ```

7. nullsFirst 方法需要一个比较器， 在这里就是比较两个字符串的比较器。naturalOrder 方法可以为任何实现了 Comparable 的类建立一个比较器。注意 naturalOrder 的类型可以推导得出。

   ```java
   Arrays.sort(people, comparing(Person::getMiddleName, nulIsFirst(naturalOrder())));
   ```

8. 静态 reverseOrder 方法会提供自然顺序的逆序。要让比较器逆序比较， 可以使用 reversed实例方法c 例如 naturalOrder().reversed() 等同于 reverseOrder()。

```java
public interface Comparator<T>{
	int compare(T first,T second);
}
//按长度递增的顺序对字符串进行排序
class LengthComparator implements Comparator{
	public int compare(String first,String second){
		return first.length()-second.length();
	}
}
```

### 2.3  对象克隆

1. 如果希望 copy 是一个新对象，它的初始状态与 original 相同， 但是之后它们各自会有自己不同的状态， 这种情况下就可以使用 clone 方法。
2. 浅拷贝会有什么影响吗？ 这要看具体情况。
   - 如果原对象和浅克隆对象共享的子对象是不可变的， 那么这种共享就是安全的。如果子对象属于一个不可变的类， 如 String, 就 是 这 种情况。
   - 或者在对象的生命期中， 子对象一直包含不变的常量， 没有更改器方法会改变它， 也没有方法会生成它的引用，这种情况下同样是安全的。
   - 不过， 通常子对象都是可变的， 必须重新定义 clone 方法来建立一个深拷贝， 同时克隆所有子对象。
3. 对于每一个类，需要确定：
   - 默认的 clone 方法是否满足要求；
   - 是否可以在可变的子对象上调用 clone 来修补默认的 clone 方法；
   - 是否不该使用 clone()
   - 实际上第 3 个选项是默认选项。 如果选择第 1 项或第 2 项，类必须：
     1 ) 实现 Cloneable 接口；
     2 ) 重新定义 clone 方法，并指定 public 访问修饰符。
4. 子类只能调用受保护的 clone方法来克隆它自己的对象。 必须重新定义 clone 为 public 才能允许所有方法克隆对象。
5.  对象对于克隆很“ 偏执”， 如果一个对象请求克隆， 但没有实现这个接口， 就会生成一个受査异常。
6. Cloneable 接口是 Java 提供的一组标记接口 ( tagging interface ) 之一。（有些程序员称之为记号接口 ( marker interface))。应该记得，Comparable 等接口的通常用途是确保一个类实现一个或一组特定的方法。 标记接口不包含任何方法； 它唯一的作用就是允许在类型查询中使用 instanceof。建议在自己的程序中不要使用标记接口。
7. 即使 clone 的默认（浅拷贝）实现能够满足要求， 还是需要实现 Cloneable 接口， 将 clone重新定义为 public， 再调用 super.clone()。
8. 必须当心子类的克隆。不能保证子类的实现者一定会修正 clone 方法让它正常工作。出于这个原因， 在 Object类中 clone 方法声明为 protected。
9. 所有数组类型都有一个 public 的 clone 方法， 而不是 protected: 可以用这个方法建立一个新数组， 包含原数组所有元素的副本。

```java
class Employee implements Cloneable,Comparable<Employee>{
	public Employee clone(){
		try{
			Employee cloned=(Employee) super.clone();  //// call Object,clone0
			cloned.hireDay=(Date) hireDay.clone();  //克隆对象中可变的实例域。
			return cloned;
		}catch (CloneNotSupportedException e) {
			return null;  //Employee 和 Date 类实现了Cloneable 接口， 所以不会抛出这个异常。 
		}
	}
	
		Employee old1=new Employee("bob",20,1994,12,11);
		Employee copy1=old1;  //
		Employee clone1=old1.clone();
		copy1.raiseSalary(10);
		clone1.raiseSalary(20);
		System.out.println("old1 salary:"+old1.getSalary());
		System.out.println("copy1 salary:"+copy1.getSalary());
		System.out.println("clone1 salary:"+clone1.getSalary());
		
```

## 3  lambda 表达式

### 3.1  为什么引入 lambda 表达式

1. lambda 表达式是一个可传递的代码块， 可以在以后执行一次或多次。

### 3.2  lambda 表达式的语法

1.  lambda 表达式就是一个代码块， 以及必须传人代码的变量规范。

2. lambda 表达式形式：参数， 箭头（->) 以及一个表达式。如果代码要完成的计算无法放在一个表达式中，就可以像写方法一样，把这些代码放在 {丨中，并包含显式的 return 语句。 

   ```java
   	(String first,String second)->{
   		if (first.length()<second.length()) {
   			return -1;
   		}else if(first.length()>second.length()){
   			return 1;
   		}else{
   			return 0;
   		}
   	}
   ```

3. 即使 lambda 表达式没有参数， 仍然要提供空括号，就像无参数方法一样：

   ```java
   	()->{
   		for (int i=0;i<20;i++){System.out.println(i);}
   	}
   ```

4. 如果可以推导出一个 lambda 表达式的参数类型，则可以忽略其类型。

   ​		

   ```java
   	Comparator<String>comp=(first,second)->first.length()-second.length();
   ```

5. 如果方法只有一参数， 而且这个参数的类型可以推导得出，那么甚至还可以省略小括号

   ```java
   	ActionListener listener=event->System.out.println("the time is "+new Date());
   ```

6. 无需指定 lambda 表达式的返回类型。 lambda 表达式的返回类型总是会由上下文推导得出。

7. 如果一个 lambda 表达式只在某些分支返回一个值， 而在另外一些分支不返回值，这是不合法的。

   ```java
   （int x)-> { if (x >= 0) return 1; }  //不合法
   ```

   ```java
   import java.util.*;
   import java.awt.*;
   import java.awt.event.*;
   import javax.swing.*;
   import javax.swing.Timer;
   
   public class TestBase2{
   	public static void main(String[] args){
   		String[] names={"mane","Alison","Arnold","salah"};
   		System.out.print("原始顺序：");
   		System.out.println(Arrays.toString(names));
   		System.out.println("按字典排序：");
   		Arrays.sort(names);
   		System.out.println(Arrays.toString(names));
   		System.out.println("按长度排序");
   		Arrays.sort(names,(first,second)->first.length()-second.length());
   		System.out.println(Arrays.toString(names));
   
   		//构造一个定时器， 每隔 interval 毫秒通告 listener—次 ，
   		Timer t=new Timer(2000,event->System.out.println("the time is "+new Date()));
   		t.start();  //启动定时器
   		JOptionPane.showMessageDialog(null,"Quit program");  //显示一个包含一条消息和 OK 按钮的对话框。
   		System.exit(0);
   	}
   }
   
   /*
   ------Output------
   原始顺序：[mane, Alison, Arnold, salah]
   按字典排序：
   [Alison, Arnold, mane, salah]
   按长度排序
   [mane, salah, Alison, Arnold]
   the time is Mon Nov 22 15:06:01 CST 2021
   the time is Mon Nov 22 15:06:03 CST 2021
   the time is Mon Nov 22 15:06:05 CST 2021
   */
   ```

###  3.3  函 数 式 接 口

1. 对于只有一个抽象方法的接口， 需要这种接口的对象时， 就可以提供一个 lambda 表达式。 这种接口称为函数式接口 （functional interface )。

2. 最好把 lambda 表达式看作是一个函数， 而不是一个对象， 另外要接受 lambda 表达式可以传递到函数式接口。

3. lambda 表达式可以转换为接口， 这一点让 lambda 表达式很有吸引力。具体的语法很简短。实际上，在 Java 中， 对 lambda 表达式所能做的也只是能转换为函数式接口。

   ```java
   		Timer t=new Timer(2000,event->{
   			System.out.println("the time is "+new Date());
   			Toolkit.getDefaultToolkit.beep();
   		});
   ```

4. 不能把ambda 表达式赋■给类型为 Object 的变量，Object 不是一个函数式接口。

### 3.4  方法引用

1. 表达式 System.out::println 是一个方法引用（ method reference), 它等价于 lambda 表达式x 一> System.out.println(x)（）

   ```java
   		Timer t = new Timer(1000, event -> System.out.println(event));
   		//方法引用
   		Timer t=new Timer(1000,System.out::println)
   		Arrays.sort(names,String::compareToIgnoreCase);
   		System.out::println  //==>x-> System.out.println(x)
   		Math::pow  //==>（x，y)->Math.pow(x, y)
            String::compareToIgnoreCase 等同于 (x, y)-> x.compareToIgnoreCase(y) 0
   ```

2. 要用：: 操作符分隔方法名与对象或类名。主要有 3 种情况：

   - object::instanceMethod

   - Class::staticMethod

   - Class::instanceMethod

     在前 2 种情况中， 方法引用等价于提供方法参数的 lambda 表达式。 

     对于第 3 种情况， 第 1 个参数会成为方法的目标。

3. 如果有多个同名的重栽方法， 编译器就会尝试从上下文中找出你指的那一个方法。

4. 类似于 lambda 表达式， 方法引用不能独立存在，总是会转换为函数式接口的实例。

5. 可以在方法引用中使用 this 参数。 例如，this::equals 等同于 x-> this.equals(x)。 使用super 也是合法的。super::instanceMethod使用 this 作为目标，会调用给定方法的超类版本。

```java
import java.util.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.Timer;

class Greeter{
	public void greet(){
		System.out.println("方法引用");
	}
}
class TimeGreeter extends Greeter{
	/*TimedGreeter.greet 方法开始执行时， 会构造一个 Timer, 它会在每次定时器滴答时执行
	super::greet 方法。这个方法会调用超类的 greet 方法。
	*/
	public void greet(){
		Timer t=new Timer(1000,super::greet);
		t.start();
	}
}
public class TestBase3{
	public static void main(String[] args){
		new TimeGreeter().greet();
        JOptionPane.showMessageDialog(null,"停止程序?");  //显示一个包含一条消息和 OK 按钮的对话框。
		System.exit(0);
	}
}
/*
Compiling TestBase3.java......
TestBase3.java:17: 错误: 不兼容的类型: 方法引用无效
		Timer t=new Timer(1000,super::greet);
		                       ^
    无法将 类 Greeter中的 方法 greet应用到给定类型
      需要: 没有参数
      找到:    ActionEvent
      原因: 实际参数列表和形式参数列表长度不同
注: 某些消息已经过简化; 请使用 -Xdiags:verbose 重新编译以获得完整输出
1 个错误
*/
```

### 3.5  构造器引用

1. 构造器引用与方法引用很类似，只不过方法名为 new。哪一个构造器呢？ 这取决于上下文。
2. 可以用数组类型建立构造器引用。例如， int[]::new 是一个构造器引用， 它有一个参数：**即数组的长度**。这等价于 lambda 表达式 x-> new int[x]。
3. Java 有一个限制，无法构造泛型类型 T 的数组。数组构造器引用对于克服这个限制很有用。

### 3.6  变量作用域

你可能希望能够在 lambda 表达式中访问外围方法或类中的变量。lambda 表达式有 3个部分：
	1 ) 一个代码块；
	2 ) 参数;
	3 ) 自由变量的值， 这是指非参数而且不在代码中定义的变量。

```java
public class TestBase3{
	public static void repeatMessage(String text,int delay){
		ActionListener listener=event->{
			System.out.println(text);
			Toolkit.getDefaultToolkit().beep();
		};
		new Timer(delay,listener).start();
		JOptionPane.showMessageDialog(null,"Quit program");  //显示一个包含一条消息和 OK 按钮的对话框。
		System.exit(0);
	}
	public static void main(String[] args){
		repeatMessage("lambda 表达式中的自由变量 text",1000);
	}
}
```

1. lambda 表达式可以捕获外围作用域中变量的值。在 lambda 表达式中， 只能引用值不会改变的变量。 如果在 lambda 表达式中改变变量， 并发执行多个动作时就会不安全。 
2. 另外如果在 lambda 表达式中引用变量， 而这个变量可能在外部改变，这也是不合法的。
3. 有一条规则：lambda 表达式中捕获的变量必须实际上是最终变量 ( effectivelyfinal)。实际上的最终变量是指， 这个变量初始化之后就不会再为它赋新值。
4. lambda 表达式的体与嵌套块有相同的作用域。 这里同样适用命名冲突和遮蔽的有关规则。在 lambda 表达式中声明与一个局部变量同名的参数或局部变量是不合法的。
5. 在方法中，不能有两个同名的局部变量， 因此， lambda 表达式中同样也不能有同名的局部变量。

### 3.7  处理 lambda 表达式

1. 使用 lambda 表达式的重点是延迟执行deferred execution )毕竟， 如果想耍立即执行代码，完全可以直接执行， 而无需把它包装在一个丨ambda 表达式中。 之所以希望以后再执行代码， 这有很多原因:

    - 在一个单独的线程中运行代码；
    - 多次运行代码；
    - 在算法的适当位置运行代码 （例如， 排序中的比较操作 )；
    - 发生某种情况时执行代码 （如， 点击了一个按钮， 数据到达， 等等 )；
    - 只在必要时才运行代码。

2. 要接受这个 lambda 表达式， 需要选择（偶尔可能需要提供）一个函数式接口。

```java
public class TestBase3{
	public static void repeat(int n,Runnable action){
		for (int i=0; i<n; i++) {
			action.run();
		}
	}
	public static void main(String[] args){
		repeat(10,()->System.out.println("调用 action.run() 时会执行这个 lambda 表达式的主体。"));
	}
}
```

Java API 中提供的最重要的函数式接口

![常用函数式接口png](E:\pogject\javalearn\学习笔记\img\常用函数式接口.png)

3. 希望告诉这个动作它出现在哪一次迭代中。 为此， 需要选择一个合适的函数式接口，其中要包含一个方法， 这个方法有一个 int 参数而且返回类型为 void: 处理 int 值的标准接口如下：

```java
interface IntConsumer{
	void accept(int value);
}

public class TestBase3{
	public static void repeat(int n,IntConsumer action){
		for (int i=0; i<n; i++) {
			action.accept(i);
		}
	}
	public static void main(String[] args){
		repeat(10,i->System.out.println("9-i="+(9-i)));
	}
}
```

基本类型 int、 long 和 double 的 34 个可能的规范。最好使用这些特殊化规范来减少自动装箱。

![基本类型的函数式接口](E:\pogject\javalearn\学习笔记\img\基本类型的函数式接口.png)

大多数标准函数式接口都提供了非抽象方法来生成或合并函数。 例如， Predicate.isEqual(a) 等同于 a::equals, 不过如果 a 为 null 也能正常工作。 已经提供了默认方法 and、or 和 negate 来合并谓词。 例如， Predicate.isEqual(a).or(Predicate.isEqual(b)) 就等同于 x->a.equals(x) || b.equals(x)。

4. 如果设计你自己的接口， 其中只有一个抽象方法， 可以用 @FunctionalInterface 注解来标记这个接口。 这样做有两个优点。 如果你无意中增加了另一个非抽象方法， 编译器会产生一个错误消息。 另外 javadoc 页里会指出你的接口是一个函数式接口。这样做有两个优点。 如果你无意中增加了另一个非抽象方法， 编译器会产生一个错误消息。 另外 javadoc 页里会指出你的接口是一个函数式接口。并不是必须使用注解根据定义， 任何有一个抽象方法的接口都是函数式接口。 不过使用 @FunctionalInterface 注解确实是一个很好的做法。

## 4  内部类

内部类（ inner class) 是定义在另一个类中的类。使用内部类的主要原因有以下三点：

- 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据。
- 内部类可以对同一个包中的其他类隐藏起来。
- 当想要定义一个回调函数且不想编写大量代码时，使用匿名 （anonymous) 内部类比较便捷。

### 4.1  使用内部类访问对象状态

1. 内部类的对象有一个隐式引用， 它引用了实例化该内部对象的外围类对象。通过这个指针， 可以访问外围类对象的全部状态。
2. 从传统意义上讲，一个方法可以引用调用这个方法的对象数据域。内部类既可以访问自身的数据域， 也可以访问创建它的外围类对象的数据域.
3. 只有内部类可以是私有类，而常规类只可以具有包可见性，或公有可见性。

```java
import java.util.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.Timer;

class TalkingClock{
	private int interval;
	private boolean beep;

	public TalkingClock(int interval,boolean beep){
		this.interval=interval;
		this.beep=beep;
	}
	public void start(){
        //TimePrinter 对象是由 TalkingClock 类的方法构造。
		ActionListener listener=new TimePrinter();
		Timer t=new Timer(interval,listener);
		t.start();
	}
    //当在 start 方法中创建了 TimePrinter 对象后， 编译器就会将 this 引用传递给当前的语音时钟的构造器：
	public class TimePrinter implements ActionListener{
        //因为 TimePrinter 类没有定义构造器， 所以编译器为这个类生成了一个默认的构造器，。
        /*
        public TimePrinter(TalkingGock clock) // automatically generated code
        {
        	outer = clock;
        }	
        */
		public void actionPerformed(ActionEvent event){
			System.out.println("the nowtime is "+new Date());
			if (beep) {
                //actionPerformed 方法在发出铃声之前检查了 beep 标志
                // TimePrinter 类没有实例域或者名为 beep 的变量，取而代之的是beep 引用了创建 TimePrinter 的 TalkingClock 对象的域。
				Toolkit.getDefaultToolkit().beep();
			}
		}
	}
}

public class TestBase3{
	public static void main(String[] args){
		TalkingClock clock=new TalkingClock(1000,true);
		clock.start();
		JOptionPane.showMessageDialog(null,"停止程序?");  //显示一个包含一条消息和 OK 按钮的对话框。
		System.exit(0);
	}
}
```

### 4.2  内部类的特殊语法规则

1. 事实上，使用外围类引用的正规语法还要复杂一些。表达式OwterC/ass.this表示外围类引用。 

2. 反过来，可以采用下列语法格式更加明确地编写内部对象的构造器：outerObject.new InnerClass{construction parameters)

   ```java
   		if (beep) {  //TalkingClock.this.beep
   				Toolkit.getDefaultToolkit().beep();
   			}
   		ActionListener listener=new TimePrinter();  //this.new TimePrinter()
   ```

3.  在外围类的作用域之外，可以这样引用内部类：OuterClass.InnerClass

4. 内部类中声明的所有静态域都必须是 final。原因很简单。 我们希望一个静态域只有一个实例， 不过对于每个外部对象， 会分别有一个单独的内部类实例。如果这个域不是 final, 它可能就不是唯一的。

5. 内部类不能有 static 方法。Java 语言规范对这个限制没有做任何解释。也可以允许有静态方法， 但只能访问外围类的静态域和方法。

### 4.3  内部类是否有用、必要和安全

1. 内部类的语法很复杂。它与访问控制和安全性等其他的语言特性的没有明显的关联。
2. 内部类是一种编译器现象， 与虚拟机无关。编译器将会把内部类翻译成用 $ (美元符号）分隔外部类名与内部类名的常规类文件， 而虚拟机则对此一无所知。
3. 由于内部类拥有访问特权， 所以与常规类比较起来功能更加强大。
4. 如果内部类访问了私有数据域， 就有可能通过附加在外围类所在包中的其他类访问它们， 但做这些事情需要高超的技巧和极大的决心。程序员不可能无意之中就获得对类的访问权限， 而必须刻意地构建或修改类文件才有可能达到这个目的。

### 4.4  局部内部类

```java
	public void start(){
		//局部内部类
		class TimePrinter implements ActionListener{
			public void actionPerformed(ActionEvent event){
				System.out.println("the nowtime is "+new Date());
				if (beep) {  //TalkingClock.this.beep
					Toolkit.getDefaultToolkit().beep();
				}
			}
		}
		ActionListener listener=new TimePrinter();  //this.new TimePrinter()
		Timer t=new Timer(interval,listener);
		t.start();
	}
```

1. 局部类不能用 public 或 private 访问说明符进行声明。它的作用域被限定在声明这个局部类的块中。
2. 局部类有一个优势， 即对外部世界可以完全地隐藏起来。 即使 TalkingClock 类中的其他代码也不能访问它。除 start 方法之外， 没有任何方法知道 TimePrinter 类的存在。
3. 局部类还有一个优点。它们不仅能够访问包含它们的外部类， 还可以访问局部变量。不过， 那些局部变量必须事实上为 final。这说明， 它们一旦赋值就绝不会改变。

### 4.5  由外部方法访问变量

```java
class TalkingClock{
	public void start(int interval,boolean beep){
		//局部内部类
		class TimePrinter implements ActionListener{
			public void actionPerformed(ActionEvent event){
				System.out.println("the nowtime is "+new Date());
				if (beep) {  //TalkingClock.this.beep
					Toolkit.getDefaultToolkit().beep();
				}
			}
		}
		ActionListener listener=new TimePrinter();  //this.new TimePrinter()
		Timer t=new Timer(interval,listener);
		t.start();
	}
}

public class TestBase3{
	public static void main(String[] args){
		TalkingClock clock=new TalkingClock();
		clock.start(1000,true);
		JOptionPane.showMessageDialog(null,"停止程序?");  //显示一个包含一条消息和 OK 按钮的对话框。
		System.exit(0);

	
	}
}
```

TalkingClock 类不再需要存储实例变量 beep 了，它只是引用 start 方法中的 beep参数变量。

1 ) 调用 start 方法。
2 ) 调用内部类 TimePrinter 的构造器， 以便初始化对象变量 listener。
3 ) 将 listener 引用传递给 Timer 构造器， 定时器开始计时， start 方法结束。此时，start方法的 beep 参数变量不复存在。
4 ) 然后，actionPerformed 方法执行 if (beep)...。

```java
		int counter = new int[l];

		for (int i = 0; i < dates.length; i ++)
			dates[i] = new Date(){
                public int compareTo(Date other){
                    counter[0]++;
                    return super.compareTo(other);
                }
			};
		}
```

### 4.6  匿名内部类

```java
public void start(int interval,boolean beep){
		//匿名内部类
		ActionListener listener=new ActionListener(){
            //创建一个实现 ActionListener 接口的类的新对象，需要实现的方法 actionPerformed 定 义 在 括 号 内。
			public void actionPerformed(ActionEvent event){
				System.out.println("the nowtime is "+new Date());
				if (beep) {  //TalkingClock.this.beep
					Toolkit.getDefaultToolkit().beep();
				}
			}
		};
		Timer t=new Timer(interval,listener);
		t.start();
	}
```

1. 假如只创建这个类的一个对象，就不必命名了。这种类被称为匿名内部类（anonymous inner class)。

2. 通常的语法格式为：

   ```java
   new SuperType(construction parameters){
   	inner class methods and data
   }
   ```

   其中， SuperType 可以是 ActionListener 这样的接口， 于是内部类就要实现这个接口。SuperType 也可以是一个类，于是内部类就要扩展它。

3. 由于构造器的名字必须与类名相同， 而匿名类没有类名， 所以， 匿名类不能有构造器。取而代之的是，将构造器参数传递给超类 （superclass) 构造器。

4. 尤其是在内部类实现接口的时候， 不能有任何构造参数。不仅如此，还要像下面这样提供一组括号：

   ```java
   new InterfaceType(){
   	methods and data
   }
   ```

5. Java 程序员习惯的做法是用匿名内部类实现事件监听器和其他回调。 如今最好还是使用 lambda 表达式。

   ```java
   	public void start(int interval,boolean beep){
   		
   		Timer t=new Timer(interval,event->{
   			System.out.println("the nowtime is "+new Date());
   			if (beep) {
   					Toolkit.getDefaultToolkit().beep();
   				}
   		});
   		t.start();
   	}
   ```

6. 双括号初始化” （double brace initialization), 这里利用了内部类语法。

   ```java
   invite(new ArrayList<String>(){{ add("Harry"); add("Tony"); }});
   ```

   注意这里的双括号。 外层括号建立了 ArrayList 的一个匿名子类。 内层括号则是一个对象构造块。

7. 生成曰志或调试消息时， 通常希望包含当前类的类名， 如：

```java
Systen.err.println("Something awful happened in " + getClass())；
    //不过， 这对于静态方法不奏效。毕竟， 调用 getClass 时调用的是 this.getClass(),而静态方法没有 this。所以应该使用以下表达式：
 new Object0{}.getCIass0-getEndosingClass0 // gets class of static method
    //在这里，newObject(){} 会建立 Object 的一个匿名子类的一个匿名对象，getEnclosingClass则得到其外围类， 也就是包含这个静态方法的类。
```

### 4.7  静态内部类

1. 有时候， 使用内部类只是为了把一个类隐藏在另外一个类的内部，并不需要内部类引用外围类对象。为此，可以将内部类声明为 static, 以便取消产生的引用。
2. 只有内部类可以声明为 static。静态内部类的对象除了没有对生成它的外围类对象的引用特权外， 与其他所冇内部类完全一样。
3. 在内部类不需要访问外围类对象的时候， 应该使用静态内部类。 有些程序员用嵌套类 （nested class ) 表示静态内部类。
4. 与常规内部类不同， 静态内部类可以有静态域和方法。
5. 声明在接口中的内部类自动成为 static 和 public 类。

```java
import java.util.*;
class ArrayAlg{
	public static class Pair{
		private double first;
		private double second;

		public Pair(double f,double s){
			first=f;
			second=s;
		}

		public double getFirst(){
			return first;
		}
		public double getSecond(){
			return second;
		}
	}
	public static Pair minmax(double[] values){
		double min=Double.POSITIVE_INFINITY;
		double max=Double.NEGATIVE_INFINITY;
		for (double v:values) {
			if (min>v) {
				min=v;
			}
			if (max<v) {
				max=v;
			}
		}
		return new Pair(min,max);

	}
}

public class TestBase3{
	public static void main(String[] args){
		double[] d=new double[5];
		for (int i=0; i<d.length; i++) {
			d[i]=100*Math.random();
		}

		System.out.println(Arrays.toString(d));
		//[7.803257533515728, 97.0222694732156, 18.359083423368983, 62.2407079145171, 36.27014599745496]最小值：7.803257533515728
		ArrayAlg.Pair p=ArrayAlg.minmax(d);  // 与前面例子中所使用的内部类不同， 在 Pair 对象中不需要引用任何其他的对象
		System.out.println("最小值："+p.getFirst());  //最小值：7.803257533515728
		System.out.println("最大值："+p.getSecond());  //最大值：97.0222694732156

	}
}
```

## 5  代理

### 5.1  何时使用代理

1. 代理类可以在运行时创建全新的类。这样的代理类能够实现指定的接口。尤其是，它具有下列方法：

   - 指定接口所需要的全部方法。

   - Object 类中的全部方法， 例如， toString、 equals 等。

     然而，不能在运行时定义这些方法的新代码。而是要提供一个调用处理器（ invocationhandler)。调用处理器是实现了 InvocationHandler 接口的类对象。在这个接口中只有一个方法：

     ```java
     Object invoke(Object proxy, Method method, Object [] args)
     ```

2. 无论何时调用代理对象的方法， 调用处理器的 invoke 方法都会被调用， 并向其传递Method 对象和原始的调用参数。 调用处理器必须给出处理调用的方式。

### 5.2  创建代理对象

1. 要想创建一个代理对象， 需要使用 Proxy 类的 newProxylnstance 方法。 这个方法有三个参数：

   - 一个类加栽器（class loader)。作为 Java 安全模型的一部分， 对于系统类和从因特网上下载下来的类，可以使用不同的类加载器。目前，用 null 表示使用默认的类加载器。
   - 一个 Class 对象数组， 每个元素都是需要实现的接口。
   - 一个调用处理器。

2. 使用代理可能出于很多原因， 例如：

   - 路由对远程服务器的方法调用。
   - 在程序运行期间，将用户接口事件与动作关联起来。
   - 为调试， 跟踪方法调用。

   使用代理对象对二分查找进行跟踪：

```java
import java.util.*;
import java.lang.reflect.*;

//调用器类
class TraceHandler implements InvocationHandler{
	private Object target;

	public TraceHandler(Object t){
		target=t;
	}
	public Object invoke(Object proxy,Method m,Object[] args) throws Throwable{
		//打印出方法名和参数
		System.out.print(target);
		System.out.print("."+m.getName()+"(");
		if (args!=null) {
			for (int i=0; i<args.length; i++){
				System.out.print(args[i]);
				if (i<args.length-1) {
					System.out.print(", ");
				}
			}
		}
        //// invoke actual method
		System.out.println(")");
		return m.invoke(target,args);
	}
}

public class TestBase3{
	public static void main(String[] args){
		Object[] elements=new Object[1000];
		for (int i=0; i<elements.length; i++) {
			//代理填充数组
			Integer value=i+1;
			InvocationHandler handler = new TraceHandler(value);
			Object proxy=Proxy.newProxyInstance(null,new Class[]{Comparable.class},handler);
			//由于数组中填充了代理对象， 所以 compareTo 调用了 TraceHander 类中的 invoke 方法。
			elements[i]=proxy;
		}
		//构建一个随机整数
		Integer key=new Random().nextInt(elements.length)+1;
		//搜索key
		int result=Arrays.binarySearch(elements,key);
		//输出结果
		if (result>0) {
			System.out.println("key="+key+",result="+elements[result]);
		}

	}
}
/*
Compiling TestBase3.java......
------Output------
500.compareTo(971)
750.compareTo(971)
875.compareTo(971)
938.compareTo(971)
969.compareTo(971)
985.compareTo(971)
977.compareTo(971)
973.compareTo(971)
971.compareTo(971)
971.toString()
key=971,result=971
*/
```

### 5.3  代理类的特性

1. 代理类是在程序运行过程中创建的。 然而， 一旦被创建， 就变成了常规类， 与虚拟机中的任何其他类没有什么区别。

2. 所有的代理类都扩展于 Proxy 类。一个代理类只有一个实例域—调用处理器，它定义在 Proxy 的超类中。 为了履行代理对象的职责， 所需要的任何附加数据都必须存储在调用处理器中。

3. 所有的代理类都覆盖了 Object 类中的方法 toString、 equals 和 hashCode。 如同所有的代理方法一样， 这些方法仅仅调用了调用处理器的 invoke。Object 类中的其他方法（如 clone和 getClass) 没有被重新定义。

4. 没有定义代理类的名字，Sun 虚拟机中的 Proxy 类将生成一个以字符串 $Proxy 开头的类名。

5. 对于特定的类加载器和预设的一组接口来说， 只能有一个代理类。 也就是说， 如果使用同一个类加载器和接口数组调用两次 newProxylustance 方法的话， 那么只能够得到同一个类的两个对象，也可以利用 getProxyClass 方法获得这个类：

   ```java
   Class proxyClass = Proxy.getProxyClass(null, interfaces);
   ```

6. 代理类一定是 public 和 final。 如果代理类实现的所有接口都是 public， 代理类就不属于某个特定的包；否则， 所有非公有的接口都必须属于同一个包，同时，代理类也属于这个包。

7. 可以通过调用 Proxy 类中的 isProxyClass 方法检测一个特定的 Class 对象是否代表一个代理类。

```java
java.Iang.reflect.InvocationHandler 1.3
	• Object invoke(Object proxy,Method method ,object[] args)  定义了代理对象调用方法时希望执行的动作。
 java.Iang.reflect.Proxy 1.3
    • static Class<?>   getProxyClass(Cl assLoader loader, Class<?>...    interfaces)
    返回实现指定接口的代理类。
    • static    Object    newProxyInstance(ClassLoader    loader, Class<?>[]    interfaces, InvocationHandler handler)
    构造实现指定接口的代理类的一个新实例。    所有方法会调用给定处理器对象的 invoke 方法。
    • static boolean isProxyClass(Class<?> cl)
    如果 cl 是一个代理类则返回 true。
```

