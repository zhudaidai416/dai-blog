# 开源项目

📑 [项目1](https://github.com/bsf-gnls/TimeManage.git)

📑 [项目2](https://github.com/leighDEV/simple-todolist-winforms.git)

# 组件库

## DevExpress Winforms

## vs2019 添加 DevExpress

方法一：运行 DevExpress 相关工具

- 进入 dev 安装目录，进入路径 `"D:\tools\DevExpress 14.2\Components\Tools"`
- 在路径栏输入 "cmd"，进入命令窗口

- 执行命令 `ToolboxCreator.exe/ini:toolboxcreator.ini` 等待添加 DevExpress 控件

方法二：手动安装

- 打开工具箱，右键添加选项卡，自定义命名比如 "DevExpress"
- 右键刚创建的选项卡 "DevExpress"，点击 "选择项"
- 在 framework 筛选出 DevExpress

# 打开新窗口

```c#
Form2 form2 = new Form2();
this.Hide();
form2.ShowDialog();
this.Dispose();
```

