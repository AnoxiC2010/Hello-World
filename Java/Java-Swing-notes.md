# Swing notes

Swing是基于AWT的，所以在Swing项目中会看到`java.awt.*`下的内容。

Swing是一套轻量化的实现。

两套控件，Swing使用`J`开头的类。

AWT : `Lable`,  `Button`...

Swing : `JLable`, `JButton`...

## 第一个GUI程序

```java
//官方示例
package start;
 
/*
 * HelloWorldSwing.java requires no other files. 
 */
import javax.swing.*;        
 
public class HelloWorldSwing {
    /**
     * Create the GUI and show it.  For thread safety,
     * this method should be invoked from the
     * event-dispatching thread.
     */
    private static void createAndShowGUI() {
        //Create and set up the window.
        JFrame frame = new JFrame("HelloWorldSwing");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
 
        //Add the ubiquitous "Hello World" label.
        JLabel label = new JLabel("Hello World");
        frame.getContentPane().add(label);
 
        //Display the window.
        frame.pack();
        frame.setVisible(true);
    }
 
    public static void main(String[] args) {
        //Schedule a job for the event-dispatching thread:
        //creating and showing this application's GUI.
        javax.swing.SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                createAndShowGUI();
            }
        });
    }
}
```



## JFrame

代表一个窗口，通过JFrame相关的AIP就可以创建窗口

```java
private static void createGUI() {
    //JFrame代表一个窗口，构造参数是窗口标题
    JFrame frame = new JFrame("Swing Demo");
    frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    ...
    frame.setSize(400, 300);
    frame.setVisible(true);
}
```

自定义JFrame

```java
public class MyFrame extends JFrame {
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());
        //添加控件
		...
    }
}

public class MySwing {
    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                createGUI();
            }
        });
    }
    private static void createGUI() {
        JFrame frame = new MyFrame("Swing Demo");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 300);
        frame.setVisible(true);
    }
}
```



## JButton

代表按钮

- 点击事件

  ```java
  public class MyFrame extends JFrame {
      JLabel timeLabel = new JLabel("00:00:00");
      JButton button = new JButton("显示事件");
      
      public MyFrame(String title) {
          super(title);
          //内容面板
          Container contentPane = getContentPane();
          contentPane.setLayout(new FlowLayout());
          //添加控件
          contentPane.add(timeLabel);
          contentPane.add(button);
          //点击事件处理
          button.addActionListener(new ActionListener() {
              @Override
              public void actionPerformed(ActionEvent e) {
                  showTime();//点击事件
              }
          });
      }
      private void showTime() {
          String timeStr = new SimpleDataFormat("HH:mm:ss").format(new Date());
          timeLabel.setText(timeStr);
      }
  }
  ```

- 监听器

  `ActionListener`接口，`addActionListener(ActionListener l)`方法

  监听器Listener，是Swing里界面事件处理的一种方式。

  1. 创建监听器对象listener。
  2. 将监听器对象交给按钮。
  3. 当俺就被点击时，Swing框架会调用监听器对象里的方法，进行处理。

- 回调

  Callback，一个设计上的术语。

  当我们调用系统的一个方法时，叫Call(调用)：

  `frame.setVisible(true);`

  `label.setText("HelloWorld");`

  当我们写一个方法被系统调用时，叫Callback(回调)：

  `public void actionPerformed(Event e){...}`

  各种编程语言里，都可以实现“回调”的设计，在Java里，使用interface语法。

  ```
  我    --Call-->  别人
  我 <--Callback-- 别人
  ```

  

## JLabel

用于显示短文本，或图标

`setText()`设置文本

`setFont()`设置字体

`setForeground()`设置文本颜色

`setToolTipText()`设置工具提示

```java
public class MyFrame extends JFrame {

    public MyFrame(String text) {
        super(text);
		//内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());

        JLabel baiduLable = new JLabel();
        baiduLable.setText("百度");
        baiduLable.setFont(new Font("微软雅黑", 0, 14));
        baiduLable.setForeground(Color.BLUE);
        baiduLable.setToolTipText("http://baidu.com");
        //添加控件
        contentPane.add(baiduLable);
    }
}
```



## JTextField

用于显示单行文本

`new JtextField(16)`

其中，16表示列数，用于计算宽度(并不是字数限制)

`setText()/getText()`设置文本，获取文本

`setFont()`设置字体

```java
public class MyFrame extends JFrame {
    JLabel label = new JLabel("姓名");
    //构造参数：16表示16列，用于计算宽度显示，斌不是字符个数限制
    JTextField textField = new JTextField(16);
    JButton button = new JButton("确定");

    public MyFrame(String text) {
        super(text);

        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());

        contentPane.add(label);
        contentPane.add(textField);
        contentPane.add(button);

        button.addActionListener(e -> onButtonClick());
    }

    private void onButtonClick() {
        String text = textField.getText();
        //消息提示框
        JOptionPane.showMessageDialog(this, "输入了：" + text);
    }
}
```

