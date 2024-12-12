---
title: js-cookie
date: 2024-12-12 11:49:31
category:
  - [前端, 依赖插件]
tags: tool
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/5-1.jpg
---

# 安装

js-cookie 是一个简单的，轻量级的处理 cookies 的 js API，用来处理 cookie 相关的插件

```sh
npm install js-cookie --save
```

# 使用

```js
import Cookie from "js-cookie";
```

# 创建

```js
// 创建简单的 cookie
Cookie.set("属性名", "值");

// 创建有效期为 7 天的 cookie
Cookie.set("属性名", "值", { expires: 7 });

// 为当前页创建有效期 7 天的 cookie
Cookie.set("属性名", "值", { expires: 7, path: "" });
```

# 读取

```js
Cookie.get("属性名");

Cookie.get("nothing"); // => undefined

// 获取所有 cookie
Cookie.get();
```

# 删除

```js
Cookie.remove("属性名");

// 如果值设置了路径，需要指定路径
Cookies.remove("属性名", { path: "" });

// 注意：删除不存在的 cookie 不会报错也不会有返回
```

# 命名冲突

如果存在可能存在命名冲突的 cookie 名，`noConflict()` 方法将使我们可以使用我们需要的命名且保留原本冲突的名字

```js
var Cookie2 = Cookies.noConflict();
Cookie2.set("name", "value");
```

# JSON

js-cookie 提供了简易的 JSON 使用。当我们需要创建 cookie 时，我们可以使用 `Array 或 Object`，而不是只能使用 `String`。当使用 `Array 或 Object` 后，可以通过 `JSON.stringify()` 来获取到对应的对象

```js
Cookie.set("name", { foo: "bar" });

// 当使用 Cookies.get() 读取 cookie 时，获取到的是 String
Cookie.get("name"); // => '{"foo":"bar"}'
Cookie.get(); // => { name: '{"foo":"bar"}' }

// 使用 getJSON()，会自动对 String 进行 JSON.parse() 操作
Cookie.getJSON("name"); // => { foo: 'bar' }
Cookie.getJSON(); // => { name: { foo: 'bar' } }
```

# set 的属性

- expires

  定义有效期，如果传入 Number，那么单位为天，你也可以传入一个 Date 对象，表示有效期至 Date 指定时间

  默认情况下 cookie 有效期截止至用户退出浏览器

- path

  string，表示此 cookie 对哪个地址可见

  默认为 "/"

- domain

  string，表示此 cookie 对哪个域名可见。设置后 cookie 会对所有子域名可见

  默认为对创建此 cookie 的域名和子域名可见

- secure

  true 或 false，表示 cookie 传输是否仅支持 https

  默认为不要求协议必须为 https
