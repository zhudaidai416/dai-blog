---
title: WinForm
date: 2024-09-26 10:24:41
category:
  - [计算机与科学, C#]
tags: DevExpress
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/2-3.jpg
---

# 介绍

windows 桌面端应用开发框架

<https://github.com/dotnet/winforms>

# 开源项目

📑 [项目 1](https://github.com/bsf-gnls/TimeManage.git)

📑 [项目 2](https://github.com/leighDEV/simple-todolist-winforms.git)

[笔记](https://www.cnblogs.com/animal/p/3505797.html#:~:text=%E4%B8%80%E3%80%81%E5%9F%BA%E7%A1%80%EF%BC%9A.%20WIN)

# 项目结构

```bash
引用 # 包括所有的系统库文件的引用依赖
App.config # 当前项目的配置文件
Form1.cs # 当前窗体的事件逻辑源码
 - Form1.Designer.cs # 当前窗体的控件布局源码
 - Form1.resx # 当前窗体的资源文件（图片、图标、资源等）
 - 注意：
  a.Form1.cs和Form1.Designer.cs都定义了Form1类，该类使用了Partial关键词声明，其定义的类可以在多个地方被定义，最后编译的时候会被当作一个类来处理。因此两个文件各司其职，最后合并为一个类编译。
  b.要手动实现自定义窗体，可以添加自己的类，然后继承Form类即可
Program.cs # 当前项目程序的主入口 Main，启动项目，运行初始窗口
```

`Program.cs 文件`

```csharp
namespace WinFormTest
{
  // Program.cs 入口程序解读
  static class Program
  {
    /// <summary>
    /// 应用程序的主入口点。
    /// </summary>
    [STAThread] // Attributes语法，修饰Main方法。示应用程序的默认线程模型是单线程单元(STA)
    static void Main()
    {
      Application.EnableVisualStyles();
      Application.SetCompatibleTextRenderingDefault(false);
      Application.Run(new Form1()); // 开启窗口的消息循环，初始化并启动Form1窗口
    }
  }
}
```

# 手动添加控件

1、方法一：在 `Form1.Designer.cs` 中添加

```csharp
private System.Windows.Forms.Button btn_design; // 声明控件
// 默认的控件初始化方法
InitializeComponent():{
  this.btn_design = new System.Windows.Forms.Button(); // 定义控件
  this.btn_design.Text = "自定义控件"; // 设置Text属性
  this.btn_design.Location = new Point(40,40); // 设置布局位置Point(x,y)
  this.btn_design.Size = new Size(100,40); // 设置尺寸大小Size(width,height)
  this.Controls.Add(this.btn_design); // 注册控件到窗体
}
```

2、方法二：在 `Form1.cs` 中添加

```csharp
private Button btn_design; // 声明控件
public Form1() {
  // 先调用Designer.cs中的控件初始化方法
  InitializeComponent();
  this.btn_design = new System.Windows.Forms.Button(); // 定义控件
  this.btn_design.Text = "自定义控件"; // 设置Text属性
  this.btn_design.Location = new Point(40,40); // 设置布局位置Point(x,y)
  this.btn_design.Size = new Size(100,40); // 设置尺寸大小Size(width,height)
  this.Controls.Add(this.btn_design); // 注册控件到窗体
}
```

# 关闭窗口

```csharp
this.hide(); // 隐藏当前窗口，但会继续占用资源
this.close(); // 直接关闭当前窗口，以后可以再调用
this.dispose(); // 关闭当前窗口，以后不可以调用
```

# 窗口事件

## 自动添加

设计界面 ➡ 右键属性 ➡ 闪电符号（事件）➡ 添加事件

GUI 界面下 `Console.WriteLine` 不显示，需要使用调试模式

+++success 案例

```csharp
namespace WinFormTest
{
  public partial class Form1 : Form
  {
    public Form1()
    {
      InitializeComponent();
    }
    private void showMessage(object sender, EventArgs e) // Button的Click点击事件（自动添加）
    {
      MessageBox.Show("Hello World!"); // 显示弹出消息提示框
    }
  }
}
```

+++

```c#
Form2 form2 = new Form2();
this.Hide(); // 关闭当前的窗口
form2.ShowDialog(); // 打开新窗口
this.Dispose();。
```

## 手动添加

- 添加按钮控件到布局
- 书写事件处理函数，必须符合 `void function_name(object param1,EventArgs e){}` 的形式
- 添加注册事件：`this.Btn_design.Click += new EventHandler(this.showTip);`

+++success 案例

```csharp
namespace WinFormTest
{
  public partial class Form1 : Form
  {
    public Form1()
    {
      InitializeComponent();
      this.Btn_design.Click += new EventHandler(this.showTip); // 注册Click事件为手动添加的函数
    }
    public void showTip(Object sender,EventArgs e) // Button的Click点击事件（手动添加）
    {
      MessageBox.Show("手动添加!");
    }
  }
}
```

+++

# 布局

## 自动布局

当窗口大小拉伸改变时，布局控件不能实现自动适应，仍会保持原大小，因此自动布局只适用于窗口大小不变的情况

## 手动布局

```csharp
namespace WindowsFormsApp_learning
{
  public partial class Form1 : Form
  {
    public Form1()
    {
      InitializeComponent();
    }
    protected override void OnLayout(LayoutEventArgs levent) // 重写父类的OnLayout方法，实现手动布局自适应
    {
      base.OnLayout(levent); // 调用父类的OnLayout()，不是必须的
      int w = this.ClientSize.Width; // 获取当前客户窗口大小 ClientSize
      int h = this.ClientSize.Height;
      int yoff = 0; // 计算并设置每一个控件的大小和位置
      yoff = 4;
      this.text_box.Location = new Point(0, yoff); // 坐标(0,4)
      this.text_box.Size = new Size(w - 80, 30); // 尺寸(w-80,30)
      this.btn_click.Location = new Point(w - 80, yoff); // 坐标(w-80,4)
      this.btn_click.Size = new Size(80, 30); // 尺寸(80,30)

      yoff += 30; // 第一行的高度
      yoff += 4; // 间隔
      this.panel1.Location = new Point(0, yoff);
      this.panel1.Size = new Size(w, h - yoff - 4);
    }
  }
}
```

## 布局属性

- Anchor：固定、锚定
- Dock：停靠属性

# 常用控件

- TextBox：输入文本框
- CheckBox：复选框
- ComboBox：下拉列表（只能单选）
- ListBox：列表框（展示数据，可单选/多选）

# 连接 mysql 数据库

出现 `MySql.Data.MySqlClient.MySqlException:“Host 'DESKTOP-1B2EKUI' is not allowed to connect to this MySQL` 错误

[❓️ 解决](https://gitcode.csdn.net/65e83e951a836825ed78b4cc.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MTk5NDE2MywiZXhwIjoxNzI4MDIxMjA3LCJpYXQiOjE3Mjc0MTY0MDcsInVzZXJuYW1lIjoibTBfNDg3MDE2NTQifQ.vx4FfLoO6Vq184GfjSOQsyuwu-YHWmd5FREFZFpiiHw&spm=1001.2101.3001.6650.10&utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~activity-10-122933316-blog-98814628.235%5Ev43%5Epc_blog_bottom_relevance_base6&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~activity-10-122933316-blog-98814628.235%5Ev43%5Epc_blog_bottom_relevance_base6)

# 窗体生命周期

- Load：窗体加载时触发
- Shown：窗体显示时触发
- Activated：窗体获取焦点时触发
- Deactive：窗体失去焦点时触发
- FormClosing：窗体关闭过程中触发
- FormClosed：窗体关闭完成触发

# 表格

```csharp
DataTable dt = new DataTable("学生表");
dt.Columns.Add("序号", typeof(int));
dt.Columns.Add("姓名", typeof(string));
dt.Columns.Add("性别", typeof(string));
dt.Columns.Add("年龄", typeof(string));
dt.Columns.Add("班级", typeof(string));
dt.Columns.Add("班主任", typeof(string));

dt.Rows.Add(new object[] { 1, "朱呆呆", "女", "24", "一年级", "李四" });
dt.Rows.Add(new object[] { 1, "朱呆呆", "女", "24", "一年级", "李四" });
dt.Rows.Add(new object[] { 1, "呆呆", "女", "24", "一年级", "李四" });
dt.Rows.Add(new object[] { 1, "朱朱", "男", "32", "二年级", "李四" });
dt.Rows.Add(new object[] { 1, "朱呆呆", "女", "24", "三年级", "李四" });
```

<https://www.cnblogs.com/my---world/p/12044302.html>
