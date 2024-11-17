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

# setup

## 概述

Vue3 中一个新的配置项，值是一个函数，它是 `Composition API` **“表演的舞台**_**”**_，组件中所用到的：数据、方法、计算属性、监视......等等，均配置在 `setup` 中

**特点：**

- setup 函数返回的对象中的内容，可直接在模板中使用
- setup 访问 this 是`undefined`
- setup 函数会在 `beforeCreate` 之前调用，它是“领先”所有钩子执行的

```vue
<script lang="ts">
  export default {
    name: 'Person',
    setup() {
      // 原来写在data中（注意：此时的name不是响应式数据）
      let name = '张三'
      function changeName() {
        name = '李四'
        console.log(name)
      }
      // 返回一个对象，对象中的内容，模板中可以直接使用
      return { name, changeName }
    }
  }
</script>
```

## setup 的返回值

- 若返回一个**对象**：则对象中的：属性、方法等，在模板中均可以直接使用**（重点关注）**

- 若返回一个**函数**：则可以自定义渲染内容，代码如下：

  ```jsx
  setup() {
    return () => '你好啊！'
  }
  ```

## 与 Options API 的关系

- Vue2 的配置（data、methods ......）中**可以访问到** setup 中的属性、方法
- 但在 setup 中**不能访问到** Vue2 的配置（data、methods ......）
- 如果与 Vue2 冲突，则 setup 优先

## setup 语法糖

可以把 setup 独立出去

```vue
<script lang="ts">
  export default {
    name:'Person',
  }
</script>

<script setup lang="ts">
  let name = '张三'
  function changName() {
    name = '李四' // 注意：此时这么修改name页面是不变化的
  }
</script>
```

借助 vite 中的插件简化，去指定组件名字

```sh
# 安装插件
npm i vite-plugin-vue-setup-extend -D
```

修改 vite.config.ts 文件

```jsx
import { defineConfig } from 'vite'
import VueSetupExtend from 'vite-plugin-vue-setup-extend'

export default defineConfig({
  plugins: [ VueSetupExtend() ]
})
```

此时代码可简化为：

```vue
<script setup lang="ts" name="Person">
  
</script>
```

# 响应式数据

## ref

- **作用**：定义响应式变量
- **语法**：`let xxx = ref(初始值)`
- **返回值**：一个 `RefImpl` 的实例对象，简称 **ref对象 或 ref**，ref 对象的 value 属性是响应式的
- **注意点**：
  - JS 中操作数据需要：`xxx.value`，但模板中不需要  `.value`，直接使用即可
  - 对于 `let name = ref('张三')` 来说，`name` 不是响应式的，`name.value` 是响应式的

```vue
<script setup lang="ts" name="Person">
  import { ref } from 'vue'
  // name是一个RefImpl的实例对象，简称ref对象，它们的value属性是响应式的
  let name = ref('张三')
  function changeName() {
    // JS中操作ref对象时候需要.value
    name.value = '李四'
    console.log(name.value)
  }
</script>
```

## reactive

- **作用**：定义一个**响应式对象**（基本类型不要用它，要用 `ref`，否则报错）
- **语法**：`let 响应式对象= reactive(源对象)`
- **返回值**：一个 `Proxy` 的实例对象，简称：响应式对象
- **注意点**：reactive 定义的响应式数据是“深层次”的

```vue
<script lang="ts" setup name="Person">
  let games = reactive([
  { id: 1, name: '开心消消乐' },
  { id: 2, name: '王者荣耀' },
  { id: 3, name: '蛋仔派对' }
])
  function changeFirstGame() {
  games[0].name = '英雄联盟'
}
</script>
```

## 两者对比

宏观角度看：

- ref：定义**基本类型数据**、**对象类型数据**
- reactive：定义**对象类型数据**

区别：

- ref 创建的变量必须使用 `.value`（可以使用 volar 插件自动添加 `.value`）

  ![Snipaste_2024-11-17_17-57-04](https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/Snipaste_2024-11-17_17-57-04.jpg)

- reactive 重新分配一个新对象，会**失去**响应式（可以使用 `Object.assign` 去整体替换）

使用原则：

- 若需要一个基本类型的响应式数据，必须使用 `ref`
- 若需要一个响应式对象，层级不深，`ref`、`reactive` 都可以
- 若需要一个响应式对象，且层级较深，推荐使用 `reactive`