<img src="Java-Swing-notes.assets/image-20210822093212771.png" alt="image-20210822093212771" style="zoom: 33%;" />

## JCheckBox

复选框

- API：

  `getSelected()/setSelected()`选中状态/设置

  `setText()`选项文字

- 事件：

  `addActionListener()`用户选中/取消时触发

```java
public class MyFrame extends JFrame {
    JCheckBox checkBox = new JCheckBox("我想订阅邮件通知");
    JTextField email = new JTextField(16);

    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());
        //添加控件
        contentPane.add(checkBox);
        contentPane.add(email);

        checkBox.setSelected(true);//默认选中
        email.setToolTipText("输入邮箱地址");
        checkBox.addActionListener(e -> {
            if (checkBox.isSelected())
                email.setEnabled(true);
            else
                email.setEnabled(false);//复选框未选中，禁用文本框
        });
    }
}
```

![image-20210822095632950](Java-Swing-notes.assets/image-20210822095632950.png)



## JComboBox

下拉列表

基本用法

- 创建JComboBox

  `JComboBox<String> colorList = new JComboBox<>();`

  可见，JComboBox是一个泛型，参数类型T表示的时数据项的类型

- 添加数据项

  ```java
  colorList.addItem("红色");
  colorList.addItem("蓝色");
  colorList.addItem("绿色");
  ```

- 按索引访问

  `getSelectedIndex()`获取选中项的索引

  `setSelectedIndex()`设置选中项

  `remove(index)`按索引删除

- 按数据项访问

  `getSelectedItem()`

  `setSelectedItem()`

- 事件处理

  还是ActionListener方式

```java
public class MyFrame extends JFrame {
    //JComboBox是一个泛型，参数类型是数据项的类型
    JComboBox<String> colorList = new JComboBox<>();
    JLabel sampleText = new JLabel("文本样例 This is a sample");
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());
        //添加控件
        contentPane.add(colorList);
        contentPane.add(sampleText);

        colorList.addItem("红色");
        colorList.addItem("蓝色");
        colorList.addItem("绿色");
        updateTextColor();
        //下拉列表事件处理
        colorList.addActionListener(e -> updateTextColor());

    }
    //跟新JLabel的颜色显示
    private void updateTextColor() {
        //获取选中项的值
        String item = (String) colorList.getSelectedItem();
        //根据选中的颜色，设置JLabel文字颜色
        Color color = null;
        switch (item) {
            case "红色":  color = Color.RED;  break;
            case "蓝色":  color = Color.BLUE;  break;
            case "绿色":  color = Color.GREEN;  break;
            default:      color = Color.BLACK;  break;
        }
        sampleText.setForeground(color);
    }
}
```

泛型

```java
public class MyFrame extends JFrame {
    //JComboBox是一个泛型，参数类型是数据项的类型
    JComboBox<ListOption> colorList = new JComboBox<>();
    JLabel sampleText = new JLabel("文本样例 This is a sample");
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());
        //添加控件
        contentPane.add(colorList);
        contentPane.add(sampleText);

        colorList.addItem(new ListOption("红色", Color.RED));
        colorList.addItem(new ListOption("蓝色", Color.BLUE));
        colorList.addItem(new ListOption("绿色", Color.GREEN));
        updateTextColor();
        //下拉列表事件处理
        colorList.addActionListener(e -> updateTextColor());

    }
    //跟新JLabel的颜色显示
    private void updateTextColor() {
        //获取选中项的值
        ListOption item = (ListOption) colorList.getSelectedItem();
        sampleText.setForeground(item.color);
    }

    //专门定义一个内部类，表示列表项
    private static class ListOption {
        public String text;
        public Color color;

        public ListOption(String text, Color color) {
            this.text = text;
            this.color = color;
        }
        //重写toString()用于列表项的显示
        @Override
        public String toString() {
            return this.text;
        }
    }
}
```

<img src="Java-Swing-notes.assets/image-20210822103809704.png" alt="image-20210822103809704" style="zoom:50%;" />



## JPanel

JPanel是一种常见的容器

默认是FlowLayout布局

JPanel可以使用`setLayout()`设置布局

JPanel掉i用`add()`添加子控件



## 示例：彩色标签

```java
public class MyFrame extends JFrame {
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        contentPane.setLayout(new FlowLayout());
        //添加控件
        JLabel l1 = new ColorfulLabel("1", Color.YELLOW);
        JLabel l2 = new ColorfulLabel("2", Color.GREEN);
        JLabel l3 = new ColorfulLabel("3", Color.LIGHT_GRAY);
        JLabel l4 = new ColorfulLabel("4", Color.CYAN);

        contentPane.add(l1);
        contentPane.add(l2);
        contentPane.add(l3);
        contentPane.add(l4);
    }
    private static class ColorfulLabel extends JLabel {
        public ColorfulLabel(String text, Color bgColor) {
            super(text);
            setOpaque(true);//不透明度，不设置看不到颜色
            setBackground(bgColor);//背景色
            setPreferredSize(new Dimension(60, 30));//单位像素
            setHorizontalAlignment(SwingConstants.CENTER);//对齐方式
        }
    }
}
```

