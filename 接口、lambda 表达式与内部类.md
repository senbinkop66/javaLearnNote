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

### 2.2  Comparator 接□









