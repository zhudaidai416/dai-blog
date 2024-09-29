# 介绍

windows 桌面端应用开发框架

https://github.com/dotnet/winforms

**项目结构：**

```json
Program.cs // 入口程序
xxx.Designer.cs // 布局
xxx.cs // 页面逻辑
```

**基本布局：**

```json
Dock // 停靠
Anchor // 锚点
```



# [安装](https://www.cnblogs.com/purvis/p/15137637.html)

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

# 窗口

```c#
Form2 form2 = new Form2();
this.Hide(); // 关闭当前的窗口
form2.ShowDialog(); // 打开新窗口
this.Dispose();
```

# 连接mysql数据库

出现 `MySql.Data.MySqlClient.MySqlException:“Host 'DESKTOP-1B2EKUI' is not allowed to connect to this MySQL` 错误

解决：https://gitcode.csdn.net/65e83e951a836825ed78b4cc.html?dp_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpZCI6MTk5NDE2MywiZXhwIjoxNzI4MDIxMjA3LCJpYXQiOjE3Mjc0MTY0MDcsInVzZXJuYW1lIjoibTBfNDg3MDE2NTQifQ.vx4FfLoO6Vq184GfjSOQsyuwu-YHWmd5FREFZFpiiHw&spm=1001.2101.3001.6650.10&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-10-122933316-blog-98814628.235%5Ev43%5Epc_blog_bottom_relevance_base6&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Eactivity-10-122933316-blog-98814628.235%5Ev43%5Epc_blog_bottom_relevance_base6

https://blog.csdn.net/weixin_46076729/article/details/117897946

# 表格

```c#
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

https://www.cnblogs.com/my---world/p/12044302.html
