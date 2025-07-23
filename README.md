## 运行

```bash
hexo server
```

## 主题依赖安装

```bash
npm i hexo-renderer-multi-markdown-it --save
npm install hexo-autoprefixer --save
npm install hexo-algoliasearch --save
npm install hexo-symbols-count-time
npm install hexo-feed --save-dev
```

## 部署

`_config.yml 文件` 修改

```bash
# 有域名时部署在vercel
url: https://www.zhudaidai.top
root: /

# 部署
直接提交代码到github
```

```bash
# 没有域名时部署在github page
url: https://zhudaidai416.github.io/dai-blog
root: # 部署在 vercel 要写root: /，部署在 github page 可删除

# 部署
hexo clean && hexo deploy
```
