---
title: 其它API
date: 2024-12-02 14:11:14
category:
  - [计算机与科学, Vue]
tags: Vue3
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/4-6.jpg
---

# shallowRef 与 shallowReactive

## shallowRef

创建一个响应式数据，但只对顶层属性进行响应式处理

特点：只跟踪引用值的变化，不关心值内部的属性变化

```ts
import { shallowRef } from "vue";
let myVar = shallowRef(initialValue);
```

## shallowReactive

创建一个浅层响应式对象，只会使对象的最顶层属性变成响应式的，对象内部的嵌套属性则不会变成响应式的

特点：对象的顶层属性是响应式的，但嵌套对象的属性不是响应式

```ts
import { shallowReactive } from "vue";
const myObj = shallowReactive({ ... });
```

## 总结

通过使用 [`shallowRef()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 和 [`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 来绕开深度响应

浅层式 `API` 创建的状态只在其顶层是响应式的，对所有深层的对象不会做任何处理，避免了对每一个内部属性做响应式所带来的性能成本，这使得属性的访问变得更快，可提升性能

# readonly 与 shallowReadonly

## readonly

用于创建一个对象的深只读副本

特点：

- 对象的所有嵌套属性都将变为只读
- 任何尝试修改这个对象的操作都会被阻止（在开发模式下，还会在控制台中发出警告）

应用场景：

- 创建不可变的状态快照
- 保护全局状态或配置不被修改

```ts
import { readonly } from "vue";
const original = reactive({ ... });
const readOnlyCopy = readonly(original);
```

## shallowReadonly

与 `readonly` 类似，但只作用于对象的顶层属性

特点：

- 只将对象的顶层属性设置为只读，对象内部的嵌套属性仍然是可变的

- 适用于只需保护对象顶层属性的场景

```ts
import { shallowReadonly } from "vue";
const original = reactive({ ... });
const shallowReadOnlyCopy = shallowReadonly(original);
```

# toRaw 与 markRaw

## toRaw

用于获取一个响应式对象的原始对象，`toRaw` 返回的对象不再是响应式的，不会触发视图更新

> :warning: 官网描述：这是一个可以用于临时读取而不引起代理访问/跟踪开销，或是写入而不触发更改的特殊方法。不建议保存对原始对象的持久引用，请谨慎使用

> 何时使用？ —— 在需要将响应式对象传递给非 `Vue` 的库或外部系统时，使用 `toRaw` 可以确保它们收到的是普通对象

+++success 示例

```ts
import { reactive, toRaw, markRaw, isReactive } from "vue";

// 响应式对象
let person = reactive({ name: "tony", age: 18 });

// 原始对象
let rawPerson = toRaw(person);
```

+++

## markRaw

标记一个对象，使其**永远不会**变成响应式的

> 例如：使用 `mockjs` 时，为了防止误把 mockjs 变为响应式对象，可以使用 `markRaw` 去标记 mockjs

```ts
let citys = markRaw([
  { id: "1", name: "北京" },
  { id: "2", name: "上海" },
  { id: "3", name: "天津" },
  { id: "4", name: "重庆" }
]);
// 根据原始对象citys去创建响应式对象citys2 —— 创建失败，因为citys被markRaw标记了
let citys2 = reactive(citys);
console.log(isReactive(person));
console.log(isReactive(rawPerson));
console.log(isReactive(citys));
console.log(isReactive(citys2));
```

# customRef

创建一个自定义的 `ref`，并对其依赖项跟踪和更新触发进行逻辑控制

+++success 示例：实现防抖效果

useSumRef.ts

```ts
import { customRef } from "vue";

export default function (initValue: string, delay: number) {
  // 使用customRef定义响应式数据
  let timer: number;
  let msg = customRef((track, trigger) => {
    // track: 跟踪 trigger: 触发
    return {
      // get调用？—— msg被读取时
      get() {
        track(); // 告诉vue：数据msg很重要，进行持续关注，一旦数据变化就去更新
        return initValue;
      },
      // set调用？—— msg被修改时
      set(value) {
        clearTimeout(timer);
        timer = setTimeout(() => {
          initValue = value;
          trigger(); // 通知vue数据变化了
        }, delay);
      }
    };
  });

  return {
    msg
  };
}
```

组件中使用

```html
<template>
  <div class="content">
    <h1>customRef 用法</h1>
    <div>1秒后更新数据：{{ msg }}</div>
    <input type="text" v-model="msg" />
  </div>
</template>

<script setup lang="ts" name="CustomRef">
  import useMsgRef from "@/hooks/useMsgRef";

  // 默认ref定义响应式数据
  // let msg = ref("你好");

  // 使用useMsgRef定义响应式数据且有延迟效果
  let { msg } = useMsgRef("你好", 1000);
</script>
```
