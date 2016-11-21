title: 迁移至Hexo，并且开源hexo-cdnify
date: 2016-11-18 12:16:32
tags:
- NodeJS
categories:
- Joke
permalink: blog-with-hexo-and-publish-hexo-cdnify
---
从最初第一次使用Wordpress搭建博客的时候便深深喜欢这套开源的博客系统，更是有一段时间在研究Wordpress的API文档也曾经做了几套主题。越到往后WP的系统功能更加丰富但同时也十分耗服务器的资源和运行相对较慢。所以在今年一直寻思换一个更加轻量的博客，专注于写东西和分享。

<!--more-->

# 选择使用Hexo

其实使用Hexo作为新的博客程序主要有两大原因：

* 绝对的轻量化。发布文件为纯静态、配置都通过配置文件生成、无撰写后台以markdown文件作为承载。
* 以NodeJS作为开发语言。主要由于熟识JavaScript，对于开发、改造等都十分方便快捷。

## 安装

hexo 的安装相当简单，一切都是基于`npm install`实现安装，如果本身对NodeJS熟识的话基本无需多费神。第一步是需要安装`hexo-cli`，然后创建项目再初始化即可完成。

```sh
npm install hexo-cli -g
hexo init blog
cd blog
```

> 其实在初始化的时候已经一同进行了`npm install`，所以官方教程中的这一步可以忽略。

## 部署

配置文件的修改、新建文章等其实官方都已经有中文文档而且十分详尽在此我也不重复了。其中部署这一块大多数人选择了部署到Github Pages上，由于阿里云还有一年才到期所以我选择了同时部署到两个不同的地方上。

### 部署到 Github Pages

> 部署到Github Pages需要注意的是在创建repo的时候repo的名字很重要。
> `${username}.github.io` 的项目名字，生成Github pages的时候是以`master`分支作为Source的。而其他名称则以`gh-pages`分支作为Source的。所以在创建仓库的时候必须注意这两点。

[部署教程][1]中其实已经很详细的介绍了所有方式，包括git、ftp、rsync等方式。但需要主要的是由于Github Pages的一个改动使得不再支持 `/vendors/*` 下的所有静态文件，会导致大部分的样式、JS文件都会出现404 Not Found错误。解决办法也相当简单就是在 source 文件夹下创建一个 `.nojekyll`的空文件即可。

`touch source/.nojekyll`

### 部署到阿里云

部署到阿里云可能有些人会选择使用FTPsync的方式，但是由于我也把源代码托管在Github上也就是说我的配置文件是“开源”的，所以FTP的密码就很容易暴露无遗。所以同步到阿里云我使用的方式是rsync。

首先在阿里云设置ssh免登录的方式免去输入密码，然后安装rsync即可。这样一来可以实时同步也可以免去密码被暴露的风险。

# 开源插件 hexo-cdnify

这个插件其实就是在编译过程中把本地的所有静态文件指向cdn上。

`<img src="/uploads/hi.jpg" /> ` 编译后 `<img src="//cdn/uploads/hi.jpg" /> `

## 初衷

在使用WP的时候便已经习惯把一些图片、CSS文件、JS文件都存放到七牛云上，所以很习惯的在搭建完hexo之后就是必须利用cdn进行优化，但是hexo的七牛插件需要一堆的配置并且需要密钥，作为利用github托管源的最不想看到的就是需要把一些敏感信息填写到配置文件上，所以决定自己动手完成这个插件。

## 使用

由于这个并非主动上传，只是利用了七牛的镜像功能所以需要在七牛上设置镜像功能和准确的路径。

`npm install hexo-cdnify --save`，安装完之后修改 hexo 的配置文件，添加如下配置：

```yaml
cdn:
  enable: true
  base: //cdn.com
  tags:
    'img[data-orign]()':  data-orign
```

`base` CDN的域名或者镜像链接。

`tags` 需要替换的标签和元素的Map对象。`'img[data-orign]':  data-orign`即：寻找含`data-orign`属性的`img`标签，并且替换其`data-orign`属性的地址为CDN。`tags`默认已支持下列元素和属性：

```html
<img data-src="____"\>
<img src="____"\>
<link rel="apple-touch-icon" href="____"\>
<link rel="icon" href="____"\>
<link rel="shortcut icon" href="____"\>
<link rel="stylesheet" href="____"\>
<script src="____"\>\</script\>
<source src="____"\>\</source\>
```

### 发布

由于在生成静态文件之前不需要替换CDN，所以只有在`hexo g`活着`hexo d -g`时候才会进行替换，而`hexo s` 或`hexo server` 都是会忽略掉的。

[1]:	https://hexo.io/docs/deployment.html "hexo部署教程"
