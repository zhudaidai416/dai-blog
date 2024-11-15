---
title: Vue3语法学习
date: 2024-11-14 17:04:45
category:
  - [计算机与科学, Vue]
tags: Vue3
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/4-2.jpg
---

# 参考

{% links %}

- site: Vue3 官方文档
  owner: Vue3
  url: https://cn.vuejs.org/
  color: "#f2debd"
- site: 尚硅谷 Vue3 入门到实战
  owner: 尚硅谷
  url: https://www.bilibili.com/video/BV1Za4y1r7KE
  color: "#e27386"

{% endlinks %}

# 编码规范

在 Vue3 中

- 编程语言：JavaScript、TypeScript
- 代码风格：组合式 API、选项式 API
- 简写形式：setup 语法糖

# 介绍

🐈︎

# 创建项目

## vue-cli

[📑 官方文档](https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create)

```sh
# 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version

# 安装或者升级你的@vue/cli
npm install -g @vue/cli

# 执行创建命令
vue create vue_test

# 随后选择3.x
# Choose a version of Vue.js that you want to start the project with (Use arrow keys)
# > 3.x
#   2.x

# 启动
cd vue_test
npm run serve
```

## vite

[📑 官方文档](https://cn.vuejs.org/guide/quick-start#creating-a-vue-application)

新一代前端构建工具，官网地址：[https://vitejs.cn](https://vitejs.cn/)

**优势：**

- 轻量快速的热重载（HMR），能实现极速的服务启动

- 对 TypeScript、JSX、CSS 等支持开箱即用

- 真正的按需编译，不再等待整个应用编译完成

- webpack 构建 与 vite 构建对比图如下：

  ![1683167182037-71c78210-8217-4e7d-9a83-e463035efbbe](https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1683167182037-71c78210-8217-4e7d-9a83-e463035efbbe.png)

  ![1683167204081-582dc237-72bc-499e-9589-2cdfd452e62f](https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1683167204081-582dc237-72bc-499e-9589-2cdfd452e62f.png)

```sh
# 创建命令
npm create vue@latest

# 具体配置
√ Project name: vue3_test  # 配置项目名称
√ Add TypeScript?  Yes  # 是否添加TypeScript支持
√ Add JSX Support?  No  # 是否添加JSX支持
√ Add Vue Router for Single Page Application development?  No  # 是否添加路由环境
√ Add Pinia for state management?  No  # 是否添加pinia环境
√ Add Vitest for Unit Testing?  No  # 是否添加单元测试
√ Add an End-to-End Testing Solution? » No  # 是否添加端到端测试方案
√ Add ESLint for code quality?  Yes  # 是否添加ESLint语法检查
√ Add Prettier for code formatting?  No  # 是否添加Prettiert代码格式化
```

- Vite 项目中，`index.html` 是项目的入口文件，在项目最外层
- 加载 `index.html` 后，Vite 解析 `<script type="module" src="xxx">` 指向的 `JavaScript`
- Vue3 中是通过 `createApp` 函数创建一个应用实例

# 语法 👇🏻

# OptionsAPI 与 CompositionAPI

- Vue2 的 API 设计是 Options（配置）风格的
- Vue3 的 API 设计是 Composition（组合）风格的

## Options API 的弊端

Options 类型的 API，数据、方法、计算属性等，是分散在：`data`、`methods`、`computed`中的

若想新增或者修改一个需求，就需要分别修改：`data`、`methods`、`computed`，不便于维护和复用

<img src="https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1696662197101-55d2b251-f6e5-47f4-b3f1-d8531bbf9279.gif" alt="1.gif" style="zoom:70%;border-radius:10px" />

<img src="https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1696662200734-1bad8249-d7a2-423e-a3c3-ab4c110628be.gif" alt="2.gif" style="zoom:70%;border-radius:10px" />

## Composition API 的优势

可以用函数的方式，更加优雅的组织代码，让相关功能的代码更加有序的组织在一起

<img src="https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1696662249851-db6403a1-acb5-481a-88e0-e1e34d2ef53a.gif" alt="3.gif" style="height:300px;border-radius:10px" />

<img src="https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1696662256560-7239b9f9-a770-43c1-9386-6cc12ef1e9c0.gif" alt="4.gif" style="height:300px;border-radius:10px" />
