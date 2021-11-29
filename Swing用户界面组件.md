# Swing用户界面组件

## 1  Swing 和模型-视图- 控制器设计模式

### 1.1  设计模式

- 模型-视图-控制器模式并不是 AWT 和 Swing 设计中使用的唯一模式。
- 容器和组件是“ 组合 （composite)” 模式
- 带滚动条的面板是“ 装饰器（decorator)” 模式
- 布局管理器是“ 策略（strategy)” 模式

### 1.2  模 型- 视 图-控 制 器 模 式

1. 每个组件都有三个要素：

   - 内容， 如： 按钮的状态 （是否按下 )， 或者文本框的文本。
   - 外观（颜色，大小等)。
   - 行为 （对事件的反应)。

2. 为了实现这样的需求， Swing 设计者采用了一种很有名的设计模式（design pattern ) : 模型 - 视图 - 控制器 （ model-view-controller) 模式。这种设计模式同其他许多设计模式一样，都遵循面向对象设计中的一个基本原则： 

   **限制一个对象拥有的功能数量。 不要用一个按钮类完成所有的事情， 而是应该让一个对象负责组件的观感， 另一个对象负责存储内容。**

3. 模型- 视图- 控制器（MVC) 模式告诉我们如何实现这种设计，实现三个独立的类：

   - 模型 （model): 存储内容。
   - 视图 （view): 显示内容。
   - 控制器 （controller): 处理用户输入。

4. 模型存储内容， 它没有用户界面。模型必须实现改变内容和查找内容的方法。模型是完全不可见的。显示存储在模型中的数据是视图的工作。

5. 模型- 视图-控制器模式的一个优点是一个模型可以有多个视图， 其中每个视图可以显示全部内容的不同部分或不同形式。当通过某一个视图的控制器对模型进行更新时， 模式会把这种改变通知给两个视图。视图得到通知以后就会自动地刷新。 

6. 控制器负责处理用户输入事件， 如点击鼠标和敲击键盘。然后决定是否把这些事件转化成对模型或视图的改变。

7. 在程序员使用 Swing 组件时， 通常不需要考虑模型 - 视图-控制器体系结构。 每个用户 界面元素都有一个包装器类（如 JButton 或 JTextField) 来保存模型和视图。当需要查询内容 (如文本域中的文本）时， 包装器类会向模型询问并且返回所要的结果。 当想改变视图时（例 如， 在一个文本域中移动光标位置)， 包装器类会把此请求转发给视图。然而，有时候包装器 转发命令并不得力。在这种情况下， 就必须直接地与模型打交道（不必直接操作视图—这 是观感代码的任务)。

### 1.3  Swing 按钮的模型- 视图- 控制器分析

1. 对于大多数组件来说， 模型类将实现一个名字以 Model 结尾的接口， 例如， 按钮就实现了 ButtonModel 接口。实现了此接口的类可以定义各种按钮的状态。

2. ButtonModel 接口的属性

   | 属性名        | 值                                                |
   | ------------- | ------------------------------------------------- |
   | actionCommand | 与按钮关联的动作命令字符串                        |
   | mnemonic      | 按钮的快捷键                                      |
   | armed         | 如果按钮被按下且鼠标仍在按钮上则为 true           |
   | enabled       | 如果按钮是可选择的则为 true                       |
   | pressed       | 如果按钮被按下且鼠标按键没有释放则为 true         |
   | rollover      | 如果鼠标在按钮之上则为 true                       |
   | selected      | 如果按钮已经被选择（用于复选框和单选钮) 则为 true |

3. 每个 JButton 对象都存储了一个按钮模型对象， 可以用下列方式得到它的引用。

   ]Button button = new JButton("Blue");
   ButtonModel model = button.getModel () ;

4. 模型不存储按钮标签或者图标。对于一个按钮来说， 仅凭模型无法知道它的外观 。

5. 需要注意的是， 同样的模型（即 DefaultButtonModel ) 可用于下压按钮、 单选按钮、复选框、 甚至是菜单项。 当然， 这些按钮都有各自不同的视图和控制器。 

6. 当使用 Metal 观感时，JButton 类用 BasicButtonUI 类作为其视图； 用 ButtonUIListener 类作为其控制器。通常， 每个 Swing 组件都有一个相关的后缀为 UI 的视图对象， 但并不是所有的 Swing 组件都有专门的控制器对象。

## 2  布局管理概述

流布局管理器（ flow layout manager) 管理， 这是面板的默认布局管理器。当一行的空间不够时，会将显示在新的一行上。另外， 按钮总是位于面板的中央， 即使用户对框架进行缩放也是如此。

1. 通常， 组件放置在容器中， 布局管理器决定容器中的组件具体放置的位置和大小。

2. 按钮、文本域和其他的用户界面元素都继承于Component 类， 组件可以放置在面板这样的容器中。由于 Container 类继承于 Component 类， 所以容器也可以放置在另一个容器中。

3. Component 的类层次结构

   ![Component 的类层次结构](E:\pogject\javalearn\学习笔记\img\Component 的类层次结构.png)

继承层次有两点显得有点混乱。

- 首先， 像 JFrame 这样的顶层窗口是 Container 的子类， 所以也是 Component 的子类， 但却不能放在其他容器内。
- 另外，JComponent 是 Container 的子类， 但不直接继承 Component, 因此， 可以将其他组件添置到 JButton 中。（但无论如何， 这些组件无法显示出来）。

