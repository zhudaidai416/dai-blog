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

- 进入 dev 安装目录，进入路径 `"D:\tools\DevExpress 14.2\Components\Tools"`
- 在路径栏输入 "cmd"，进入命令窗口

- 执行命令 `ToolboxCreator.exe/ini:toolboxcreator.ini` 等待添加 DevExpress 控件

方法二：手动安装

- 打开工具箱，右键添加选项卡，自定义命名比如 "DevExpress"
- 右键刚创建的选项卡 "DevExpress"，点击 "选择项"
- 在 framework 筛选出 DevExpress

# GridControl 表格

1、去除 GridView 头上的 `Drag a column header here to group by that column`

点击 Run Designer ➡ 找到 Views 中的 OptionsView ➡ 将 ShowGroupPanel 设置为 false
