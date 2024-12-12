---
title: vue-count-to
date: 2024-12-12 13:25:52
category:
  - [前端, 依赖插件]
tags: tool
cover: https://daiblog.oss-cn-chengdu.aliyuncs.com/cover/5-2.jpg
---

# 安装

```sh
npm install vue-count-to
```

# 使用

```html
<count-to :start-val="0" // 起始值 :end-val="228" // 终点值 :duration="1000" //
动画时间（毫秒） />

<script>
  import CountTo from "vue-count-to";
  export default {
    components: {
      CountTo
    }
  };
</script>
```