4. 每个容器都有一个默认的布局管理器，但可以重新进行设置。 

   ```java
   panel.setLayout(new CridLayout(4, 4));
   //这个面板将用 GridLayout 类布局组件。可以往容器中添加组件。容器的 add 方法将把组件和放置的方位传递给布局管理器。
   
   Void SetLayout(LayoutManager m)  //为容器设置布局管理器
   
   Component add(Component c)    //将组件添加到容器中，并返回组件的引用
   Component add(Component c, Object constraints)   // 参数： c  要添加的组件 constraints  布局管理器理解的标识符
       
   FIowLayout ()   // 构造一个新的 FlowLayout 对象。
   FIowLayout(int align)  //参数： align   LEFT、 CENTER 或者 RIGHT
   FIowLayout (int align, int hgap , int vgap)  //  以像素为单位的水平、垂直间距（如果为负值，则强行重叠）
    
       BorderLayout()  //构造一个新的 BorderLayout 对象。
       BorderLayout( int hgap, int vgap)  //以像素为单位的水平垂直间距（如果为负值， 则强行重叠）
       
   ```
   

### 2.1  边框布局

1. 边框布局管理器（border layout manager) 是每个 JFrame的内容窗格的默认布局管理器。

2.  流布局管理器完全控制每个组件的放置位置，边框布局管理器则不然，它允许为每个组件选择一个放置位置。 可以选择把组件放在内容窗格的中部、 北部、南部、 东部或者西部。

   frame,add(component, BorderLayout.SOUTH);

3. 先放置边缘组件，剩余的可用空间由中间组件占据。当容器被缩放时，边缘组件的尺寸不会改变， 而中部组件的大小会发生变化。

4. 在添加组件时可以指定 BorderLayout 类中的 CENTER、 NORTH、 SOUTH、EAST和 WEST 常量。并非需要占用所有的位置，如果没有提供任何值， 系统默认为 CENTER。

5. 与流布局不同，边框布局会扩展所有组件的尺寸以便填满可用空间（流布局将维持每个组件的最佳尺寸)。

   frame.add(yellowButton, BorderLayout.SOUTH); // don't

    按钮扩展至填满框架的整个南部区域。而且，如果再将另外一个按钮添加到南部区域， 就会取代第一个按钮。解决这个问题的常见方法是使用另外一个面板（panel)。边框布局管理器将会扩展面板大小， 直至填满整个南部区域。

   ```java
   JPanel panel = new JPanel();
   panel.add(yellowButton) ;
   panel.add(blueButton);
   panel.add(redButton);
   frame.add(panel , BorderLayout.SOUTH);
   ```

### 2.2  网格布局

1. 网格布局像电子数据表一样， 按行列排列所有的组件。不过，它的每个单元大小都是一样的。
2. 网格布局对象的构造器中，需要指定行数和列数.
3. 添加组件， 从第一行的第一列开始， 然后是第一行的第二列， 以此类推。
4. 极少有像计算器这样整齐的布局。实际上， 在组织窗口的布局时小网格（通常只有一行或者一列）比较有用。 

计算器程序

```java
import java.awt.*;
import javax.swing.*;
import java.awt.event.*;

class TestFrame extends JFrame{
	private static final int DEFAULT_WIDTH=600;
	private static final int DEFAULT_HEIGHT=400;

	public TestFrame(){
		//构造器将框架大小设置为 300 x 200 像素。
		setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);
		CalculatorPanel panel1=new CalculatorPanel();
		add(panel1);
		pack();
	}

}


class CalculatorPanel extends JPanel{
	private JButton display;
	private JPanel panel;
	private double result;
	private String lastCommand;
	private boolean start;

	public CalculatorPanel(){
		setLayout(new BorderLayout());
		result=0;
		lastCommand="=";
		start=true;

		display=new JButton("0");
		display.setEnabled(false);
		add(display,BorderLayout.NORTH);

		ActionListener insert=new InsertAction();
		ActionListener command=new CommandAction();

		panel=new JPanel();
		panel.setLayout(new GridLayout(4,4));

		addButton("7",insert);
		addButton("8",insert);
		addButton("9",insert);
		addButton("/",command);
		addButton("4",insert);
		addButton("5",insert);
		addButton("6",insert);
		addButton("*",command);
		addButton("1",insert);
		addButton("2",insert);
		addButton("3",insert);
		addButton("-",command);
		addButton("0",insert);
		addButton(".",insert);
		addButton("=",command);
		addButton("+",command);

		add(panel,BorderLayout.CENTER);
	}
	private void addButton(String label,ActionListener listener){
		JButton button=new JButton(label);
		button.addActionListener(listener);
		panel.add(button);
	}
	//内部类
	private class InsertAction implements ActionListener{
		public void actionPerformed(ActionEvent event){
			String input=event.getActionCommand();
			if (start) {
				display.setText("");
				start=false;
			}
			display.setText(display.getText()+input);
		}
	}
	//内部类
	private class CommandAction implements ActionListener{
		public void actionPerformed(ActionEvent event){
			String command=event.getActionCommand();
			if (start) {
				if (command.equals("-")) {
					display.setText(command);
					start=false;
				}else{
					lastCommand=command;
				}
			}else{
				calculate(Double.parseDouble(display.getText()));
				lastCommand=command;
				start=true;
			}
		}
	}
	public void calculate(double x){
		if (lastCommand.equals("+")) {
			result+=x;
		}else if(lastCommand.equals("-")){
			result-=x;
		}else if(lastCommand.equals("*")){
			result*=x;
		}else if(lastCommand.equals("/")){
			result/=x;
		}else if(lastCommand.equals("=")){
			result=x;
		}
		display.setText(""+result);
	}
}

public class TestApplet2{
	public static void main(String[] args) {
		EventQueue.invokeLater(()->{
			JFrame frame=new TestFrame();
			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			frame.setTitle("计算器");  //设置框架标题
			frame.setLocation(300,300);
			frame.setVisible(true);  //使其可见性
		});
	}
}

 GridLayout(int rows, int columns)
 GridLayout(int rows, int columns, int hgap, int vgap)    
     //构造一个新的 GridLayout 对象。rows 或者 columns 可以为零， 但不能同时为零， 指定的每行或每列的组件数量可以任意的。
```

