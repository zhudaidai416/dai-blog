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

# 博客搭建过程

## 博客搭建

安装使用 hexo 之前需要先安装 Node.js 和 Git

```sh
# 安装 hexo
npm install -g hexo-cli

# 建站
hexo init <folder>
cd <folder>
npm install
```

## 使用 shoka 主题

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
npm install hexo-feed --save-dev
```

### [配置](https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/dependents/)

### 参考

[Hexo 主题 Shoka & multi-markdown-it 渲染器使用说明](https://shoka.lostyu.me/computer-science/note/theme-shoka-doc/)

[shoka 主题搭建过程 - note | 囧 o(╯□╰)o 囧](https://blog.twinkling.top/2023/11/16/note/shoka%20主题搭建过程/)

[关于 Shoka 图床修复 | Yiqiu Note](https://ui123456ax.github.io/2023/06/23/02_Shoka图床修复/)

[Shoka 使用自己的图床 | FuFan](https://fufan1025.github.io/2024/03/07/Shoka使用自己的图床/)

[Hexo-theme-ShokaX](https://docs.kaitaku.xyz/)
