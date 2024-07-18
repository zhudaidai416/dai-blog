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

+++

[LeanCloud](https://console.leancloud.cn/) ➡ 登录 ➡ 数据存储 ➡ 结构化数据 ➡ Comment ➡ 数据（mailMd5 的值）

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

访问不到

设置 ➡ 安全中心 ➡ Web 安全域名 ➡ 添加域名即可访问