## 3  文本输入

1. 文本域（JTextField) 和文本区 （ JTextArea) 组件用于获取文本输人。文本域只能接收单行文本的输人， 而文本区能够接收多行文本的输人。JPassword 也只能接收单行文本的输人，但不会将输入的内容显示出来。

2. 这三个类都继承于 JTextComponent 类。由于 JTextComponent 是一个抽象类，所以不能够构造这个类的对象。

3. 在 Java 中常会看到这种情况。在査看 API 文档时，发现自己正在寻找的方法实际上来自父类 JTextComponent, 而不是来自派生类自身。例如，在一个文本域和文本区内获取（get)、 设置（set) 文本的方法实际上都是 JTextComponent 类中的方法。

   ```java
   javax.swing.text.JTextComponent 
   	String getText()
   	void setText(String text)  //获取或设置文本组件中的文本。
   	boolean isEditable()
   	void setEditabe(booleanb)  //获取或设置 editable 特性， 这个特性决定了用户是否可以编辑文本组件中的内容。
   ```

### 3.1  文本域

1. 把文本域添加到窗口的常用办法是将它添加到面板或者其他容器中，这与添加按钮完全一样：

   ```java
   JPanel panel = new JPanel();
   JTextField textField = new JTextField("Default input", 20);  //构造一个给定列数、 给定初始字符串的 JTextField 对象。
   panel,add(textField);
   
   int getColumns()
   textField.setColumns(10);  //获取或设置文本域使用的列数。
   panel.revalidate();  //重新计算组件的位置和大小。
   
   JTextField textField = new JTextField(20);  //构造一个给定列数的空 JTextField 对象。
   
   textField.setText("Hello!");
   
   String text = textField.getText().trim()；
       
   void validate( )  //重新计算组件的位置和大小。如果组件是容器， 容器中包含的所有组件的位置和大小也被重新计算。
   ```

   这段代码将添加一个文本域， 同时通过传递字符串“ Default input” 进行初始化。构造 器的第二个参数设置了文本域的宽度。在这个示例中， 宽度值为 20“ 列”。但是，这里所说 的列不是一个精确的测量单位。一列就是在当前使用的字体下一个字符的宽度。 如果希望文 本域最多能够输人 n 个字符， 就应该把宽度设置为 n 列。在实际中， 这样做效果并不理想， 应该将最大输人长度再多设 1 ~ 2 个字符。列数只是给 AWT 设定首选（ preferred) 大小的一 个提示。如果布局管理器需要缩放这个文本域， 它会调整文本域的大小。在 JTextField 的构 造器中设定的宽度并不是用户能输人的字符个数的上限。 用户可以输入一个更长的字符串， 但是当文本长度超过文本域长度时输人就会滚动。用户通常不喜欢滚动文本域， 因此应该尽 量把文本域设置的宽一些。如果需要在运行时重新设置列数，可以使用 setColumns 方法。

2. 使用 setColumns 方法改变了一个文本域的大小之后， 需要调用包含这个文本框的容器的 revalidate 方法。

   - revalidate 方法会重新计算容器内所有组件的大小， 并且对它们重新进行布局。 调用revalidate 方法以后， 布局管理器会重新设置容器的大小， 然后就可以看到改变尺寸后的文本域了。
   - revalidate 方法是 JComponent 类中的方法。 它并不是马上就改变组件大小， 而是给 这个组件加一个需要改变大小的标记。这样就避免了多个组件改变大小时带来的重复计 算。但是， 如果想重新计算一个 JFrame 中的所有组件， 就必须调用 validate 方法. JFrame 没有扩展 JComponent。

3. 通常情况下， 希望用户在文本域中键入文本（或者编辑已经存在的文本)。文本域一般初始为空白。只要不为 JTextField 构造器提供字符串参数，就可以构造一个空白文本域。

4. 可以在任何时候调用 setText 方法改变文本域中的内容。 这个方法是从前面提到的JTextComponent 中继承而来的。

5. 可以调用 getText 方法来获取用户键人的文本。这个方法返回用户输人的文本。 如果想要将 getText 方法返回的文本域中的内容的前后空格去掉， 就应该调用 trim 方法。

6. 如果想要改变显示文本的字体， 就调用 setFont 方法。

### 3.2  标签和标签组件

1. 标签是容纳文本的组件， 它们没有任何的修饰（例如没有边缘 )， 也不能响应用户输入。可以利用标签标识组件。

