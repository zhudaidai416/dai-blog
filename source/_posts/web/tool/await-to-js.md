---
title: await-to-js
date: 2024-12-12 13:28:05
category:
  - [前端, 依赖插件]
tags: tool
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/5-3.jpg
---

# 参考

{% links %}

- site: 深入 await-to-js 源码
  owner: 小黄瓜没有刺
  url: https://juejin.cn/post/7119253480170061855
  color: "#b7d28d"

{% endlinks %}

# [安装](https://www.npmjs.com/package/await-to-js)

异步等待封装器，便于错误处理，不需要 `try-catch`

```sh
npm i await-to-js --save
```

# 对比 async

```js
// async的处理方式
function async getData() {
  try {
    const data1 = await fn1()
  } catch(error) {
    return new Error(error)
  }
  try {
    const data2 = await fn2()
  } catch(error) {
    return new Error(error)
  }
  try {
    const data3 = await fn3()
  } catch(error) {
    return new Error(error)
  }
}
```

# 使用

```js
// 引入
import to from 'await-to-js';

function async getData() {
  const [err, data1]  = await to(promise)
  if(err) throw new (error);

  const [err, data2]  = await to(promise)
  if(err) throw new (error);

  const [err, data3]  = await to(promise)
  if(err) throw new (error);
}
```