![image-20210822153814996](Java-Swing-notes.assets/image-20210822153814996.png)





## 布局管理器

布局管理器：Layout Manager

指容器如何排列显示子控件。

例如，FlowLayout就是一个布局管理器。



### 流布局FlowLayout

- 默认地，自左至右逐个排列
- 当一行排满时，自动拍到下一行



`setPreferredSize()`控制每个控件的显示高度和宽度。

```
public class MyFrame extends JFrame {
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        //流布局
        LayoutManager layout = new FlowLayout(FlowLayout.LEFT);//无参是居中
        contentPane.setLayout(layout);
        //添加控件
    }
}
```

![image-20210822163841757](Java-Swing-notes.assets/image-20210822163841757.png)

### 边界布局BorderLayout

把容器分为上、下、左、右、中，五个区域。

`setPreferredSize()`

- 对于上下边界，可以调整高度
- 对于左右边界，可以调整宽度

<img src="Java-Swing-notes.assets/image-20210822165040460.png" alt="image-20210822165040460" style="zoom:33%;" />

### 卡片布局CardLayout

CardLayout：犹如多张叠在一起的卡片，每次只显示一张卡片的内容。

使用：

```java
pane.setLayout(new CardLayout);
pane.add(c1, "name1");
pane.add(c2, "name2");
```

在添加控件时，关联一个String作为名字，便于后面控制。

例如：`cardLayout.show(pane, "name1");`



演示：

```
ContentPane(BorderLayout)
  - 上方：JComboBox
  - 中间：JPanel(CardLayout)
    - JPanel 测试面板1(FlowLayout)
      - JButton...
    - JPanel 测试面板2(FlowLayout)
      - JTextField
```

```java
public class MyFrame extends JFrame {
    JComboBox<String> options = new JComboBox<>();
    JPanel cards = new JPanel();
    public MyFrame(String text) {
        super(text);
        //内容面板
        Container contentPane = getContentPane();
        //给顶层容器设置边界布局BorderLayout
        contentPane.setLayout(new BorderLayout());
        //添加控件
        //创建一个下拉列表，供选择
        options.addItem("第一个面板");
        options.addItem("第二个面板");

        contentPane.add(options, BorderLayout.PAGE_START);
        contentPane.add(cards, BorderLayout.CENTER);

        //创建第一个面板
        JPanel p1 = new JPanel();
        p1.add(new JButton("红"));
        p1.add(new JButton("绿"));
        p1.add(new JButton("蓝"));
        //创建第二个面板
        JPanel p2 = new JPanel();
        p2.add(new JLabel("输入"));
        p2.add(new JTextField(20));


        cards.setLayout(new CardLayout());
        cards.add(p1, "buttons"); //添加第一张卡片，名字叫buttons
        cards.add(p2, "text"); //添加第二章卡片，名字叫text

        //添加事件响应
        options.addItemListener(new ItemListener() {
            @Override
            public void itemStateChanged(ItemEvent e) {
                itemChanged();
            }
        });
    }

    private void itemChanged() {
        CardLayout cardLayout = (CardLayout) cards.getLayout();
        int index = options.getSelectedIndex();
        if (index == 0) {
            cardLayout.show(cards, "buttons");
        } else if (index == 1) {
            cardLayout.show(cards, "text");
        }
    }
}
```

![image-20210822172854132](Java-Swing-notes.assets/image-20210822172854132.png)

## 自定义布局器

- 窗口布局

  把上层窗口称为容器(Container)

  容器里可以又多个子窗口或子空间(Component)

  所谓布局，就是决定每一个子控件间显示在什么位置

  ![image-20210822174414631](Java-Swing-notes.assets/image-20210822174414631.png)

  > 左上角为(0,0)，横坐标享有增长，纵坐标向下增长

- 演示：

  取消布局器

  使用`setBounds()`对子空间手动布局

- 要点与细节

  取消布局器后，子空间默认是不显示的。

  Frame窗口大小，包括了标题栏。所以，下面的有效区域的高度不足300像素。

```java
public class MyFrame extends JFrame {
    public MyFrame(String text) {
        super(text);
        //不使用布局器(默认地，ContentPane自带一个布局器)
        Container root = getContentPane();
        root.setLayout(null);//取消其布局器
        //由于没有布局器，所以默认地，子控件无法显示
        JLabel l1 = new ColorfulLabel("1", Color.YELLOW);
        JLabel l2 = new ColorfulLabel("2", Color.BLUE);
        root.add(l1);
        root.add(l2);
		// public Rectangle(int x, int y, int width, int height) {
        l1.setBounds(new Rectangle(0, 0, 200, 200)); //会遮挡l2
        l2.setBounds(new Rectangle(100, 100, 200, 100));
    }

    private static class ColorfulLabel extends JLabel {
        public ColorfulLabel(String text, Color bgColor) {
            super(text);
            setOpaque(true);//不透明度，不设置看不到颜色
            setBackground(bgColor);//背景色
            setPreferredSize(new Dimension(60, 30));//单位像素
            setHorizontalAlignment(SwingConstants.CENTER);//对齐方式
        }
    }
}
```