2. 与按钮不同， 文本域没有标识它们的标签要想用标识符标识这种不带标签的组件， 应该
   1 ) 用相应的文本构造一个 JLabel 组件。
   2 ) 将标签组件放置在距离需要标识的组件足够近的地方， 以便用户可以知道标签所标识的组件。

3. 儿abel 的构造器允许指定初始文本和图标， 也可以选择内容的排列方式。可以用 Swing Constants 接口中的常量来指定排列方式。在这个接口中定义了几个很有用的常量， 如 LEFT、 RIGHT、CENTER、NORTH、 EAST 等。JLabel 是实现这个接口的一个 Swing 类。

   ```java
   ]Label label = new ]Label("User name: ", SwingConstants.RIGHT);
   //
   JLabel label = new JLabel("User name: ", 3Label.RIGHT);
   
   label = new JLabel("<html><b>Required</b> entry:</html>°);
                      
   JLabel (String text )
   JLabel (Icon icon)
   JLabel (String text, int align)
    JLabel (String text, Icon Icon, int align)    //构造一个标签。
    
   String getText( )
   void setText(String text )  //     获取或设置标签的文本。    
   
   Icon getIcon( )
   void setIcon(Icon Icon)  //  获取或设置标签的图标。                 
   ```

4. 利用 setText 和 setlcon 方法可以在运行期间设置标签的文本和图标。

5. 可以在按钮、 标签和菜单项上使用无格式文本或 HTML 文本。 我们不推荐在按钮 上使用 HTML 文本—这样会影响观感。 但是 HTML 文本在标签中是非常有效的。 只要 简单地将标签字符串放置在 <html>...</html> 中即可。

6. 与其他组件一样，标签也可以放置在容器中。这就是说，可以利用前面介绍的技巧将标签放置在任何需要的地方。

### 3.3  密码域

1. 密码域是一种特殊类型的文本域。为了避免有不良企图的人看到密码， 用户输入的字符不显示出来。 每个输人的字符都用回显字符 （ echo character) 表示， 典型的回显字符是星号(*)。Swing 提供了 JPasswordField 类来实现这样的文本域。

2. 密码域采用与常规的文本域相同的模型来存储数据， 但是，它的视图却改为显示回显字符，而不是实际的字符。

   ```java
   JPasswordField(String text, int columns)  //构造一个新的密码域对象。
   void setEchoChar(char echo)  /*为密码域设置回显字符。注意： 独特的观感可以选择自己的回显字符。0 表示重新设置为默认的回显字符。*/
   char[ ] getPassword()  /*返回密码域中的文本。 为了安全起见， 在使用之后应该覆写返回的数组内容 （密码并不是以 String 的形式返回，这是因为字符串在被垃圾回收器回收之前会一直驻留在虚拟机中）。*/
   ```

### 3.4  文本区

1. 有时， 用户的输人超过一行。 正像前面提到的， 需要使用 JTextArea 组件来接收这样的 输入。当在程序中放置一个文本区组件时， 用户就可以输 人多行文本，并用 ENTER 键换行。每行都以一个“ \n” 结 尾。

2. 在 JTextArea 组件的构造器中，可以指定文本区的行数和列数。 

   ```java
   textArea = new JTextArea(8, 40);
   
   textArea.setLineWrap(true):// long lines are wrapped
   ```

3. 与文本域一样。 出于稳妥的考虑， 参数 columns 应该设置得大一些。另外，用户并不受限于输人指定的行数和列数。当输人过长时，文本会滚动。

4. 可以用 setColumns 方法改变列数， 用 setRows 方法改变行数。这些数值只是首选大小一布局管理器可能会对文本区进行缩放。

5. 如果文本区的文本超出显示的范围， 那么剩下的文本就会被剪裁掉。可以通过开启换行特性来避免裁剪过长的行。换行只是视觉效果；文档中的文本没有改变，在文本中并没有插入“ \n” 字符。

### 3.5  滚动窗格

1. 在 Swing 中， 文本区没有滚动条。 如果需要滚动条， 可以将文本区插人到滚动窗格(scroll pane) 中。

   ```
   textArea = new JTextArea(8, 40);
   JScrollPane scrollPane = new JScrollPane(textArea):
   ```

   现在滚动窗格管理文本区的视图。 如果文本超出了文本区可以显示的范围， 滚动条就会自动地出现， 并且在删除部分文本后， 当文本能够显示在文本区范围内时， 滚动条会再次自动地消失。滚动是由滚动窗格内部处理的， 编写程序时无需处理滚动事件。

2. 这是一种为任意组件添加滚动功能的通用机制， 而不是文本区特有的。 也就是说， 要想为组件添加滚动条， 只需将它们放人一个滚动窗格中即可。

3. JTextArea 组件只显示无格式的文本， 没有特殊字体或者格式设置。 如果想要显示格式化文本（如 HTML ), 就需要使用 JEditorPane 类。

