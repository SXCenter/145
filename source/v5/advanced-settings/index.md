---
layout: docs
group: docs-volantis-latest
order: 601
title: 进阶设定
short_title: 6. 进阶设定
sidebar: [docs-volantis-latest, toc]
disqus:
  path: /
---


## 设置子模块

{% folding yellow open, 将主题添加为子模块 %}

{% note warning:: 开始前需要确认不存在 themes/volantis 文件夹, 如果有, 请自行删除. %}

{% tabs 子模块 %}

<!-- tab ssh -->

```sh
git submodule add git@github.com:volantis-x/hexo-theme-volantis.git themes/volantis
```

<!-- endtab -->
<!-- tab https -->

```sh
git submodule add https://github.com/volantis-x/hexo-theme-volantis.git themes/volantis
```

<!-- endtab -->
{% endtabs %}

{% endfolding %}

## 多人协同

默认的作者信息在主题配置文件中设置：

```yaml blog/themes/volantis/_config.yml
# 文章布局
article:
  ...
  body:
    ...
    meta_library:
      author:
        avatar:
        name: 请设置文章作者
        url: /
```

Volantis 支持多个作者在一个站点发布文章，其他作者信息需要写在数据文件中，例如：

```yaml blog/source/_data/author.yml
Jon:
  name: Jon Snow
  avatar: https://cn.bing.com/th?id=AMMS_fc8f99fd41ebd737a71c4e13806db9a0&w=110&h=110&c=7&rs=1&qlt=80&pcl=f9f9f9&cdv=1&dpr=2&pid=16.1
  url: https://gameofthrones.fandom.com/wiki/Jon_Snow
Dany:
  name: Daenerys Targaryen
  avatar: https://tse1-mm.cn.bing.net/th?id=OIP.Yax4wLzIFbcBVUa_RsKywQHaLH&w=80&h=80&c=8&rs=1&qlt=90&dpr=2&pid=3.1&rm=2
  url: https://gameofthrones.fandom.com/wiki/Daenerys_Targaryen
```

在文章的 front-matter 中新增 `author` 即可：

```md
---
title: Jon Snow | Game of Thrones Wiki | Fandom
author: Jon
---
```

## 内容安全策略(CSP)

```yaml blog/_config.volantis.yml
# 内容安全策略( CSP ) meta 标签 http-equiv="Content-Security-Policy"
# https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP
# https://content-security-policy.com/
# 也可以设为 false 在 HTTP 标头中设置 https://volantis.js.org/v5/advanced-settings/#设置-HTTP-响应标头
csp:
  enable: true
  content: "
    default-src 'self' https:;
    block-all-mixed-content;
    base-uri 'self' https:;
    form-action 'self' https:;
    worker-src 'self' https:;
    connect-src 'self' https: *;
    img-src 'self' data: https: *;
    media-src 'self' https: *;
    font-src 'self' data: https: *;
    frame-src 'self' https: *;
    manifest-src 'self' https: *;
    child-src https:;
    script-src 'self' https: 'unsafe-inline' *;
    style-src 'self' https: 'unsafe-inline' *;
  "
  # 可以使用自动程序替换默认的 'unsafe-inline' 和 * 生成更严格的内容安全策略.
  # 另可以参考官网的 gulp 方案.
  # gulpfile.js https://github.com/volantis-x/community/blob/main/gulpfile.js
```


## 设置 HTTP 响应标头

