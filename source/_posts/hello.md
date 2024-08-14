---
title: Hello
sticky: true
cover: https://b0.bdstatic.com/f91548eaea79cf41c10e0fe2b9c49f6e.jpg@h_1280
tags:
  - hexo
  - 教程
---

# 介绍

欢迎来到呆鸭的笔记空间 ~

！！啥也没有 ~ 略略略！！{.bulr}

# 博客搭建

## hexo 搭建

安装使用 hexo 之前需要先安装 Node.js 和 Git

```sh
# 安装 hexo
npm install -g hexo-cli

# 建站
hexo init <folder>
cd <folder>
npm install
```

## shoka 主题

在 `/myblog` 目录拷贝 shoka 主题的文件到 `./themes/shoka` 目录下

```sh
git clone https://github.com/amehime/hexo-theme-shoka.git ./themes/shoka
```

然后删掉 `/themes/shoka` 里面的 `.git` 文件夹

修改根目录的 `_config.yml` 文件中的 `theme` 为 `shoka`

### 安装依赖

```sh
# 卸载掉默认的 hexo-renderer-marked
npm un hexo-renderer-marked --save

npm install hexo-renderer-multi-markdown-it --save
npm install hexo-autoprefixer --save
npm install hexo-algoliasearch --save
npm install hexo-symbols-count-time
npm install hexo-feed --save
```

### [配置](https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/dependents/)

### 参考

{% links %}
- site: 優萌初華
  owner: 霜月琉璃
  url: https://shoka.lostyu.me/computer-science/note/theme-shoka-doc
  desc: Hexo 主题 Shoka & multi-markdown-it 渲染器使用说明
  image: https://cdn.jsdelivr.net/gh/amehime/shoka@latest/images/avatar.jpg
  color: "#e9546b"
- site: 囧 o(╯□╰)o 囧
  owner: 囧 o(╯□╰)o 囧
  url: https://blog.twinkling.top/2023/11/16/note/shoka%20主题搭建过程/
  desc: shoka 主题搭建过程
  image: https://blog.twinkling.top/images/avatar.jpg
  color: "#c6d9a3"
- site: Yiqiu
  owner: Yiqiu
  url: https://ui123456ax.github.io/2023/06/23/02_Shoka图床修复
  desc: 关于 Shoka 图床修复
  image: https://ui123456ax.github.io/images/avatar.jpg
  color: "#e9546b"
- site: FuFan
  owner: FuFan
  url: https://fufan1025.github.io/2024/03/07/Shoka使用自己的图床
  desc: Shoka 使用自己的图床
  image: https://cdn.jsdelivr.net/gh/fufan1025/fufan1025.github.io@master/assets/avatar.webp
  color: "#499dd9"
- site: ShokaX
  url: https://docs.kaitaku.xyz
  desc: Hexo-theme-ShokaX
  color: "#9d5b8b"
  {% endlinks %}

# 踩坑

## 文章评论

:::primary no-icon

**问题**：设置评论身份

**解决**：获取邮箱的md5加密值：[LeanCloud](https://console.leancloud.cn/) ➡ 登录 ➡ 数据存储 ➡ 结构化数据 ➡ Comment ➡ 数据（mailMd5 的值）

:::

```yml
valine:
  tagMeta:
    visitor: 新朋友
    master: 主人
    friend: 小伙伴
    investor: 金主粑粑
  tagMember:
    master:
      # - hash of master@email.com
      # - 填写邮箱的md5加密值
    friend:
      # - hash of friend@email.com
    investor:
      # - hash of investor@email.com
```

:::primary no-icon

**问题**：valine 获取不到评论

**解决**：设置 ➡ 安全中心 ➡ Web 安全域名 ➡ 添加域名即可访问

:::

## 标签页

**问题**：标签页不显示下划线

**解决**：找到 `shoka/source/css/_common/components/tags/tabs.styl`，删掉第 6 行的 `overflow: hidden;`

# 插件

## [追番](https://github.com/HCLonely/hexo-bilibili-bangumi)

[:broken_heart:重要！重要！确保哔哩哔哩的追番列表是公开的！]{.label .danger}

1、安装

```sh
npm install hexo-bilibili-bangumi --save
```

2、新建页面

```sh
hexo new page bangumis
```

3、修改配置（在站点的配置文件 _config.yml 里添加）

```yml
bangumi:
  enable: true
  source: bili
  bgmInfoSource: "bgmv0"
  path: bangumis/index.html # 番剧页面路径，bangumis/index.html（默认）
  vmid: # 哔哩哔哩番剧页面的 vmid(uid)
  title: "我的追番"
  quote: "生命不息，追番不止！"
  show: 0 # 初始显示页面：0: 想看 , 1: 在看 , 2: 看过，默认为 1
  lazyload: true
  metaColor: "#94A270" # meta 部分 (简介上方) 字体颜色
```

4、运行命令

```sh
# 在 hexo generate 或 hexo deploy 之前使用
# 命令更新番剧数据
hexo bangumi -u

# 删除数据命令
hexo bangumi -d
```

## [live2d](https://github.com/EYHN/hexo-helper-live2d)

1、安装

```sh
npm install hexo-helper-live2d --save
npm install live2d-widget-model-tororo
```

2、配置（在站点的配置文件 _config.yml 里添加）

```yml
live2d:
  enable: true
  scriptFrom: local # 默认
  pluginModelPath: node_modules/live2d-widget-model-tororo # 模型文件相对与插件根目录路径
  tagMode: false # 标签模式, 是否仅替换 live2d tag标签而非插入到所有页面中
  debug: false # 调试, 是否在控制台输出日志
  model:
    use: live2d-widget-model-tororo # 模型名字
    scale: 1
    hHeadPos: 0.5
    vHeadPos: 0.618
  display:
    superSample: 2
    width: 150 # 显示位置及大小
    height: 300
    position: left
    hOffset: 40 # 控制看板娘平行位置
    vOffset: -60 # 控制看板娘垂直位置
  mobile:
    show: true # 手机显示开关，建议关闭
    scale: 0.5
  react:
    opacityDefault: 0.7
    opacityOnHover: 0.2
```

