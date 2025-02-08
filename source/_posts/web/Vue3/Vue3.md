---
title: Vue3 语法学习
date: 2024-11-14 17:04:45
category:
  - [前端, Vue3]
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

🐈︎ [官方发版地址](https://github.com/vuejs/core/releases/tag/v3.0.0)

- 性能提升
- 源码升级
  - 使用 `Proxy` 代替 `defineProperty` 实现响应式
  - 重写虚拟 `DOM` 的实现和 `Tree-Shaking`
- 拥抱 TypeScript

**新的特性：**

- Composition API（组合 API）：

  - setup

  - ref 与 reactive

  - computed 与 watch

    ...

- 新的内置组件：

  - Fragment

  - Teleport

  - Suspense

    ...

- 其他改变：

  - 新的生命周期钩子

  - data 选项应始终被声明为一个函数

  - 移除 keyCode 支持作为 v-on 的修饰符

    ...

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
- setup 访问 this 是 `undefined`
- setup 函数会在 `beforeCreate` 之前调用，它是“领先”所有钩子执行的

```html
<template>
  <h2>姓名：{{ name }}</h2>
  <button @click="changeName">修改名字</button>
</template>

<script lang="ts">
  export default {
    name: "Person",
    setup() {
      // 原来写在data中（注意：此时的name不是响应式数据）
      let name = "张三";
      function changeName() {
        name = "李四";
        console.log(name);
      }
      // 返回一个对象，对象中的内容，模板中可以直接使用
      return { name, changeName };
    }
  };
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

```html
<template>
  <h2>姓名：{{ name }}</h2>
  <button @click="changeName">修改名字</button>
</template>

<script lang="ts">
  export default {
    name: "Person"
  };
</script>

<script setup lang="ts">
  let name = "张三";
  function changName() {
    name = "李四"; // 注意：此时这么修改name页面是不变化的
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
import { defineConfig } from "vite";
import VueSetupExtend from "vite-plugin-vue-setup-extend";

export default defineConfig({
  plugins: [VueSetupExtend()]
});
```

此时代码可简化为：

```html
<script setup lang="ts" name="Person"></script>
```

# 响应式数据

## ref：基本类型的响应式数据

- **作用**：定义响应式变量
- **语法**：`let xxx = ref(初始值)`
- **返回值**：一个 `RefImpl` 的实例对象，简称 **ref 对象 或 ref**，ref 对象的 value 属性是响应式的
- **注意点**：
  - JS 中操作数据需要：`xxx.value`，但模板中不需要 `.value`，直接使用即可
  - 对于 `let name = ref('张三')` 来说，`name` 不是响应式的，`name.value` 是响应式的

```html
<template>
  <h2>姓名：{{ name }}</h2>
  <button @click="changeName">修改名字</button>
</template>

<script setup lang="ts" name="Person">
  import { ref } from "vue";
  // name是一个RefImpl的实例对象，简称ref对象，它们的value属性是响应式的
  let name = ref("张三");
  function changeName() {
    // JS中操作ref对象时候需要.value
    name.value = "李四";
    console.log(name.value);
  }
</script>
```

## reactive：对象类型的响应式数据

- **作用**：定义一个**响应式对象**（基本类型不要用它，要用 `ref`，否则报错）
- **语法**：`let 响应式对象= reactive(源对象)`
- **返回值**：一个 `Proxy` 的实例对象，简称：响应式对象
- **注意点**：reactive 定义的响应式数据是“深层次”的

```html
<template>
  <h2>游戏列表：</h2>
  <ul>
    <li v-for="g in games" :key="g.id">{{ g.name }}</li>
  </ul>
  <button @click="changeFirstGame">修改游戏</button>
</template>

<script lang="ts" setup name="Person">
  import { reactive } from "vue";
  let games = reactive([
    { id: 1, name: "开心消消乐" },
    { id: 2, name: "王者荣耀" },
    { id: 3, name: "蛋仔派对" }
  ]);
  function changeFirstGame() {
    games[0].name = "英雄联盟";
  }
</script>
```

## ref：对象类型的响应式数据

- 其实 ref 接收的数据可以是：**基本类型**、**对象类型**
- 若 ref 接收的是对象类型，内部其实也是调用了 reactive 函数

```html
<template>
  <h2>游戏列表：</h2>
  <ul>
    <li v-for="g in games" :key="g.id">{{ g.name }}</li>
  </ul>
  <button @click="changeFirstGame">修改游戏</button>
</template>

<script lang="ts" setup name="Person">
  import { ref } from "vue";
  let games = ref([
    { id: 1, name: "开心消消乐" },
    { id: 2, name: "王者荣耀" },
    { id: 3, name: "蛋仔派对" }
  ]);
  function changeFirstGame() {
    games.value[0].name = "英雄联盟";
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

  +++success 示例

  ```html
  <template>
    <h2>一辆{{ car.name }},价值：{{ car.price }}</h2>
    <button @click="changeCar">修改车信息</button>
  </template>

  <script lang="ts" setup name="Person">
    import { ref, reactive } from "vue";
    let car = reactive({
      name: "奔驰",
      price: 100000
    });
    function changePrice() {
      car.price += 10000;
    }
    function changeCar() {
      car = { name: "宝马", price: 200000 }; // 页面不更新
      car = Object.assign(car, { name: "宝马", price: 200000 });
      // car.value = { name: "宝马", price: 200000 } // 若此时car用ref来定义
    }
  </script>
  ```

  +++

使用原则：

- 若需要一个基本类型的响应式数据，必须使用 `ref`
- 若需要一个响应式对象，层级不深，`ref`、`reactive` 都可以
- 若需要一个响应式对象，且层级较深，推荐使用 `reactive`

# toRefs 与 toRef

- 作用：将一个响应式对象中的每一个属性，转换为 `ref` 对象（解构对象使每个属性成为响应式数据）
- 备注：`toRefs` 与 `toRef` 功能一致，但 `toRefs` 可以批量转换

+++success 示例

```html
<template>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>性别：{{ person.sex }}</h2>
  <button @click="changeName">修改名字</button>
  <button @click="changeAge">修改年龄</button>
  <button @click="changeSex">修改性别</button>
</template>

<script lang="ts" setup name="Person">
  import { ref, reactive, toRefs, toRef } from 'vue'

  let person = reactive({ name: '张三', age: 18, sex: '男's })
  // 通过toRefs将person对象中的n个属性批量取出，且依然保持响应式的能力
  let { name, sex } =  toRefs(person)
  // 通过toRef将person对象中的sex属性取出，且依然保持响应式的能力
  let age = toRef(person,'age')

  function changeName() {
    name.value += '~'
  }
  function changeAge() {
    age.value += 1
  }
  function changeSex() {
    sex.value = '女'
  }
</script>
```

+++

# computed

根据已有数据计算出新数据

+++sucess 示例

```html
<template>
  <div>
    姓：<input type="text" v-model="firstName" /> <br />
    名：<input type="text" v-model="lastName" /> <br />
    全名：<span>{{ fullName }}</span> <br />
    <button @click="changeFullName">全名改为：li-si</button>
  </div>
</template>

<script setup lang="ts" name="App">
  import { ref, computed } from "vue";

  let firstName = ref("zhang");
  let lastName = ref("san");

  // 只读取，不修改
  // let fullName = computed(() => {
  //   return firstName.value + "-" + lastName.value;
  // });

  // 可读可写
  let fullName = computed({
    // 读取
    get() {
      return firstName.value + "-" + lastName.value;
    },
    // 修改
    set(val) {
      firstName.value = val.split("-")[0];
      lastName.value = val.split("-")[1];
    }
  });
  function changeFullName() {
    fullName.value = "li-si";
  }
</script>
```

+++

# watch

- 作用：监视数据的变化
- 特点：Vue3 中的 watch 只能监视以下**四种数据**：
  - ref 定义的数据
  - reactive 定义的数据
  - 函数返回一个值（getter 函数）
  - 一个包含上述内容的数组

## 情况一

监视 ref 定义的【基本类型】数据：直接写数据名即可，监视的是其 value 值的改变

+++success 示例

```html
<template>
  <h1>情况一：监视【ref】定义的【基本类型】数据</h1>
  <h2>当前求和为：{{ sum }}</h2>
  <button @click="changeSum">点我sum+1</button>
</template>

<script lang="ts" setup name="Person">
  import { ref, watch } from "vue";

  let sum = ref(0);
  function changeSum() {
    sum.value += 1;
  }
  const stopWatch = watch(sum, (newValue, oldValue) => {
    console.log("sum变化了", newValue, oldValue);
    if (newValue >= 10) {
      stopWatch();
    }
  });
</script>
```

+++

## 情况二

监视 ref 定义的【对象类型】数据：直接写数据名，监视的是对象的【地址值】

若想监视对象内部的数据，要手动开启深度监视

> :warning: 注：
>
> - 若修改的是 ref 定义的对象中的属性：newValue 和 oldValue 都是新值，因为它们是同一个对象
>
> - 若修改整个 ref 定义的对象：newValue 是新值， oldValue 是旧值，因为不是同一个对象了

+++success 示例

```html
<template>
  <h1>情况二：监视【ref】定义的【对象类型】数据</h1>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <button @click="changeName">修改名字</button>
  <button @click="changeAge">修改年龄</button>
  <button @click="changePerson">修改整个人</button>
</template>

<script lang="ts" setup name="Person">
  import { ref, watch } from "vue";

  let person = ref({
    name: "张三",
    age: 18
  });
  function changeName() {
    person.value.name += "~";
  }
  function changeAge() {
    person.value.age += 1;
  }
  function changePerson() {
    person.value = { name: "李四", age: 90 };
  }
  /* 
    情况1：监视的是对象的地址值，若想监视对象内部属性的变化，需要手动开启深度监视
    第一个参数：被监视的数据
    第二个参数：监视的回调
    第三个参数：配置对象（deep、immediate等等...）
    
    watch(person, (newValue, oldValue) => {
      console.log("person变化了", newValue, oldValue)
    })
  */

  // 情况2：深度监视
  watch(
    person,
    (newValue, oldValue) => {
      console.log("person变化了", newValue, oldValue);
    },
    { deep: true }
  );
</script>
```

+++

## 情况三

监视 reactive 定义的【对象类型】数据，且默认开启了深度监视

+++success 示例

```html
<template>
  <h1>情况三：监视【reactive】定义的【对象类型】数据</h1>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <button @click="changeName">修改名字</button>
  <button @click="changeAge">修改年龄</button>
  <button @click="changePerson">修改整个人</button>
  <hr />
  <h2>测试：{{ obj.a.b.c }}</h2>
  <button @click="test">修改obj.a.b.c</button>
</template>

<script lang="ts" setup name="Person">
  import { reactive, watch } from "vue";

  let person = reactive({
    name: "张三",
    age: 18
  });
  let obj = reactive({
    a: {
      b: {
        c: 666
      }
    }
  });
  function changeName() {
    person.name += "~";
  }
  function changeAge() {
    person.age += 1;
  }
  function changePerson() {
    Object.assign(person, { name: "李四", age: 80 });
  }
  function test() {
    obj.a.b.c = 888;
  }

  // 默认是开启深度监视的
  watch(person, (newValue, oldValue) => {
    console.log("person变化了", newValue, oldValue);
  });
  watch(obj, (newValue, oldValue) => {
    console.log("Obj变化了", newValue, oldValue);
  });
</script>
```

+++

## 情况四

监视 ref 或 reactive 定义的【对象类型】数据中的**某个属性**，注意点如下：

- 若该属性值**不是**【对象类型】，需要写成函数形式
- 若该属性值是**依然**是【对象类型】，可直接编，也可写成函数，建议写成函数

结论：监视的要是对象里的属性，那么最好写函数式，注意点：若是对象监视的是地址值，需要关注对象内部，需要手动开启深度监视

+++success 示例

```html
<template>
  <h1>情况四：监视【ref】或【reactive】定义的【对象类型】数据中的某个属性</h1>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>汽车：{{ person.car.c1 }}、{{ person.car.c2 }}</h2>
  <button @click="changeName">修改名字</button>
  <button @click="changeAge">修改年龄</button>
  <button @click="changeC1">修改第一台车</button>
  <button @click="changeC2">修改第二台车</button>
  <button @click="changeCar">修改整个车</button>
</template>

<script lang="ts" setup name="Person">
  import { reactive, watch } from "vue";

  let person = reactive({
    name: "张三",
    age: 18,
    car: {
      c1: "奔驰",
      c2: "宝马"
    }
  });
  function changeName() {
    person.name += "~";
  }
  function changeAge() {
    person.age += 1;
  }
  function changeC1() {
    person.car.c1 = "奥迪";
  }
  function changeC2() {
    person.car.c2 = "大众";
  }
  function changeCar() {
    person.car = { c1: "雅迪", c2: "爱玛" };
  }

  // 监视响应式对象中的某个属性，且该属性是基本类型的，要写成函数式
  /* 
  watch(()=> person.name,(newValue,oldValue) => {
    console.log('person.name变化了',newValue,oldValue)
  }) 
*/

  // 监视响应式对象中的某个属性，且该属性是对象类型的，可以直接写，也能写函数，更推荐写函数
  watch(
    () => person.car,
    (newValue, oldValue) => {
      console.log("person.car变化了", newValue, oldValue);
    },
    { deep: true }
  );
</script>
```

+++

## 情况五

监视上述的多个数据

+++success 示例

```html
<template>
  <h1>情况五：监视上述的多个数据</h1>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>汽车：{{ person.car.c1 }}、{{ person.car.c2 }}</h2>
  <button @click="changeName">修改名字</button>
  <button @click="changeAge">修改年龄</button>
  <button @click="changeC1">修改第一台车</button>
  <button @click="changeC2">修改第二台车</button>
  <button @click="changeCar">修改整个车</button>
</template>

<script lang="ts" setup name="Person">
  import { reactive, watch } from "vue";

  let person = reactive({
    name: "张三",
    age: 18,
    car: {
      c1: "奔驰",
      c2: "宝马"
    }
  });
  function changeName() {
    person.name += "~";
  }
  function changeAge() {
    person.age += 1;
  }
  function changeC1() {
    person.car.c1 = "奥迪";
  }
  function changeC2() {
    person.car.c2 = "大众";
  }
  function changeCar() {
    person.car = { c1: "雅迪", c2: "爱玛" };
  }

  watch(
    [() => person.name, person.car],
    (newValue, oldValue) => {
      console.log("person.car变化了", newValue, oldValue);
    },
    { deep: true }
  );
</script>
```

+++

# watchEffect

- 立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行该函数

- watch 对比 watchEffect

  - 都能监听响应式数据的变化，不同的是监听数据变化的方式不同

  - watch：要明确指出监视的数据

  - watchEffect：不用明确指出监视的数据（函数中用到哪些属性，那就监视哪些属性）

+++success 示例

```html
<template>
  <h1>需求：水温达到50℃，或水位达到20cm，则联系服务器</h1>
  <h2 id="demo">水温：{{ temp }}</h2>
  <h2>水位：{{ height }}</h2>
  <button @click="changePrice">水温+1</button>
  <button @click="changeSum">水位+10</button>
</template>

<script lang="ts" setup name="Person">
  import { ref, watch, watchEffect } from "vue";

  let temp = ref(0);
  let height = ref(0);
  function changePrice() {
    temp.value += 10;
  }
  function changeSum() {
    height.value += 1;
  }

  // watch：需要明确指出要监视的数据
  watch([temp, height], value => {
    const [newTemp, newHeight] = value; // 从value中获取最新的temp值、height值
    if (newTemp >= 50 || newHeight >= 20) {
      console.log("联系服务器");
    }
  });

  // watchEffect：不用
  const stopWatch = watchEffect(() => {
    if (temp.value >= 50 || height.value >= 20) {
      console.log(document.getElementById("demo")?.innerText);
      console.log("联系服务器");
    }
    // 水温达到100，或水位达到50，取消监视
    if (temp.value === 100 || height.value === 50) {
      console.log("清理了");
      stopWatch();
    }
  });
</script>
```

+++

# 标签的 ref 属性

用于注册模板引用

- 用在普通 DOM 标签上，获取的是 DOM 节点

- 用在组件标签上，获取的是组件实例对象

+++success 示例：普通 DOM 标签

```html
<template>
  <h1 ref="title1">尚硅谷</h1>
  <h2 ref="title2">前端</h2>
  <h3 ref="title3">Vue</h3>
  <button @click="showLog">点我打印元素</button>
</template>

<script lang="ts" setup name="Person">
  import { ref } from "vue";

  let title1 = ref();
  let title2 = ref();
  let title3 = ref();
  function showLog() {
    // 通过id获取元素
    const t1 = document.getElementById("title1");
    console.log((t1 as HTMLElement).innerText);
    console.log((<HTMLElement>t1).innerText);
    console.log(t1?.innerText);

    // 通过ref获取元素
    console.log(title1.value);
    console.log(title2.value);
    console.log(title3.value);
  }
</script>
```

+++

+++success 示例：组件标签

```html
<!-- 父组件App.vue -->
<template>
  <Person ref="ren" />
  <button @click="test">测试</button>
</template>

<script lang="ts" setup name="App">
  import Person from "./components/Person.vue";
  import { ref } from "vue";

  let ren = ref();
  function test() {
    console.log(ren.value.name);
    console.log(ren.value.age);
  }
</script>

<!-- 子组件Person.vue中要使用defineExpose暴露内容 -->
<script lang="ts" setup name="Person">
  import { ref, defineExpose } from "vue";
  let name = ref("张三");
  let age = ref(18);
  // 使用defineExpose将组件中的数据交给外部
  defineExpose({ name, age });
</script>
```

+++

# props

+++success 示例

types/index.ts

```ts
// 定义接口，限制Person对象的格式
export interface PersonInter {
  id: string;
  name: string;
  age: number;
}

// 定义自定义类型Persons
export type Persons = Array<PersonInter>;
```

App.vue（父组件）

```html
<template>
  <Person :list="persons" />
</template>

<script lang="ts" setup name="App">
  import Person from "./components/Person.vue";
  import { reactive } from "vue";
  import { type Persons } from "./types";

  let persons = reactive<Persons>([
    { id: "e98219e12", name: "张三", age: 18 },
    { id: "e98219e13", name: "李四", age: 19 },
    { id: "e98219e14", name: "王五", age: 20 }
  ]);
</script>
```

Person.vue（子组件）

```html
<template>
  <ul>
    <li v-for="item in list" :key="item.id">{{ item.name }}--{{ item.age }}</li>
  </ul>
</template>

<script lang="ts" setup name="Person">
  import { defineProps } from "vue";
  import { type PersonInter } from "@/types";

  // 写法1：仅接收
  // const props = defineProps(['list'])

  // 写法2：接收 + 限制类型
  // defineProps<{list:Persons}>()

  // 写法3：接收 + 限制类型 + 指定默认值 + 限制必要性
  let props = withDefaults(defineProps<{ list?: Persons }>(), {
    list: () => [{ id: "asdasg01", name: "呆呆", age: 18 }]
  });
  console.log(props);
</script>
```

+++

# 生命周期

Vue 组件实例在创建时要经历一系列的初始化步骤，在此过程中 Vue 会在合适的时机，调用特定的函数，从而让开发者有机会在特定阶段运行自己的代码，这些特定的函数统称为：生命周期钩子

**Vue2 的生命周期：**

- 创建阶段：`beforeCreate`、`created`

- 挂载阶段：`beforeMount`、`mounted`

- 更新阶段：`beforeUpdate`、`updated`

- 销毁阶段：`beforeDestroy`、`destroyed`

**Vue3 的生命周期：**

- 创建阶段：`setup`

- 挂载阶段：`onBeforeMount`、`onMounted`

- 更新阶段：`onBeforeUpdate`、`onUpdated`

- 卸载阶段：`onBeforeUnmount`、`onUnmounted`

常用的钩子：`onMounted`（挂载完毕）、`onUpdated`（更新完毕）、`onBeforeUnmount`（卸载之前）

+++success 示例：生命周期

```html
<template>
  <h2>当前求和为：{{ sum }}</h2>
  <button @click="changeSum">点我sum+1</button>
</template>

<script lang="ts" setup name="Person">
  import {
    ref,
    onBeforeMount,
    onMounted,
    onBeforeUpdate,
    onUpdated,
    onBeforeUnmount,
    onUnmounted
  } from "vue";

  let sum = ref(0);
  function changeSum() {
    sum.value += 1;
  }
  console.log("setup");
  onBeforeMount(() => {
    console.log("挂载之前");
  });
  onMounted(() => {
    console.log("挂载完毕");
  });
  onBeforeUpdate(() => {
    console.log("更新之前");
  });
  onUpdated(() => {
    console.log("更新完毕");
  });
  onBeforeUnmount(() => {
    console.log("卸载之前");
  });
  onUnmounted(() => {
    console.log("卸载完毕");
  });
</script>
```

+++

# 自定义 hook

- 什么是 hook？本质是一个函数，把 setup 函数中使用的 `Composition API` 进行了封装，类似于 vue2.x 中的 `mixin`

- 自定义 hook 的优势：复用代码，让 setup 中的逻辑更清楚易懂

+++success 示例

useSum.ts

```ts
import { ref, onMounted } from "vue";

export default function () {
  let sum = ref(0);
  const increment = () => {
    sum.value += 1;
  };
  const decrement = () => {
    sum.value -= 1;
  };
  onMounted(() => {
    increment();
  });

  // 向外部暴露数据
  return { sum, increment, decrement };
}
```

useDog.ts

```ts
import { reactive, onMounted } from "vue";
import axios, { AxiosError } from "axios";

export default function () {
  let dogList = reactive<string[]>([]);
  async function getDog() {
    try {
      // 发请求
      let { data } = await axios.get(
        "https://dog.ceo/api/breed/pembroke/images/random"
      );
      // 维护数据
      dogList.push(data.message);
    } catch (error) {
      // 处理错误
      const err = <AxiosError>error;
      console.log(err.message);
    }
  }
  onMounted(() => {
    getDog();
  });

  // 向外部暴露数据
  return { dogList, getDog };
}
```

组件具体使用

```html
<template>
  <h2>当前求和为：{{ sum }}</h2>
  <button @click="increment">点我+1</button>
  <button @click="decrement">点我-1</button>
  <hr />
  <img
    v-for="(u, index) in dogList.urlList"
    :key="index"
    :src="(u as string)"
  />
  <span v-show="dogList.isLoading">加载中...</span><br />
  <button @click="getDog">再来一只修勾</button>
</template>

<script lang="ts">
  import { defineComponent } from "vue";

  export default defineComponent({
    name: "App"
  });
</script>

<script setup lang="ts">
  import useSum from "@/hooks/useSum";
  import useDog from "@/hooks/useDog";

  let { sum, increment, decrement } = useSum();
  let { dogList, getDog } = useDog();
</script>
```

+++
