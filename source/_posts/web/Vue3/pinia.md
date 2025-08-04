---
title: Vue3 pinia
date: 2024-11-22 16:28:56
category:
  - [前端, Vue3]
tags: Vue3
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/4-4.jpg
---

# [pinia](https://pinia.vuejs.org/zh/)

# 概念

集中式状态（数据）管理

- Vue2：vuex
- Vue3：pinia
- React：redux

# 测试效果

![img](https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/pinia_example.gif)

# 搭建 pinia 环境

```sh
npm install pinia
```

main.ts

```ts
import { createApp } from "vue";
import App from "./App.vue";

// 引入createPinia，用于创建pinia
import { createPinia } from "pinia";

// 创建pinia
const pinia = createPinia();
const app = createApp(App);

// 使用插件
app.use(pinia);
app.mount("#app");
```

此时开发者工具中已经有了 `pinia` 选项

![img](https://daiblog.oss-cn-chengdu.aliyuncs.com/vue/1684309952481-c67f67f9-d1a3-4d69-8bd6-2b381e003f31.png)

# 存储+读取数据

- `Store`是一个保存：**状态**、**业务逻辑** 的实体，每个组件都可以**读取**、**写入**它。
- 它有三个概念：`state`、`getter`、`action`，相当于组件中的： `data`、 `computed` 和 `methods`

+++success 示例

src/store/count.ts

```ts
// 引入defineStore用于创建store
import { defineStore } from "pinia";

// 定义并暴露一个store
export const useCountStore = defineStore("count", {
  // 动作
  actions: {},
  // 状态
  state() {
    return {
      sum: 6
    };
  },
  // 计算
  getters: {}
});
```

src/store/talk.ts

```ts
// 引入defineStore用于创建store
import { defineStore } from "pinia";

// 定义并暴露一个store
export const useTalkStore = defineStore("talk", {
  // 动作
  actions: {},
  // 状态
  state() {
    return {
      talkList: [
        { id: "1", content: "你今天有点怪，哪里怪？怪好看的！" },
        { id: "2", content: "草莓、蓝莓、蔓越莓，你想我了没？" },
        { id: "3", content: "心里给你留了一块地，我的死心塌地" }
      ]
    };
  },
  // 计算
  getters: {}
});
```

组件中使用

```html
<template>
  <h2>当前求和为：{{ sumStore.sum }}</h2>
  <select v-model.number="n">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>
  <button @click="add">加</button>
  <button @click="minus">减</button>
</template>

<script setup lang="ts" name="Count">
  // 引入对应的useXxxxxStore
  import { useSumStore } from "@/store/sum";

  // 调用useXxxxxStore得到对应的store
  const sumStore = useSumStore();
</script>
```

```html
<template>
  <ul>
    <li v-for="item in talkStore.talkList" :key="item.id">
      {{ item.content }}
    </li>
  </ul>
</template>

<script setup lang="ts" name="Count">
  import axios from "axios";
  import { useTalkStore } from "@/store/talk";

  const talkStore = useTalkStore();
</script>
```

+++

# 修改数据（三种方式）

方式一：直接修改

```ts
countStore.sum = 666;
```

方式二：批量修改

```ts
countStore.$patch({
  sum: 999,
  school: "xxx大学"
});
```

方式三：借助 `action` 修改（`action` 中可以编写一些业务逻辑）

```ts
import { defineStore } from "pinia";

export const useCountStore = defineStore("count", {
  ...
  actions: {
    // 加
    increment(value: number) {
      if (this.sum < 10) {
        // 操作countStore中的sum
        this.sum += value;
      }
    },
    // 减
    decrement(value: number) {
      if (this.sum > 1) {
        this.sum -= value;
      }
    }
  }
});
```

组件中调用

```ts
// 使用countStore
const countStore = useCountStore();

// 调用对应action
countStore.increment(n.value);
```

# storeToRefs

借助 `storeToRefs` 将 `store` 中的数据转为 `ref` 对象，方便在模板中使用

> :warning: 注：pinia 提供的 `storeToRefs` 只会将数据做转换，而 Vue 的 `toRefs` 会转换 `store` 中数据

```html
<template>
  <h2>当前求和为：{{ sum }}</h2>
</template>

<script setup lang="ts" name="Count">
  import { useCountStore } from "@/store/count";
  import { storeToRefs } from "pinia";

  const countStore = useCountStore();

  // let { sum, school, address } = toRefs(countStore); // toRefs会将方法一起包裹

  // 使用storeToRefs转换countStore，随后解构
  // storeToRefs只会关注store中的数据，不会对方法进行ref包裹
  const { sum } = storeToRefs(countStore);
</script>
```

# getters

当 state 中的数据，需要经过处理后再使用时，可以使用 getters 配置

追加 getters 配置

```ts
// 引入defineStore用于创建store
import { defineStore } from "pinia";

// 定义并暴露一个store
export const useCountStore = defineStore("count", {
  // 动作
  actions: {},
  // 状态
  state() {
    return {
      sum: 1,
      school: "xxx大学"
    };
  },
  // 计算
  getters: {
    bigSum: (state): number => state.sum * 10,
    upperSchool(): string {
      return this.school.toUpperCase();
    }
  }
});
```

组件中读取数据

```ts
import { useCountStore } from "@/store/count";
import { storeToRefs } from "pinia";

const countStore = useCountStore();

const { increment, decrement } = countStore;
let { sum, school, bigSum, upperSchool } = storeToRefs(countStore);
```

# $subscribe

通过 store 的 `$subscribe()` 方法侦听 state 及其变化

```ts
import { useTalkStore } from "@/store/loveTalk";
import { storeToRefs } from "pinia";

const talkStore = useTalkStore();
const { talkList } = storeToRefs(talkStore);

talkStore.$subscribe((mutate, state) => {
  console.log("talkStore数据发生变化了");
  console.log("LoveTalk", mutate, state);
  localStorage.setItem("talk", JSON.stringify(talkList.value));
});
```

# 选项式写法

```ts
import { defineStore } from "pinia";
import axios from "axios";
import { nanoid } from "nanoid";

export const useTalkStore = defineStore("talk", {
  state() {
    return {
      talkList: JSON.parse(localStorage.getItem("talkList") as string) || []
      // [
      //   { id: "1", title: "近朱者赤，近你者甜" },
      //   { id: "2", title: "你知道我的缺点是什么吗? 是缺点你" },
      //   { id: "3", title: "只许州官放火，不许你离开我" },
      //   { id: "4", title: "今天我只做两件事呼吸跟想你" }
      // ]
    };
  },
  actions: {
    async getTalk() {
      let {
        data: { content: title }
      } = await axios.get("https://api.uomg.com/api/rand.qinghua?format=json");
      let obj = {
        id: nanoid(),
        title
      };
      this.talkList.unshift(obj);
    }
  }
});
```

# 组合式写法

```ts
import { defineStore } from "pinia";
import axios from "axios";
import { nanoid } from "nanoid"; // 随机生成id插件
import { reactive } from "vue";

export const useTalkStore = defineStore("talk", () => {
  // talkList就是state
  const talkList = reactive(
    JSON.parse(localStorage.getItem("talkList") as string) || []
  );

  // getATalk函数相当于action
  async function getATalk() {
    // 发请求，下面这行的写法是：连续解构赋值+重命名
    let {
      data: { content: title }
    } = await axios.get("https://api.uomg.com/api/rand.qinghua?format=json");
    // 把请求回来的字符串，包装成一个对象
    let obj = {
      id: nanoid(),
      title
    };
    // 放到数组中
    talkList.unshift(obj);
  }
  return { talkList, getATalk };
});
```