```java
import java.awt.*;
import javax.swing.*;
import java.awt.event.*;
import java.awt.BorderLayout;
import java.awt.GridLayout;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.SwingConstants;


class TestFrame extends JFrame{
	private static final int DEFAULT_WIDTH=600;
	private static final int DEFAULT_HEIGHT=600;

	private static final int TEXTAREA_ROWS=8;
	private static final int TEXTAREA_COLUMNS=20;

	public TestFrame(){
		
		//setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);

		JTextField textField=new JTextField();
		JPasswordField passwordField=new JPasswordField();

		JPanel northPane=new JPanel();
		northPane.setLayout(new GridLayout(2,2));
		northPane.add(new JLabel("user name: ",SwingConstants.RIGHT));
		northPane.add(textField);
		northPane.add(new JLabel("Password: : ",SwingConstants.RIGHT));
		northPane.add(passwordField);

		add(northPane,BorderLayout.NORTH);

		JTextArea textarea=new JTextArea(TEXTAREA_ROWS,TEXTAREA_COLUMNS);
		JScrollPane scrollPane=new JScrollPane(textarea);  //创建一个滚动窗格， 用来显示指定组件的内容。 组件内容超过显示范围时， 滚动条会自动地出现。
		add(scrollPane,BorderLayout.CENTER);

		JPanel southPane=new JPanel();

		JButton insertButton=new JButton("Insert");
		southPane.add(insertButton);
		insertButton.addActionListener(event->{
			textarea.append("user name: "+textField.getText()+" Password: "+new String(passwordField.getPassword())+"\n");
		});

		add(southPane,BorderLayout.SOUTH);
		pack();
	}
}


public class TestApplet3{
	public static void main(String[] args) {
		EventQueue.invokeLater(()->{
			JFrame frame=new TestFrame();
			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			frame.setTitle("文本框");  //设置框架标题
			frame.setLocation(300,300);
			frame.setVisible(true);  //使其可见性
		});
	}
}
```

## 4  选择组件

### 4.1  复选框

1. 如果想要接收的输人只是“ 是” 或“ 非”， 就可以使用复选框组件。复选框自动地带有标识标签。用户通过点击某个复选框来选择相应的选项， 再点击则取消选取。 当复选框获得焦点时， 用户也可以通过按空格键来切换选择。

2. 复选框需要一个紧邻它的标签来说明其用途。 在构造器中指定标签文本。

   ```java
   bold = new ]CheckBox("Bold");
   bold,setSelected(true);
   ActionListener listener = . . .
   bold.addActionListenerflistener);
   italic.addActionListener(listener);
   
   ActionListener listener = event -> {
       int mode = 0;
       if (bold.isSelectedO) mode += Font.BOLD;
       if (italic.isSelectedO) mode += Font.ITALIC;
       label.setFont(new Font(Font.SERIF, mode, FONTSIZE));
   }；
   ```

3. 可以使用 setSelected 方法来选定或取消选定复选框。isSelected 方法将返回每个复选框的当前状态。 如果没有选取则为 false, 否则为 true。

4. 当用户点击复选框时将触发一个动作事件。通常， 可以为复选框设置一个动作监听器。在下面程序中， 两个复选框使用了同一个动作监听器。

5. actionPerformed 方法查询 bold 和 italic 两个复选框的状态，并且把面板中的字体设置为常规、 加粗、 倾斜或者粗斜体。

   ```java
   class CheckBoxFrame extends JFrame{
   	private JLabel label;
   	private JCheckBox bold;
   	private JCheckBox italic;
   	private static final int FONTSIZE=24;
   
   	public CheckBoxFrame(){
   		label=new JLabel("You will never walk alone!");
   		label.setFont(new Font("Serif",Font.BOLD,FONTSIZE));
   		add(label,BorderLayout.CENTER);
   
   		ActionListener listener=event->{
   			int mode=0;
   			if (bold.isSelected()) mode+=Font.BOLD;
   			if (italic.isSelected()) mode+=Font.ITALIC;
   			label.setFont(new Font("Serif",mode,FONTSIZE));
   		};
   
   		JPanel buttonPanel=new JPanel();
   
   		bold=new JCheckBox("Bold");  //构造一个复选框， 初始没有被选择。
   		bold.addActionListener(listener);
   		bold.setSelected(true);  //获取或设置复选框的选择状态。
   		buttonPanel.add(bold);
   		italic=new JCheckBox("Italic");
   		italic.addActionListener(listener);
   		buttonPanel.add(italic);
   
   		add(buttonPanel,BorderLayout.SOUTH);
   		pack();
   	}
   }
   
   public class TestApplet3{
   	public static void main(String[] args) {
   		EventQueue.invokeLater(()->{
   			JFrame frame=new CheckBoxFrame();
   			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
   			frame.setTitle("复选框");  //设置框架标题
   			frame.setLocation(300,300);
   			frame.setVisible(true);  //使其可见性
   		});
   	}
   }
   
   ```

### 4.2  单选钮

1. 需要用户只选择几个选项当中的一个。当用户选择另一项的时候， 前一项就自动地取消选择。这样一组选框通常称为单选钮组（Radio Button Group),

2. 在 Swing 中，实现单选钮组非常简单。为单选钮组构造一个 ButtonGroup 的对象。然后，再将 JRadioButton 类型的对象添加到按钮组中。按钮组负责在新按钮被按下时，取消前一个被按下的按钮的选择状态。

   ```java
   ButtonGroup group = new ButtonGroupO;
   JRadioButton smallButton = new ]RadioButton("Small", false);
   group.add(smallButton);
   JRadioButton mediumButton = new ]RadioButton("Medium", true);
   group,add(mediumButton);
   
   ActionListener listener = event ->
   	label.setFont(new FontC'Serif", Font.PLAIN, size));
   ```

   构造器的第二个参数为 true 表明这个按钮初始状态是被选择，其他按钮构造器的这个参数为 false。注意， 按钮组仅仅控制按钮的行为， 如果想把这些按钮组织在一起布局， 需要把它们添加到容器中， 如 JPanel。

