---
title: Hexo + Github Pages 博客搭建教程（五）：问题解答
date: 2019-03-19 17:01:09
tags:
    - "搭建博客"
    - "教程"
categories:
    - "搭建博客"
---
![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/f32be74b5abb4a05b57a37a2141e80a4.jpeg)

## 百度和Google收录
使用 `GitHub + Hexo` 搭建的博客，默认只能你自己能看到，别人是无法通过百度、谷歌等搜索引擎搜索到的:

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553065501429.jpg)

可以手动将自己的博客站点提交给百度、谷歌的搜索引擎，这样就可以通过百度或谷歌搜索到自己的博客内容了：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553086521591.jpg)

### 百度收录
#### 验证站点
登录[百度搜索资源平台](https://ziyuan.baidu.com)>用户中心>站点管理>添加网站，输入网站域名，选择站点属性，到第三步“验证网站”：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553082150167.jpg)

有三种不同的验证方式：文件验证、HTML标签验证、CNAME验证。这里我们选择文件验证，下载验证文件到本地，放置在 `themes/next/source`目录下，执行生成和部署命令：

```
$ hexo g -d
```

然后点击完成验证即可：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553065459196.jpg)

#### 添加站点地图
站点地图（sitemap）可以告诉搜索引擎网站上有哪些可供抓取的网页，以便搜索引擎可以更加智能地抓取网站。

- 安装百度和谷歌站点地图生成插件：cd 到你的站点目录，执行以下命令

```
$ npm install hexo-generator-baidu-sitemap --save
$ npm install hexo-generator-sitemap --save
```

- 修改 url：修改站点配置文件 `_config.yml` 中的 `url` 参数:

```
url: http://jonzzs.cn # 修改成你博客的首页地址
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```

- 修改配置文件：修改站点配置文件 `_config.yml`，添加以下内容：

```
# 自动生成sitemap
sitemap:
  path: sitemap.xml
baidusitemap:
  path: baidusitemap.xml
```

- 生成和部署：执行生成和部署命令后，进入public目录，你会发现里面有 `sitemap.xml` 和`baidusitemap.xml` 两个文件，这就是生成的站点地图，前者用来提交给谷歌，后者用来提交给百度

```
$ hexo g -d
```

- 自动推送站点地图：在百度资源搜索平台，找到链接提交，这里我们可以看到有两种提交方式，自动提交和手动提交，自动提交又分为主动推送、自动推送和sitemap，自动推送配置最简单，将主题配置文件下的 `baidu_push` 设置为 `true`：

```
# Enable baidu push so that the blog will push the url to baidu automatically which is very helpful for SEO
baidu_push: true
```

百度收录网站到此配置结束，只需要等待百度收录，这个过程会比较久。

### 谷歌收录
#### 验证站点
登陆[Google网站站长](https://www.google.com/webmasters/#?modal_active=none) > 进入Search Console > 添加资源：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553085549734.jpg)

我们选择 HTML 文件上传的方式验证，下载验证文件到本地，放置在 `themes/next/source`目录下，执行生成和部署命令：

```
$ hexo g -d
```
部署完成之后，进行验证即可：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553084916927.jpg)

#### 添加站点地图
安装插件、修改配置文件在上述百度收录过程已经做过了，现在只需要点击前往站点资源页面 > 点击站点地图，添加新的站点地图 `sitemap.xml`：

![](https://likeitea-1257692904.cos.ap-guangzhou.myqcloud.com/liketea_blog/1553085912959.jpg)

即可完成谷歌收录网站，只需要等待谷歌收录，这个过程会比较久。



## 参考

1. [Hexo博客提交百度和Google收录](https://www.jianshu.com/p/f8ec422ebd52)
2. [百度资源平台](https://ziyuan.baidu.com)
3. [Google网站站长](https://www.google.com/webmasters/#?modal_active=none)