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

## 创建布局器

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



## 手动布局

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

## 线性布局

线性布局，包含 `水平布局` 和 `竖直布局` 两种。

以水平布局为例，

<img src="Java-Swing-notes.assets/image-20210822220712490.png" alt="image-20210822220712490" style="zoom:50%;" />

容器内添加了4个子控件，每个控件的宽度占多少？



演示：

水平布局器`AbstractXLayout`，水平方向上一次排列

- 对子空间进行水平方向上的布局

- 每个子空间可以单独指定宽度

  “100px”固定栈100像素

  “25%”固定栈总宽度的25%

  “auto”固定使用他的 Preferred Size

  “1w”按权重动态分配，参考《示例代码》文档

  

竖直布局

将各个控件从上到下一次布局，每个控件的高度可以单独指定。可以使用`AbstractYLayout`



<img src="Java-Swing-notes.assets/image-20210824082727257.png" alt="image-20210824082727257" style="zoom: 33%;" />