3. 单选钮的事件通知机制与其他按钮一样。当用户点击一个单选钮时， 这个按钮将产生一个动作事件。

4. 每个单选钮都对应一个不同的监听器对象。 每个监听器都非常清楚所要做的事情—把字体尺寸设置为一个特定值。在复选框示例中， 使用的是一种不同的方法， 两个复选框共享一个动作监听器。这个监听器调用一个方法来检查两个复选框的当前状态。

5. 只有在通过 setActionCommand 命令明确地为所有单选钮设定动作命令后， 才能够通过调用方法 buttonGroup.getSelection().getActionCommand() 获得当前选择的按钮的动作命令。

```java
class RadioButtonFrame extends JFrame{
	private JLabel label;
	private JPanel buttonPanel;
	private ButtonGroup group;
	private JCheckBox italic;
	private static final int FONTSIZE=24;

	public RadioButtonFrame(){
		label=new JLabel("You will never walk alone!");
		label.setFont(new Font("Serif",Font.PLAIN,FONTSIZE));
		add(label,BorderLayout.CENTER);

		buttonPanel=new JPanel();
		group=new ButtonGroup();

		addRadioButton("Small",8);
		addRadioButton("Medium",12);
		addRadioButton("Large",18);
		addRadioButton("Extra large",36);

		add(buttonPanel,BorderLayout.SOUTH);
		pack();

		
	}
	public void addRadioButton(String name,int size){
		boolean isSelected=size==FONTSIZE;
		JRadioButton button=new JRadioButton(name,isSelected);  //用给定的标签和初始状态构造一个单选钮。
		group.add(button);  //将按钮添加到组中。
		buttonPanel.add(button);

		ActionListener listener=event->label.setFont(new Font("Serif",Font.PLAIN,size));
		button.addActionListener(listener);
	}
}

public class TestApplet3{
	public static void main(String[] args) {
		EventQueue.invokeLater(()->{
			JFrame frame=new RadioButtonFrame();
			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			frame.setTitle("单选按钮");  //设置框架标题
			frame.setLocation(300,300);
			frame.setVisible(true);  //使其可见性
		});
	}
}

```

### 4.3  边框

1. 如果在一个窗口中有多组单选按钮， 就需要用可视化的形式指明哪些按钮属于同一组。 Swing 提供了一组很有用的边框（ borders) 来解决这个问题。可以在任何继承了 JComponent 的组件上应用边框。 最常用的用途是在一个面板周围放置一个边框， 然后用其他用户界面元 素 （如单选钮）填充面板。
2. 有几种不同的边框可供选择，但是使用它们的步骤完全一样。
   - 调用 BorderFactory 的静态方法创建边框。凹斜面 、凸斜面 、蚀刻 、直 线 、蒙版 、空（只是在组件外围创建一些空白空间）
   - 可以给边框添加标题， 具体的实现方法是将边框传递给BroderFactory.createTitledBorder。
   - 如果确实想把一切凸显出来， 可以调用下列方法将几种边框组合起来使用：BorderFactory.createCompoundBorder。
   - 调用 JComponent 类中 setBorder 方法将结果边框添加到组件中。
3. 不同的边框有不同的用于设置边框的宽度和颜色的选项。

```java
class BorderFrame extends JFrame{
	private JPanel demoPanel;
	private JPanel buttonPanel;
	private ButtonGroup group;

	public BorderFrame(){
		demoPanel=new JPanel();
		buttonPanel=new JPanel();
		group=new ButtonGroup();
		//创建一个具有凹面或凸面效果的边框。
		addRadioButton("Lowered bevel",BorderFactory.createLoweredBevelBorder());
		addRadioButton("Raised bevel",BorderFactory.createRaisedBevelBorder());
		addRadioButton("Etched",BorderFactory.createEtchedBorder());
		//创建一个简单的直线边框。
		addRadioButton("Line",BorderFactory.createLineBorder(Color.BLUE));
		//创建一个用 color 颜色或一个重复 （repeating ) 图标填充的粗的边框。
		addRadioButton("Matte",BorderFactory.createMatteBorder(10,10,10,10,Color.RED));
		//创建一个空边框。
		addRadioButton("Empty",BorderFactory.createEmptyBorder());
		//创建一个具有 3D 效果的直线边框。
		Border etched=BorderFactory.createEtchedBorder(Color.GREEN,Color.BLUE);
		//创建一个具有给定特性的带标题的边框。
		Border titled=BorderFactory.createTitledBorder(etched,"Border types");
		buttonPanel.setBorder(titled);

		setLayout(new GridLayout(2,1));
		add(buttonPanel);
		add(demoPanel);
		pack();
	}
	public void addRadioButton(String name,Border b){
		JRadioButton button=new JRadioButton(name);  //用给定的标签和初始状态构造一个单选钮。
		//设置这个组件的边框。
		button.addActionListener(event->demoPanel.setBorder(b));
		group.add(button);  //将按钮添加到组中。
		buttonPanel.add(button);
	}
}

public class TestApplet3{
	public static void main(String[] args) {
		EventQueue.invokeLater(()->{
			JFrame frame=new BorderFrame();
			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
			frame.setTitle("边框");  //设置框架标题
			frame.setLocation(300,300);
			frame.setVisible(true);  //使其可见性
		});
	}
}
```

### 4.4  组合框