### 创建布局器

布局管理器 Layout Manager：

负责对子空间的布局，当窗口变化的时候，动态调整子空间的位置和大小。

比如，FlowLayout、BorderLayout ...



演示：

创建一个布局器，和一个标签在右上角，另一个标签在窗口中央。

当容器大小变化时，子空间的位置随之动态改变。



1. 创建一个内部类SimpleLayout

   ```java
   private static class SimpleLayout implements LayoutManager {
       ...
       @Override
       public void layoutContainer(Container parent) {
   
       }
   }
   ```

2. 给窗口设置布局器

   ```java
   Container root = getContentPane();
   root.setLayout(new SimpleLayout());
   ```

3. 在`layoutContainer()`里进行布局

   根据父容器的大小，动态计算每个控件的位置

   l1：居中显示

   l2：右上角显示

   l1，l2的大小均使用Perferred Size，让他们自己计算最合适的大小

   注意：在ColorfullLabel里，不要设置PreferredSize，让它自己计算

```java
public class MyFrame extends JFrame {
    JLabel l1 = new ColorfulLabel("Hello, World", Color.YELLOW);
    JLabel l2 = new ColorfulLabel("样例文本", new Color(202, 255, 112));
    public MyFrame(String text) {
        super(text);
        //根容器
        Container root = getContentPane();
        //设置一个自定义的布局器
        LayoutManager layoutMgr = new SimpleLayout();
        root.setLayout(layoutMgr);

        root.add(l1);
        root.add(l2);
    }

    private class ColorfulLabel extends JLabel {
        public ColorfulLabel(String text, Color bgColor) {
            super(text);
            setOpaque(true);//不透明度，不设置看不到颜色
            setBackground(bgColor);//背景色
//            setPreferredSize(new Dimension(60, 30));//单位像素
            setHorizontalAlignment(SwingConstants.CENTER);//对齐方式
            setFont(new Font("宋体", Font.PLAIN, 16));
        }
    }

    private class SimpleLayout implements LayoutManager {
        @Override
        public void addLayoutComponent(String name, Component comp) { }
        @Override
        public void removeLayoutComponent(Component comp) { }
        @Override
        public Dimension preferredLayoutSize(Container parent) { return null; }
        @Override
        public Dimension minimumLayoutSize(Container parent) { return null; }

        @Override
        public void layoutContainer(Container parent) {
            int w = parent.getWidth(); //父窗口的宽度width
            int h = parent.getHeight(); //父窗口的高度height
            System.out.println("父容器：" + w + "," + h );
            //l1显示在中间，以Preferred Size作为大小
            if (l1.isVisible()) {
                //取得该控件所需的显示尺寸
                Dimension size = l1.getPreferredSize();
                int x = (w - size.width) / 2;
                int y = (h - size.height) / 2;
                //在设置子空间位置时，其坐标是相对于父窗口的
                //（0，0）就是父窗口的左上角
                l1.setBounds(x, y, size.width, size.height);
            }
            //l2小时在右上角
            if (l2.isVisible()) {
                Dimension size = l2.getPreferredSize();
                int x = w - size.width;
                int y = 0;
                l2.setBounds(x, y, size.width, size.height);
            }
        }
    }
}
```

![image-20210822215032401](Java-Swing-notes.assets/image-20210822215032401.png)

布局器的运行机制：

1. 给容器设置一个布局器

   `root.setLayout(layoutMgr);`

2. 当容器大小改变时，自动调用布局器重新布局

   `layoutMgr.layoutContainer(...);`

所以，`layoutContainer(...)`是Swing内部自动调用的



要点和细节：

1. Preferred Size，指控件的最佳大小

   当调用`label.getPreferredSize()`时，由它自行测算自己所需要显示的尺寸

2. Dimension表示尺寸信息

   `Dimension size = ...`

   `size.width`， `size.height`



### 手动布局

上面创建的布局器的布局方式，就是手动布局。

即，手工定义每个控件的位置。



手工布局时，只需要重写`layoutContainer(Container parent)`这个方法



优化：

由于只关心`layoutContainer(...)`这一个方法，可以做一点代码和优化

1. 抽象类AbstractSimpleLayout
2. 在需要授工部局是，只需要继承AbstractSimpleLayout即可

