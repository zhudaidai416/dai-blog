---
title: DevExpress 控件
date: 2024-09-29 09:19:57
category:
  - [计算机与科学, C#]
tags: DevExpress
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/2-4.jpg
---

# 安装

[📑 教程1](https://www.cnblogs.com/purvis/p/15137637.html)

[📑 教程2](https://blog.csdn.net/qq_41812739/article/details/116596129)

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

2、以管理员方式打开 `Developer Command Prompt for VS 2019`（开始菜单查找）

3、输入 `cd D:\DevExpress 20.2\Components\Bin\Framework\zh-CN`

4、成功进入，输入 `dir /B *.dll>temp.bat`

5、以文本方式打开汉化包中出现的 `temp.bat`进行编辑：在所有的 DevExpress 前加上 `gacutil -i`

6、在再次运行 `D:\DevExpress 20.2\Components\Bin\Framework\zh-CN>temp.bat`

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

- LookAndFeel
  - SkinName：皮肤设置，UserDefaultLookAndFeel 和 UserWindowsXPTheme 设置为 false 才可生效

# GridControl 表格

1、去除 GridView 头上的 `Drag a column header here to group by that column`

点击 Run Designer ➡ 找到 Views 中的 OptionsView ➡ 将 ShowGroupPanel 设置为 false

# BarStaticItem

# Bar

# ToolbarFormControl
