---
title: DevExpress 控件
date: 2024-09-29 09:19:57
category:
  - [计算机与科学, C#]
tags: DevExpress
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/3-3.jpg
---

# [DevExpress 中文文档](https://www.dxper.net/documents)

# 安装

[📑 教程 1](https://www.cnblogs.com/purvis/p/15137637.html)

[📑 教程 2](https://blog.csdn.net/qq_41812739/article/details/116596129)

# vs2019 添加 DevExpress

方法一：运行 DevExpress 相关工具

- 进入 dev 安装目录，进入路径 `"D:\DevExpress 14.2\Components\Tools"`
- 在路径栏输入 "cmd"，进入命令窗口

- 执行命令 `ToolboxCreator.exe/ini:toolboxcreator.ini` 等待添加 DevExpress 控件

方法二：手动安装

- 打开工具箱，右键添加选项卡，自定义命名比如 "DevExpress"
- 右键刚创建的选项卡 "DevExpress"，点击 "选择项"
- 在 framework 筛选出 DevExpress

# 汉化

下载汉化包，将汉化包重命名为 `zh-CN`

## 项目汉化

把汉化包放到 `bin/debug` 下

```csharp
// main()中添加
System.Threading.Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("zh-CN");
```

## 统一汉化

1、把汉化包放至安装目录中：`D:\DevExpress 20.2\Components\Bin\Framework`

2、以管理员方式打开 `Developer Command Prompt for VS 2019`（在开始菜单查找）

3、输入 `cd D:\DevExpress 20.2\Components\Bin\Framework\zh-CN`

4、成功进入，再输入命令 `dir /B *.dll>temp.bat`，将会生成 `temp.bat` 文件

5、以文本方式打开汉化包中出现的 `temp.bat` 进行编辑：在所有的 DevExpress 前加上 `gacutil -i`

6、再次运行 `D:\DevExpress 20.2\Components\Bin\Framework\zh-CN>temp.bat`

# LabelControl 文本

## 属性

- AutoSizeMode：调节文本框（对应 label 中的 AutoSize）
  - defult：默认
  - None：自由调整
  - Vertical：垂直方向 - 不可调整模式
  - Horizontal：水平方向 - 不可调整模式
- Appearance：外观样式。可以设置字体、前景色、背景色和文本对齐方式等
- AppearanceDisabled、AppearanceHover、AppearancePressed：在不同状态下的标签外观样式，例如禁用状态、悬停状态和按下状态
- AutoEllipsis：指定是否自动省略长文本。如果设置为 true，则当标签文本超出控件宽度时，末尾部分将以省略号（…）显示。
- AutoSize：指定是否自动调整标签大小以适应文本内容。可以根据文本的长度自动调整标签的宽度和高度。
- Text：标签的文本内容
- TextAlignment：对齐方式
- Padding：内边距
- LineAlignment：多行文本的对齐方式。适用于 AutoSize 设置为 true 且文本跨多行显示时。
- UseMnemonic：是否启用助记符。如果设置为 true，则标签文本中使用的"&"符号将被解释为助记符。
- ImageOptions：显示图片/图标
- AllowHtmlString：链接效果，在 Text 写入 a 标签

# XtraForm

添加项，选择 `DevExpress v20.2 Template Gallery` 模板

派生于 Form，提供了 Form 的更换皮肤功能

- LookAndFeel：整个项目采用统一风格
  - SkinName：皮肤设置，UserDefaultLookAndFeel 和 UserWindowsXPTheme 设置为 false 才可生效
- TopMost：窗体置顶
- WindowState：窗体最大化、最小化、常规
- ShowInTaskbar：出现在任务栏，默认开启

# SimpleButton

- 外观渐变（单独样式）不生效：LookAndFeel ➡ Style：UItraFlat、UserDefaultLookAndFeel：false

# TextEdit 单行文本框

## 格式化

MaskSetting（属性 ➡ Properties ➡ MaskSetting ➡ 设置格式）

DisplayFormat

- FormatType：Custom（自定义）
- FormSrting：设置显示的格式，例如：`Price:{0:c2}`

属性 Text（string）不一定等于 EditValue（object）

## 文本代替

- PasswordChar：属性 ➡ Properties ➡ PasswordChar

## 提示

- NullText：设置提示文字
- AllowNullInput：是否允许为空
- ShowNullValuePrompt：

## 设置文本大小写

- CharacterCasing
  - Lower：小写
  - Upper：大写

## 事件

- EditValueChanged：值发生改变后
- EditValueChanging：值将要发生改变

# ButtonEdit 文本框（内置按钮）

- AutoHeight：自动高度

## 按钮

- ImageOptions：选择 icon
- King：Glyph（自定义图标）才会显示 icon

## 事件

- ButtonClick：内置按钮事件
- EditValueChange：判断是否输入、有值

+++success 演示：不同按钮执行不同语句

```csharp
private void buttonEdit1_ButtonClick(object sender, DevExpress.XtraEditors.Controls.ButtonPressedEventArgs e)
{
  EditorButton btn = e.Button;
  if (btn.Kind == ButtonPredefines.Ellipsis)
  {
    OpenFileDialog file = new OpenFileDialog(); // 打开文件夹
    if (file.ShowDialog() == DialogResult.OK)
    {
      buttonEdit1.EditValue = file.FileName; // 显示文件名
    }
  }

  else if (btn.Kind == ButtonPredefines.Glyph && btn.Caption == "Search")
  {
    MessageBox.Show("你点击了搜索按钮");
  }

  else if (btn.Kind == ButtonPredefines.Glyph && btn.Caption == "Add")
  {
    MessageBox.Show("你点击了添加按钮");
  }
}
```

+++

# CheckEdit 单选、复选框

- Properties ➡ AllowGrayed：是否允许有第三种状态（不确定状态） ➡ CheckState 出现 Indeterminate

- AutoHeight：是否自动高度

- Properties ➡ CheckBoxOptions ➡ Style：选择样式

- Properties ➡ RadioGroupIndex：设置相同的值，可变成单选

- Properties ➡ GlyphAlignment：选择框位置（居左、居中、居右）

- 设置选中的三种状态值 `checkEdit1.EditValue 来获取`

  - ValueChecked：设置类型才可填入值

  - ValueGrayed

  - ValueUnchecked

初始化时，EditValue 为 false/true，改变勾选状态后，EditValue 的值就是我们设置的值了

# GridControl 表格

1、去除 GridView 头上的 `Drag a column header here to group by that column`

点击 Run Designer ➡ 找到 Views 中的 OptionsView ➡ 将 ShowGroupPanel 设置为 false

# GridView

## 选中取消

```csharp
// 方法1：取消选中gridView中的所有行
gridView.ClearSelection();

// 方法2：遍历当前选中的行并逐一取消选择
int[] selectedRows = gridView.GetSelectedRows(); // 获取当前选中行的索引
foreach (int rowHandle in selectedRows) // 遍历选中的行并取消选中
{
    gridView.UnselectRow(rowHandle);
}
```

## 获取指定行的值

```csharp
// 获取指定行的值
gridView.GetRowCellValue(idx, "USER_NAME").ToString().Trim(); // 获取第idx行USER_NAME列的值
// 设置指定行的值
gridView.SetRowCellValue(idx, "USER_NAME", "要赋的值");
```

# TreeList 树形结构

## 单击节点以选中或取消选中

确保你的节点支持 CheckState 属性。如果没有设置 CheckBox，可能需要先启用

```csharp
private void treeList2_NodeClick(object sender, DevExpress.XtraTreeList.NodeClickEventArgs e)
{
    // 写法1：检查当前节点的 CheckState
    if (e.Node.CheckState == CheckState.Checked)
    {
        e.Node.CheckState = CheckState.Unchecked; // 取消选中
    }
    else
    {
        e.Node.CheckState = CheckState.Checked; // 选中
    }

    // 写法2
    e.Node.CheckState = e.Node.CheckState == CheckState.Checked ? CheckState.Unchecked : CheckState.Checked;
}
```

# BarDockControl

# DateEdit

# SpinEdit

显示小数点位数

- DisPlayFormat
- EditFormat

# MemoEdit

# XtraScrollableControl

# BarStaticItem

# Bar

# ToolbarFormControl
