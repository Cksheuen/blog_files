---
title: It's about why and how I build up this blog.
date: '2023-11-5'
---



# 踩坑记录

## ——`nuxt`真是太神奇辣

### 首先是nuxt自带的fetch系列composable

没有找到实际证据，但是实操中的bug以及nuxt作者之一**[Anthony Fu]([Anthony Fu (antfu.me)](https://antfu.me/))**开发的另一款插件**[nuxt-server-fn](https://github.com/antfu/nuxt-server-fn)**中的cache的使用，使得我有理由推断这里的fetch不能在同一个setup中不能被多次调用（如果fetch本来就不可以那就当我没说）。

然后是似乎nuxt发fetch最好都用完整的方式接收，如

```js
const { data, pending, error, refresh } = await useFetch('/api/posts/postDirs', {
  method: 'POST',
  body: JSON.stringify({
    path,
  }),
})
```

不然似乎会出现刷新后无法获取数据的bug（？

### fu神的nuxt-server-fn

按照[github]([antfu/nuxt-server-fn: Server functions in client for Nuxt 3 (github.com)](https://github.com/antfu/nuxt-server-fn))上的usage的步骤似乎会报错（？），介于当时卡同一个坑太久没有深挖原因。

### 关于nuxt在vercel上的部署

介于next才是vercel的亲儿子这点，nuxt在vercel的部署并不像next那般顺利，甚至可以说是麻烦（后端读不到public的本地文件一定算是麻烦了吧！）。

总之就是nuxt在vercel上部署后是前后端分开的，而nuxt会默认把前端文件直接打包到和public中的各种静态资源一起，所以本地读取blog文件的方式就会失效。

最终选择的解决方法是把文件放到**[github page]([GitHub Pages | Websites for you and your projects, hosted directly from your GitHub repository. Just edit, push, and your changes are live.](https://pages.github.com/))**上（虽然github在中国的访问并不流畅，但只能说还可以，甚至blog上访问的更快？不排除因为vercel也在外网然后带着资源域名解析后更加适宜中国了）。但是还是存在一些弊端，比方不能读取文件的创建时间来作为日期。

### 然后是关于pnpm的版本问题

说起来你可能不信，但我当时好像被这问题耽搁了一天（？），当时是在本地基本实现了展示blog的功能，所以想上传vercel看看效果找找bug，然后就发现build过程中存在问题，显示在`.pnpm`的文件夹下，最后是更新了pnpm的版本。