以 [Cloudflare](https://developers.cloudflare.com/rules/transform/response-header-modification/create-dashboard/) 为例， 在 规则 > 转换规则 > HTTP 响应头修改 中，可以添加以下设置：

- 内容安全策略( CSP )

```
Content-Security-Policy: default-src 'self' https:; block-all-mixed-content; base-uri 'self' https:; form-action 'self' https:; worker-src 'self' https:; connect-src 'self' https: *; img-src 'self' data: https: *; media-src 'self' https: *; font-src 'self' data: https: *; frame-src 'self' https: *; manifest-src 'self' https: *; child-src https:; script-src 'self' https: 'unsafe-inline' *; style-src 'self' https: 'unsafe-inline' *;
```

[Doc for Content-Security-Policy](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)

- 不允许在 frame 中展示

```
x-frame-options: DENY
```

[Doc for X-Frame-Options](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options)


## 为网站提速

### 加载速度

- 减少不必要的 js 插件，例如字数统计、动态背景。

- 查找并解决拖慢速度的资源，以 Chrome 浏览器为例：
  1. 页面中点击右键，选择「检查」。
  2. 在右边的窗口中「Network」选项卡，并勾选「Disable cache」。
  3. 刷新网页，查看加载速度慢的资源。
    3.1. 加载缓慢的图片，建议使用 CDN。
    3.2. 加载缓慢的可以不用的 js 插件，建议舍弃。
    3.3. 加载缓慢却必须使用的 js 插件，建议下载并自己上传至 CDN。

### 运行速度

- 访问具有动态特效背景（如雪花、粒子等）的网站时，手机很快会发烫变卡，笔记本很快会风扇狂转并且浏览器提示建议关闭此页面。如果你希望网站有好的使用体验请尽量不要安装这类插件。


## 优化 SEO

### Robots

```yaml blog/_config.volantis.yml
seo:
  # When there are no keywords in the article's front-matter, use tags as keywords.
  use_tags_as_keywords: true
  # When there is no description in the article's front-matter, use excerpt as the description.
  use_excerpt_as_description: true
  robots:
    home_first_page: index,follow
    home_other_pages: noindex,follow
    archive: noindex,follow
    category: noindex,follow
    tag: noindex,follow
    # robots can be written in front-matter
```

在 front-matter 中，可以设置 `keywords`、`description`、`robots` 和 `seo_title`。其中 `seo_title` 仅仅用作网页标题，优先级高于 `title`。

- 文章内部不要使用 H1 标题。

- 通过死链检测工具检查并删除无法访问的链接。

- 安装 SEO 优化插件：
  {% link hexo-autonofollow, https://github.com/liuzc/hexo-autonofollow %}
  {% link hexo-generator-seo-friendly-sitemap, https://github.com/ludoviclefevre/hexo-generator-seo-friendly-sitemap %}

- 页面不要堆砌关键词，不要频繁更改路径。

### Open Graph

```yaml blog/_config.volantis.yml
# https://ogp.me/
# https://hexo.io/zh-cn/docs/helpers#open-graph
open_graph:
  image: https://cdn.jsdelivr.net/gh/volantis-x/cdn-org/blog/favicon/android-chrome-192x192.png
  twitter_card: summary # summary_large_image , summary
  #twitter_id:
  #twitter_site:
```

### Structured Data

```yaml blog/_config.volantis.yml
# SEO 入门文档: https://developers.google.com/search/docs
# https://schema.org.cn/
# 结构化数据用于更改搜索结果的显示效果
# 目前内置的结构化数据: blogposting, breadcrumblist, organization, person, website
# 目前内置的富媒体搜索结果: 路径(面包屑导航), 徽标(Logo), 站点链接搜索框(SearchAction)
# https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data
# 富媒体搜索结果测试: https://search.google.com/test/rich-results
structured_data:
  enable: true
  # 以下是覆盖配置, 默认配置见 scripts/helpers/structured-data/lib/config.js
  data:
    person:
      sns:
        - https://github.com/volantis-x
    logo:
      path: https://cdn.jsdelivr.net/gh/volantis-x/cdn-org/blog/favicon/android-chrome-192x192.png
      width: 192
      height: 192
```

## 使用 CDN

对于大部分将博客 deploy 到 GitHub 的用户来说，直接加载本地资源速度比较慢，可以使用 jsdelivr 为开源项目提供的 CDN 服务。

### 开启方法

```yaml blog/_config.volantis.yml
# 本地静态文件使用 CDN 加速
# 默认使用 https://unpkg.com/hexo-theme-volantis@<%- theme.info.theme_version %>/source/js/*.js ，注意版本号对应关系！！可以通过修改以下配置项覆盖
# 开发者注意 cdn.enable 设置为 false
cdn:
  enable: true
  # CDN 前缀，为空使用默认值，链接最后不加 "/",
  # 例如： https://cdn.jsdelivr.net/gh/volantis-x/volantis-x.github.io@gh-page 填写最后编译生成的源码CDN地址前缀，此路径下应该含有/js与/css目录,
  # 该配置默认值是："https://unpkg.com/hexo-theme-volantis@"+ theme.info.theme_version +"/source"
  prefix: #https://unpkg.com/hexo-theme-volantis/source
  # 以下配置可以覆盖 cdn.prefix,配置项的值可以为空，但是要使用CDN必须依据路径填写配置项的键
  set:
    js:
      app: /js/app.js
    css:
      style: /css/style.css # (异步加载样式)
# 静态资源版本控制
# 本地文件使用文件内容的hash值作为版本号(app.8c1e7c88.js)  其他为时间戳 (?time=1648684470140)
# 建议静态资源设置标头浏览器缓存一年边缘缓存一个月 cache-control: max-age=86400, s-maxage=31536000 如果有更新记得刷新缓存
cdn_version: true
# volantis static 静态资源文件 npm 包 CDN 地址 (后面加 "/" )
# https://github.com/volantis-x/volantis-static
volantis_static_cdn: https://unpkg.com/volantis-static/
```

{% note info, 如果你需要对样式进行 DIY，可以只关闭 style 文件的 CDN。 %}

从V5版本开始，首屏样式采用硬编码的方式写在HTML中。首屏样式内含 cover navbar search 的样式，其他样式放入/css/style.css异步加载。

{% note warning up, 如果你需要对样式进行 DIY，请注意首屏渲染和异步延迟加载的差异。 %}


### 自定义 CDN

如果你把对应的文件上传到自己的 CDN 服务器，可以把对应的链接改为自己的 CDN 链接。


## 尝试使用 gulp 压缩静态文件

### 安装压缩工具

```shell
npm install -g gulp
npm install --save-dev gulp gulp-html-minifier-terser gulp-htmlclean gulp-htmlmin gulp-clean-css gulp-terser gulp-sourcemaps
```

### gulp 配置文件

参考文档： [gulp](https://github.com/gulpjs/gulp) [gulp-html-minifier-terser](https://github.com/pioug/gulp-html-minifier-terser) [gulp-htmlclean](https://github.com/anseki/gulp-htmlclean) [gulp-htmlmin](https://github.com/jonschlinkert/gulp-htmlmin) [gulp-clean-css](https://github.com/scniro/gulp-clean-css) [gulp-terser](https://github.com/duan602728596/gulp-terser) [gulp-sourcemaps](https://github.com/gulp-sourcemaps/gulp-sourcemaps)

{% folding green, gulp 配置文件 %}

https://github.com/volantis-x/community/blob/main/gulpfile.js

<script src="https://gist.github.com/MHuiG/4d443d3bfb10eac961a13e46023581f7.js"></script>

{% endfolding %}


### 运行 gulp

```shell
gulp
```

## 尝试使用 babel 转换兼容 ES6

如果想要兼容旧版浏览器，可以尝试使用 [gulp-babel](https://github.com/babel/gulp-babel) 将 ES6 转换为 ES5。

### 安装 gulp-babel 工具

```bash
npm install -g gulp
npm install --save-dev gulp-babel @babel/core @babel/preset-env
```

### gulp 配置文件

参考文档： [gulp](https://github.com/gulpjs/gulp) [gulp-babel](https://github.com/babel/gulp-babel)

{% folding green, gulp 配置文件 %}

https://github.com/volantis-x/community/blob/main/gulpfile.js

```js
gulp.src(['./public/**/*.js', '!./public/**/*.min.js', '!./public/{lib,lib/**}', '!./public/{libs,libs/**}', '!./public/{media,media/**}'])
  .pipe(babel({
    presets: ['@babel/preset-env']
  }))
  .pipe(gulp.dest('./public'))
```

{% endfolding %}

### 运行 gulp

```shell
gulp
```

## 安装 Service Worker 服务

### 方案一 安装插件

安装 [hexo-offline-popup](https://github.com/Colsrch/hexo-offline-popup)  或者 [hexo-offline](https://github.com/JLHwung/hexo-offline) 插件，初次加载速度不变，后期切换页面和刷新网页速度越来越快。

### 方案二 使用 workbox 自定义配置

{% folding green, step 1. 修改 blog/_config.yml 文件。 %}

```yaml blog/_config.yml
# 全局导入
import:
  body_end:
    - <script>"serviceWorker"in navigator&&navigator.serviceWorker.register("/sw.js").then(function(n){n.onupdatefound=function(){var e=n.installing;e.onstatechange=function(){switch(e.state){case"installed":navigator.serviceWorker.controller?console.log("Updated serviceWorker."):console.log("serviceWorker Sucess!");break;case"redundant":console.log("The installing service worker became redundant.")}}}}).catch(function(e){console.log("Error during service worker registration:",e)}); </script>
```
{% endfolding %}

{% folding green, step 2. 在 blog/source 中创建 sw.js 文件。 %}

https://gist.github.com/MHuiG/a423c0a953ed5645840a651c33dcd60c

<script src="https://gist.github.com/MHuiG/a423c0a953ed5645840a651c33dcd60c.js"></script>

{% endfolding %}

{% note warning up, 如果你使用了此方案，修改静态文件后发布网页一定要修改缓存版本号。 %}

### 方案三 参考官网 volantis-sw.js

[volantis-sw.js](https://github.com/volantis-x/community/blob/main/source/volantis-sw.js)

[discussions/791](https://github.com/volantis-x/hexo-theme-volantis/discussions/791)


## 安装「相关文章」插件

1. 安装插件
```sh
npm i -S hexo-related-popular-posts
```

2. 插件的自定义配置方法：
{% link hexo-related-popular-posts, https://github.com/tea3/hexo-related-popular-posts %}

如果您使用了头图，可以在站点配置文件中添加以下设置来让相关文章显示正确的文章头图：

```yaml blog/_config.yml
popularPosts:
  eyeCatchImageAttributeName: headimg
```

{% noteblock warning up, 注意 %}

需要升级到 5.0.1 及以上版本才可以支持自定义头图，详见 [#29](https://github.com/tea3/hexo-related-popular-posts/issues/29)

{% endnoteblock %}

## 分析与统计

### 数据统计

#### PV 和 UV

支持 [不蒜子](http://busuanzi.ibruce.info/) 的访问统计和 [leancloud](https://leancloud.app/) 统计，在配置文件中设置。

- 若你选择 [leancloud](https://leancloud.app/) 统计, 你还需前往 leancloud 创建应用并填写下面 leancloud 相关配置
- 若你选择 [不蒜子](http://busuanzi.ibruce.info/) 统计, 请取消下面 busuanzi 的配置注释

``` yaml blog/_config.volantis.yml
analytics:
  busuanzi: #/libs/busuanzi/js/busuanzi.pure.mini.js #https://cdn.jsdelivr.net/gh/volantis-x/cdn-busuanzi@2.3/js/busuanzi.pure.mini.js
  leancloud: # 请使用自己的 id & key 以防止数据丢失
    app_id: # 应用 APP_ID
    app_key: # 应用 APP_KEY
    custom_api_server: # 国际版一般不需要写，除非自定义了 API Server
```

我们还支持以下评论系统提供的访问统计: [waline](https://waline.js.org/)、[twikoo](https://twikoo.js.org/)、[discuss](https://discuss.js.org)、[artalk](https://artalk.js.org)

如需使用它们，请将上面 `leancloud` 和 `busuanzi` 的所有配置注释，并启用对应评论系统的统计功能

#### 字数和阅读时长

1. 安装以下插件：
```
npm i --save hexo-wordcount
```
2. 修改配置文件，将 `wordcount` 插件打开
```yaml blog/_config.volantis.yml
plugins:
  ...
  # 文章字数统计、阅读时长，开启需要安装插件: npm i --save hexo-wordcount
  wordcount:
    enable: #true
```
3. 然后修改配置文件，将 `wordcount` 写入需要显示的 meta 位置：
```yaml blog/_config.volantis.yml
# 文章布局
article:
  ...
  # 文章详情页面的文章卡片本体布局方案
  body:
    # 文章顶部信息
    # 从 meta_library 中取
    top_meta: [..., wordcount, ...]
    ...
    # 文章底部信息
    # 从 meta_library 中取
    bottom_meta: [..., wordcount, ...]
```

### 数据分析

#### 百度统计

```yaml blog/_config.yml
baidu_analytics_key: 百度统计的key
```

#### Google Analytics

```yaml blog/_config.yml
google_analytics_key: Google Analytics Key
```

#### 腾讯前端性能监控

```yaml blog/_config.yml
tencent_aegis_id: 上报 id
```

#### 51LA统计
```yaml blog/_config.yml
v6_51_la: 应用id
```

#### 灵雀监控
```yaml blog/_config.yml
perf_51_la: 应用id
```

#### CNZZ 统计

请参考 ZYMIN 的这篇教程：
{% link Hexo hexo+ejs+material x 添加CNZZ统计代码, https://zymin.cn/arcticle/hexo+ejs+material.html %}

## 更多进阶玩法

详见 [@TRHX](https://www.itrhx.com) 的这篇博客：
{% link Hexo 博客主题个性化, https://www.itbob.cn/article/003/ %}

内含卡片半透明、增加卡通人物、自定义鼠标样式、鼠标特效、烟花特效、彩色滚动字体、网站运行时间、动态浏览器标题、雪花飘落特效等多种详细教程。

{% link 主题官网 #进阶玩法, https://volantis.js.org/categories/进阶玩法/ %}