```java
public abstract class AbstractSimpleLayout implements LayoutManager {
    @Override 
    public void addLayoutComponent(String name, Component comp) { }
    @Override 
    public void removeLayoutComponent(Component comp) { }
    @Override 
    public Dimension preferredLayoutSize(Container parent) { return null; }
    @Override 
    public Dimension minimumLayoutSize(Container parent) { return null; }
    //     @Override 
    //     public void layoutContainer(Container parent) { }
}
class SimpleLayout extends AbstractSimpleLayout {
    @Override
    public void layoutContainer(Container parent) {
        int w = parent.getWidth(); //父窗口的宽度width
        int h = parent.getHeight(); //父窗口的高度height
        System.out.println("父容器：" + w + "," + h );
        //l1显示在中间，以Preferred Size作为大小
        if (l1.isVisible()) {
            //取得该控件所需的显示尺寸
            Dimension size = l1.getPreferredSize();
            int x = (w - size.width) / 2;
            int y = (h - size.height) / 2;
            //在设置子空间位置时，其坐标是相对于父窗口的
            //（0，0）就是父窗口的左上角
            l1.setBounds(x, y, size.width, size.height);
        }
        //l2小时在右上角
        if (l2.isVisible()) {
            Dimension size = l2.getPreferredSize();
            int x = w - size.width;
            int y = 0;
            l2.setBounds(x, y, size.width, size.height);
        }
    }
}
```

### 线性布局 

线性布局，包含 `水平布局` 和 `竖直布局` 两种。

以水平布局为例，

<img src="Java-Swing-notes.assets/image-20210822220712490.png" alt="image-20210822220712490" style="zoom:50%;" />

容器内添加了4个子控件，每个控件的宽度占多少？



演示：

水平布局器`RowLayout`，水平方向上一次排列

- 对子空间进行水平方向上的布局

- 每个子空间可以单独指定宽度

  “100px”固定栈100像素

  “25%”固定栈总宽度的25%

  “auto”固定使用他的 Preferred Size

  “1w”按权重动态分配，参考《示例代码》文档


```java
/* 横向布局器
 */
public class RowLayout implements LayoutManager2
{
	private List<Item> items = new ArrayList<>();
	private int gap = 2;
	private boolean usePerferredSize = false; // 竖立方向是否占满
	
	public AfRowLayout()
	{		
	}
	public AfRowLayout(int gap)
	{		
		this.gap = gap; // 控件之间的间距
	}
	public AfRowLayout(int gap, boolean usePerferredSize)
	{	
		this.gap = gap;
		this.usePerferredSize = usePerferredSize;
	}
		
	@Override
	public void addLayoutComponent(String name, Component comp)
	{
		Item item = new Item();
		item.comp = comp;
		item.constraints = "auto";
		items.add(item);
	}

	@Override
	public void removeLayoutComponent(Component comp)
	{
		Iterator<Item> iter = items.iterator();
		while(iter.hasNext())
		{
			Item item = iter.next();
			if(item.comp == comp)
			{
				iter.remove();
			}
		}
	}

	@Override
	public void addLayoutComponent(Component comp, Object constraints)
	{
		Item item = new Item();
		item.comp = comp;
		item.constraints = (String) constraints;
		items.add(item);
	}
	
	@Override
	public Dimension preferredLayoutSize(Container parent)
	{
		return new Dimension(30,30);
	}

	@Override
	public Dimension minimumLayoutSize(Container parent)
	{
		return new Dimension(30,30);
	}

	@Override
	public Dimension maximumLayoutSize(Container target)
	{
		return new Dimension(30,30);
	}

	
	@Override
	public void layoutContainer(Container parent)
	{
		// 得到内矩形
		Rectangle rect = new Rectangle(parent.getWidth(), parent.getHeight());
		//Rectangle rect = parent.getBounds();
		Insets insets = parent.getInsets();
		rect.x += insets.left;
		rect.y += insets.top;
		rect.width -= (insets.left + insets.right);
		rect.height -= (insets.top + insets.bottom);
		
		// 第一轮：过滤到无效的 Item ( 有些控件是隐藏的 )
		List<Item> validItems = new ArrayList<>();
		for(Item it: items )
		{
			if(it.comp.isVisible())
				validItems.add(it);
		}
		
		// 第二轮处理：百分比，像素，auto的，直接计算出结果; 权重的，在第三轮计算
		int totalGapSize = gap * (validItems.size() - 1);// 间距大小
		int validSize = rect.width - totalGapSize;
		int totalSize = 0;
		int totalWeight = 0;
		for(Item it : validItems)
		{
			Dimension preferred = it.comp.getPreferredSize();
			it.width = preferred.width;
			it.height = usePerferredSize ? preferred.height : rect.height;
			it.weight = 0;
			
			// 计算宽度
			String cstr = it.constraints;
			if( cstr == null || cstr.length() == 0)
			{
				//System.out.println("(AfRowLayout) Warn: Must define constraints when added to container!");
			}
			else if( cstr.equals("auto"))
			{
			}
			else if(cstr.endsWith("%")) // 按百分比
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-1));
				it.width = validSize * num / 100;
			}
			else if(cstr.endsWith("w")) // 按权重
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-1));
				it.width = 0;
				it.weight = num;
			}
			else if(cstr.endsWith("px")) // 按权重
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-2));
				it.width = num;
			}
			else // 按像素
			{
				int num = Integer.valueOf(cstr);
				it.width = num;
			}
			
			totalSize += it.width;
			totalWeight += it.weight;
			
			//System.out.println("计算值：width=" + it.width + ",weight=" + it.weight);
		}
		
		// 第三轮: 剩余空间按权重分配
		if( totalWeight > 0)
		{
			int remainSize = validSize - totalSize;
			double unit = (double) remainSize / totalWeight;
			for(Item it : validItems)
			{
				if(it.weight > 0)
				{
					it.width = (int)( unit * it.weight );
				}
			}			
		}

		//System.out.println("总宽度: " + rect.width);
		
		// 第四轮: 按宽度和高度布局
		int x = 0;
		for(Item it : validItems)
		{
			int y = (rect.height - it.height)/2;
			if(x + it.width > rect.width)
				it.width = rect.width - x;
			if(it.width <= 0) break;
			
			it.comp.setBounds(rect.x + x, rect.y + y, it.width, it.height);
			
			//System.out.println("宽度: " + it.width);
			x += it.width;
			x += gap; // 间距
		}
	}

	@Override
	public float getLayoutAlignmentX(Container target)
	{
		return 0;
	}

	@Override
	public float getLayoutAlignmentY(Container target)
	{
		return 0;
	}

	@Override
	public void invalidateLayout(Container target)
	{
		
	}

	///////////////////////
	private static class Item
	{
		Component comp;
		String constraints = "auto";
		int width = 0;
		int height = 0;
		int weight = 0;  // 权重
	}
	

}
```