1. 如果有多个选择项， 使用单选按钮就不太适宜了， 其 原因是占据的屏幕空间太大。 这时就可以选择组合框。 当 用户点击这个组件时， 选择列表就会下拉出来， 用户可以 从中选择一项。

2. 如果下拉列表框被设置成可编辑 （ editable ), 就可以像编辑文本一样编辑当前的选项内容。鉴于这个原因， 这种组件被称为组合框（ combo box),它将文本域的灵活性与一组预定义的选项组合起来。JComboBox 类提供了组合框的组件。

3. 在 Java SE 7 中，JComboBox 类是一个泛型类。 例如，JComboBox<String> 包含 String类型的对象， JComboBox<Integer> 包含整数。

4. 调用 setEditable 方法可以让组合框可编辑。 注意， 编辑只会影响当前项， 而不会改变列表内容。

5. 可以调用 getSelectedltem 方法获取当前的选项， 如果组合框是可编辑的， 当前选项则是可以编辑的。不过，对于可编辑组合框， 其中的选项可以是任何类型，这取决于编辑器（即由编辑器获取用户输人并将结果转换为一个对象 )。 如果你的组合框不是可编辑的， 最好调用

   **combo.getltemAt(combo.getSelectedlndex())**

   这会为所选选项提供正确的类型。

6. 可以调用 addltem 方法增加选项。这个方法将字符串添加到列表的尾部。可以利用 insertltemAt 方法在列表的任何位置插人一个新选项

   faceCombo.insertltemAt("Monospaced", 0); // add at the beginning

7. 可以增加任何类型的选项，组合框可以调用每个选项的 toString 方法显示其内容。

8. 如果需要在运行时删除某些选项，可以使用 removeltem 或者 removeltemAt 方法，使用哪个方法将取决于参数提供的是想要删除的选项内容， 还是选项位置。

   faceCombo.removeltem("Monospaced");
   faceCombo.removeltemAt(0): // remove first item

9. 调用 removeAllltems 方法将立即移除所有的选项。

10. 如果需要往组合框中添加大量的选项，addltem 方法的性能就显得很差了 n 取 而 代之的是构造一个 DefaultComboBoxModel, 并调用 addElement 方法进行加载， 然后再调用 JComboBox 中的 setModel 方法。

11. 当用户从组合框中选择一个选项时，组合框就将产生一个动作事件。为了判断哪个选项被选择， 可以通过事件参数调用 getSource 方法来得到发送事件的组合框引用， 接着调用getSelectedltem 方法获取当前选择的选项。需要把这个方法的返回值转化为相应的类型，通常是 String 型。

12. 如果希望持久地显示列表， 而不是下拉列表， 就应该使用 _iList 组件。

    ```java
    class ComboBoxFrame extends JFrame{
    	private JLabel label;
    	private JComboBox<String> faceCombo;
    	private static final int DEFAULT_SIZE=24;
    
    	public ComboBoxFrame(){
    		label=new JLabel("You will never walk alone!");
    		label.setFont(new Font("Serif",Font.PLAIN,DEFAULT_SIZE));
    		add(label,BorderLayout.CENTER);
    
    		faceCombo=new JComboBox<>();
    		//把一个选项添加到选项列表中。
    		faceCombo.addItem("Serif");
    		faceCombo.addItem("SansSerif");
    		faceCombo.addItem("Monospaced");
    		faceCombo.addItem("Dialog");
    		faceCombo.addItem("DialogInput");
    		faceCombo.addItem("宋体");
    
    		//返回当前选择的选项。
    		faceCombo.addActionListener(event->{
    			label.setFont(new Font(faceCombo.getItemAt(faceCombo.getSelectedIndex()),Font.PLAIN,DEFAULT_SIZE));
    		});
    
    		JPanel comboPanel=new JPanel();
    		comboPanel.add(faceCombo);
    		add(comboPanel,BorderLayout.SOUTH);
    		pack();
    	}
    }
    
    public class TestApplet3{
    	public static void main(String[] args) {
    		EventQueue.invokeLater(()->{
    			JFrame frame=new ComboBoxFrame();
    			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    			frame.setTitle("组合框");  //设置框架标题
    			frame.setLocation(300,300);
    			frame.setVisible(true);  //使其可见性
    		});
    	}
    }
    ```

### 4.5  滑动条

1. 组合框可以让用户从一组离散值中进行选择。 滑动条允许进行连续值的选择， 例如， 从 1 ~ 100 之间选择任意数值。

2. 通常， 可以使用下列方式构造滑动条：

   JSlider slider = new ]Slide「(min, max, initialValue) ;

   如果省略最小值、 最大值和初始值， 其默认值分别为 0、100 和 50。

3. 如果需要垂直滑动条， 可以按照下列方式调用构造器：

   ]Slider slider = new ]SIider(SwingConstants.VERTICAL, min, max, initialValue);

4. 当用户滑动滑动条时， 滑动条的值就会在最小值和最大值之间变化。 当值发生变化时， ChangeEvent 就会发送给所有变化的监听器。 为了得到这些改变的通知，需要调用 addChangeListener 方法并且安装一个实现了 ChangeListener 接口的对象。这个接口只有一个方法 StateChanged。在这个方法中，可以获取滑动条的当前值：

   ```java
   ChangeListener listener = event -> {
       JSlider slider = (]Slider) event.getSource()；
       int value - slider.getValueO;
   }; 
   
   slider.setMajorTickSpacing(20);
   slider.setMinorTickSpacing(5);
   ```