竖直布局

将各个控件从上到下一次布局，每个控件的高度可以单独指定。可以使用`CloumnLayout`



<img src="Java-Swing-notes.assets/image-20210824082727257.png" alt="image-20210824082727257" style="zoom: 33%;" />

```java
/* 纵向布局器 
 */
public class ColumnLayout implements LayoutManager2
{
	private List<Item> items = new ArrayList<>();
	private int gap = 2;
	private boolean usePerferredSize = false; // 竖立方向是否占满
	
	public AfColumnLayout()
	{		
	}
	public AfColumnLayout(int gap)
	{		
		this.gap = gap; // 控件之间的间距
	}
	public AfColumnLayout(int gap, boolean usePerferredSize)
	{	
		this.gap = gap;
		this.usePerferredSize = usePerferredSize;
	}
		
	@Override
	public void addLayoutComponent(String name, Component comp)
	{
		Item item = new Item();
		item.comp = comp;
		item.constraints = "auto";
		items.add(item);
	}

	@Override
	public void removeLayoutComponent(Component comp)
	{
		Iterator<Item> iter = items.iterator();
		while(iter.hasNext())
		{
			Item item = iter.next();
			if(item.comp == comp)
			{
				iter.remove();
			}
		}
	}

	@Override
	public void addLayoutComponent(Component comp, Object constraints)
	{
		Item item = new Item();
		item.comp = comp;
		item.constraints = (String) constraints;
		items.add(item);
	}
	
	@Override
	public Dimension preferredLayoutSize(Container parent)
	{
		return new Dimension(30,30);
	}

	@Override
	public Dimension minimumLayoutSize(Container parent)
	{
		return new Dimension(30,30);
	}

	@Override
	public Dimension maximumLayoutSize(Container target)
	{
		return new Dimension(30,30);
	}

	
	@Override
	public void layoutContainer(Container parent)
	{
		// 得到内矩形
		Rectangle rect = new Rectangle(parent.getWidth(), parent.getHeight());
		//Rectangle rect = parent.getBounds();
		Insets insets = parent.getInsets();
		rect.x += insets.left;
		rect.y += insets.top;
		rect.width -= (insets.left + insets.right);
		rect.height -= (insets.top + insets.bottom);
		
		// 第一轮：过滤到无效的 Item ( 有些控件是隐藏的 )
		List<Item> validItems = new ArrayList<>();
		for(Item it: items )
		{
			if(it.comp.isVisible())
				validItems.add(it);
		}
		
		// 第二轮处理：百分比，像素，auto的，直接计算出结果; 权重的，在第三轮计算
		int totalGapSize = gap * (validItems.size() - 1);// 间距大小
		int validSize = rect.height - totalGapSize;
		int totalSize = 0;
		int totalWeight = 0;
		for(Item it : validItems)
		{
			Dimension preferred = it.comp.getPreferredSize();
			it.width = usePerferredSize ? preferred.width : rect.width;
			it.height = preferred.height;
			it.weight = 0;
			
			// 计算宽度
			String cstr = it.constraints;
			if( cstr == null || cstr.length() == 0)
			{
				//System.out.println("(AfColumnLayout) Warn: Must define constraints when added to container!");
			}
			else if( cstr.equals("auto"))
			{
			}
			else if(cstr.endsWith("%")) // 按百分比
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-1));
				it.height = validSize * num / 100;
			}
			else if(cstr.endsWith("w")) // 按权重
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-1));
				it.height = 0;
				it.weight = num;
			}
			else if(cstr.endsWith("px")) // 按权重
			{
				int num = Integer.valueOf(cstr.substring(0,cstr.length()-2));
				it.height = num;
			}
			else // 按像素
			{
				int num = Integer.valueOf(cstr);
				it.height = num;
			}
			
			totalSize += it.height;
			totalWeight += it.weight;
			
			//System.out.println("计算值：width=" + it.width + ",weight=" + it.weight);
		}
		
		// 第三轮: 剩余空间按权重分配
		if( totalWeight > 0)
		{
			int remainSize = validSize - totalSize;
			double unit = (double) remainSize / totalWeight;
			for(Item it : validItems)
			{
				if(it.weight > 0)
				{
					it.height = (int)( unit * it.weight );
				}
			}			
		}

		//System.out.println("总宽度: " + rect.width);
		
		// 第四轮: 按宽度和高度布局
		int y = 0;
		for(Item it : validItems)
		{
			int x = 0; // 水平靠左			
			if(y + it.height > rect.height)
				it.height = rect.height - y;
			if(it.height <= 0) break;
			
			it.comp.setBounds(rect.x + x, rect.y + y, it.width, it.height);
			
			//System.out.println("宽度: " + it.width);
			y += it.height;
			y += gap; // 间距
		}
	}

	@Override
	public float getLayoutAlignmentX(Container target)
	{
		return 0;
	}

	@Override
	public float getLayoutAlignmentY(Container target)
	{
		return 0;
	}

	@Override
	public void invalidateLayout(Container target)
	{
		
	}

	///////////////////////
	private static class Item
	{
		Component comp;
		String constraints = "auto";
		int width = 0;
		int height = 0;
		int weight = 0;  // 权重
	}
}
```



### 自由位置布局

自由指定每一个控件的位置，可以为任何大小、任意位置。



自定义自由位置布局器`AnyWhereLayout`

一般步骤：

- 创建布局器

  `panel.setLayout(new AnyWhereLayout)`;

- 添加子空间时，指定`MyMargin`参数

  `panel.add(label, new MyMargin(01, 15, 20, 15));`

位置参数`MyMargin`，描述一个控件的4个边距：

`new MyMargin(top, left, bottom, right)`按上、左、下、右的顺序指定，-1表示自动计算，例如`root.add(a4, new MyMargin(-1, 15, 20, 15));



## 边框

### 创建边框

`javax.swing.border.*`下是边框相关的接口和实现

`Border`：是一个接口

`BorderFactory`：是一个工具类，用于创建各型边框

例如：

```java
Border border = BorderFactory.createLineBorder(Color.BLUE);
root.setBorder(border);
```



### 边框种类

>  Swing提供了各式边框，详见官方文档。

几个大类型的边框：

- 简单边框Simple
- 特种边框Matte
- 代表提的边框Titled
- 复合边框Compound

### 边距与填充

在Swing里，边距和填充也是由`Border`来实现的

`Border padding = BorderFactory.createEmptyBorder(8, 8, 8, 8);`

`EmptyBorder`可以用来实现空白填充效果



布局时增加Padding/Margin，是常见操作，都可以用`Border`实现。

颜色选择可百度RGB颜色。

### AfBorder



### AfPanel



## 图标

### 使用图标

图标`Icon` | `ImageIcon`

默认地，`JLabel`，`JButton`都可以显示图标

支持jpeg/jpg/png格式的静态图片



准备图标文件：

- 下载图标文件(iconfont.cn)

  支持jpg/jpeg/png格式

- 新建一个package，放图标在里面



注意：

- 支持格式jpg/png，勉强支持gif动态图格式
- 不支持缩放，所以准备图片时先确定好尺寸



### 资源文件



### 本地文件



## 自定义控件

### 自定义控件

当需要自定义控件时，可以从JPanel派生

继承于 JPanel 便可以得到自定义的控件

重写 paintComponent() ，便可以定义控件的显示

例如：

```java
class MyPanel extends JPanel {
}
```

则MyPanel是一个自定义控件

重写`paintComponent`方法，可以决定它的显示

```java
protected void paintComponent(Graphics g){
}
```

例如可以显示为一个蓝色方块，也可以是任何图案

```java
public class MyPanel extends JPanel {
    @Override
    protected void paintComponent(Graphics g) {
        int width = getWidth();
        int height = getHeight();
        g.clearRect(0, 0, width, height);
        g.setColor(Color.BLUE);
        g.fillRect(0, 0, width, height);
    }
}
```



自定义控件的所有行为，都可以定制

- 显示成什么样子
- 响应鼠标或键盘事件
- 最小尺寸/最佳尺寸/最大尺寸
- ...



在 paintComponent() 可以绘制出任何形状或颜色

- 直线、三角形、长方形、椭圆等几何图形
- 直线、点划线、等各种线型

- 纯色、渐变色等各种填充色

- 绘制文本

- 绘制图片
- 还可以施加一些变形效果..

### RGB颜色

RGB , 是一种 颜色空间 Color Space
指定红、绿、蓝三种颜色的值，即可构造各种颜色
例如：
黑色  (0,   0,    0)
白色  (255,255,255)
红色  (255,  0,   0)



在 Swing 里，用Color 类来代表示颜色
例如，
Color color = Color.red;
Color color = new Color (255, 0, 0);
Color color = new Color(0xFF0000);



那么，如何选择一个自己喜欢的颜色？

1. 看到某个颜色，用拾色工具去取
2. 查看颜色表，选一个自己喜欢的颜色



RGBA 透明色

RGBA, 最后一个A表示的透明度的值，也是0-255之间
0 ：全透明
255 ：完全不透明 

![image-20210905154323417](Java-Swing-notes.assets/image-20210905154323417.png)

### 绘制几何图形

控件的绘制

在控件里可以绘制什么内容？
- 几何图形
- 文本 
- 图片

绘制时，使用Graphics 和 Graphics2D 下的方法
- Line 直线
- Rect 矩形 ( 包含正方形 )
- Oval 椭圆 ( 包含圆 )
- Polygon 多边形 ( 含三角形 )
- Arc 圆弧 / 扇形

绘制方法分为两种：

- drawXXX( ) : 表示只画线条

- fillXXX( ): 表示只填充

例如，

- drawRect() : 画一个矩形 (仅线条)
- fillRect() : 画一个矩形 ( 仅填充 )



getWidth() : 控件的宽度

getHeight(): 控件的高度

坐标：左上角(0, 0) 右下角(width, height)

示例：

- g.setColor(Color.blue);
- g.drawLine(0, 0, 200, 100);

```java
public class MyFrame extends JFrame {
    // SpinnerNumberModel: initialValue, min, max, step
    JSpinner grainField = new JSpinner(new SpinnerNumberModel(1, 1, 10, 1));
    JSpinner rangeField = new JSpinner(new SpinnerNumberModel(50, 20, 80, 5));
    JSpinner periodField = new JSpinner(new SpinnerNumberModel(100, 50, 150, 10));
    MyPanel root = new MyPanel();
    public MyFrame(String text) {
        super(text);
        //根容器
        setContentPane(root);
        root.add(new JLabel("粒度"));
        root.add(grainField);
        root.add(new JLabel("振幅"));
        root.add(rangeField);
        root.add(new JLabel("周期"));
        root.add(periodField);

        ChangeListener changeListener = new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                UpdateUi();
            }
        };
        grainField.addChangeListener(changeListener);
        rangeField.addChangeListener(changeListener);
        periodField.addChangeListener(changeListener);
    }

    private void UpdateUi() {
        root.grain = (int) grainField.getValue();
        root.range = (int) rangeField.getValue();
        root.period = (int) periodField.getValue();
        root.repaint();
    }
}
public class MyPanel extends JPanel {
    public int grain = 3;
    public int range = 50;
    public int period = 100;
    @Override
    protected void paintComponent(Graphics g) {
        int width = getWidth();
        int height = getHeight();
        // 清除显示
        g.clearRect(0, 0, width, height);
        g.setColor(Color.WHITE);
        g.fillRect(0, 0, width, height);
        //中线
        int center_y = height / 2;
        g.setColor(Color.RED);
        g.drawLine(0, center_y, width, center_y);
        // 正弦曲线 （由多个小段连接而成，近似为一条曲线 )
        int x1 = 0, y1 = 0;
        for (int i = 0; i < width; i += grain) {
            int x2 = i;
            double angle = (double)x2 / period * 2 * Math.PI;
            int y2 = (int) (range * Math.sin(angle));
            g.drawLine(x1, y1 + center_y, x2, y2 + center_y);
            x1 = x2;
            y1 = y2;
        }
    }
}
```



## 图片的绘制

### 绘制图片



### 锁定长宽比



## 鼠标事件

鼠标事件 MouseEvent ，包括以下动作：

鼠标点击 mouseClicked

鼠标按下 mousePressed 鼠标抬起 mouseReleased

鼠标移入 mouseEntered 鼠标移出 mouseExited

鼠标移动 mouseMoved 鼠标拖动 mouseDragged

鼠标滚轮 mouseWheelMoved



将所有动作分为三类监听器：

1 addMouseListener()

 点击、按下、抬起、移入、移出

2 addMouseMotionListener()

 移动、拖动

3 addMouseWheelListener()

 鼠标滚轮转动



鼠标事件对象 MouseEvent:

getX() / getY() : 点击中的坐标，相对于该控件

getXOnScreen() / getOnScreen() ：相对于屏幕的坐标

getSource() : 事件源，即点中的控件

getButton() : 左键、中键、右键

getClickCount() : 单击、双击、三击



常见问题

1 区分 mouseClicked 

mousePressed / mouseReleased

2 区分 mouseMoved 与 mouseDragged 

mouseMoved: 鼠标移动

mouseDragged: 鼠标按住时移动



思考：如果我只关心 mouseClicked() 事件，还需要重写5个方法，是不是有点啰嗦？

↓

### 鼠标适配器

鼠标适配器 MouseAdapter
它已经把所有的鼠标回调方法都重写了
我们只需要选择自己关心的方法重写一下



## 菜单与工具栏

### 菜单栏



### 工具栏



### 右键菜单