5. 可以通过显示标尺（tick ) 对滑动条进行修饰。上述滑动条在每 20 个单位的位置显示一个大标尺标记， 每 5 个单位的位置显示一个小标尺标记。所谓单位是指滑动条值， 而不是像素。

6. 这些代码只设置了标尺标记， 要想将它们显示出来， 还需要调用：slider.setPaintTicks(true);

7. 大标尺和小标尺标记是相互独立的。 例如， 可以每 20 个单位设置一个大标尺标记， 同时每 7 个单位设置一个小标尺标记， 但是这样设置， 滑动条看起来会显得非常凌乱。

8. 可以强制滑动条对齐标尺。这样一来， 只要用户完成拖放滑动条的操作， 滑动条就会立即自动地移到最接近的标尺处。 激活这种操作方式需要调用：

   slider.setSnapToTicks(true);

9. 可以调用下列方法为大标尺添加标尺标记标签（tick mark labels)：slider.setPaintLabels(true);

10. 还可以提供其他形式的标尺标记， 如字符串或者图标。首先需要填充一个键为 Integer 类型且值为 Component 类型的散列表。 然后再调用setLabelTable 方法，组件就会放置在标尺标记处。通常组件使用的是几此以对象。

    ```
    Hashtab1e<Integer, Component〉labelTable = new Hashtab1e<Integer, Components);
    labelTable.put(0, new ]Label("A"));
    labelTable.put(20, new 儿abel("B"));
    labelTable.putdOO, new 儿abel("F”)）；
    slider,setLabelTable(labelTable):
    ```

11. 如果标尺的标记或者标签不显示， 请检查一下是否调用了 setPaintTicks(true) 和setPaintLabels(true)。

12. 要想隐藏滑动条移动的轨迹， 可以调用：slider.setPaintTrack(false);

13. 滑动条是逆向的， 调用下列方法可以实现这个效果：slider.setlnverted(true);

    ```java
    class SliderFrame extends JFrame{
    	private JPanel slidePanel;
    	private JTextField textField;
    	private ChangeListener listener;
    
    	public SliderFrame(){
    		slidePanel=new JPanel();
    		slidePanel.setLayout(new GridLayout(10,1));
    
    		listener=event->{
    			JSlider source=(JSlider) event.getSource();
    			textField.setText(""+source.getValue());
    		};
    
    		JSlider slider=new JSlider();
    		addSlider(slider,"Plain");
    
    		//用给定的方向、最大值、 最小值和初始化值构造一个水平滑动条。
    		slider=new JSlider();
    		//显示标尺
    		slider.setPaintTicks(true);
    		//用给定的滑动条单位的倍数设置大标尺和小标尺。
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		addSlider(slider,"Ticks");
    
    		slider=new JSlider();
    		slider.setPaintTicks(true);
    		//每一次调整后滑块都要对齐到最接近的标尺处
    		slider.setSnapToTicks(true);
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		addSlider(slider,"Snap to ticks");
    
    		slider=new JSlider();
    		slider.setPaintTicks(true);
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		//如果 b 是 tme， 显示滑动条滑动的轨迹。
    		slider.setPaintTrack(false);
    		addSlider(slider,"No track");
    
    		slider=new JSlider();
    		slider.setPaintTicks(true);
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		slider.setInverted(true);
    		addSlider(slider,"Inverted");
    
    		slider=new JSlider();
    		slider.setPaintTicks(true);
    		//显示标尺标签。
    		slider.setPaintLabels(true);
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		addSlider(slider,"Labels");
    
    		slider=new JSlider();
    		slider.setPaintTicks(true);
    		slider.setPaintLabels(true);
    		slider.setMajorTickSpacing(20);
    		slider.setMinorTickSpacing(5);
    		
    		Dictionary<Integer,Component> labelTable=new Hashtable<>();
    		labelTable.put(0,new JLabel("A"));
    		labelTable.put(20,new JLabel("B"));
    		labelTable.put(40,new JLabel("C"));
    		labelTable.put(60,new JLabel("D"));
    		labelTable.put(80,new JLabel("E"));
    		labelTable.put(100,new JLabel("F"));
    		//设置用于作为标尺标签的组件。 
    		slider.setLabelTable(labelTable);
    		addSlider(slider,"Custom labels");
    
    		textField=new JTextField();
    		add(slidePanel,BorderLayout.CENTER);
    		add(textField,BorderLayout.SOUTH);
    		pack();
    	}
    	public void addSlider(JSlider s,String description){
    		s.addChangeListener(listener);
    		JPanel panel=new JPanel();
    		panel.add(s);
    		panel.add(new JLabel(description));
    		panel.setAlignmentX(Component.LEFT_ALIGNMENT);
    		GridBagConstraints gbc = new GridBagConstraints();
    		gbc.gridy=slidePanel.getComponentCount();
    		gbc.anchor=GridBagConstraints.WEST;
    		slidePanel.add(panel,gbc);
    	}
    }
    
    public class TestApplet3{
    	public static void main(String[] args) {
    		EventQueue.invokeLater(()->{
    			JFrame frame=new SliderFrame();
    			frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    			frame.setTitle("组合框");  //设置框架标题
    			frame.setLocation(300,300);
    			frame.setVisible(true);  //使其可见性
    		});
    	}
    }
    
    ```

##  5  菜 单

