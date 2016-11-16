title: Angular-UEditor
date: 2015-1-10 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: publish-angular-ueditor
---

angualr 作为最近前端大热的一款框架，越来越多国人开始使用并且不断有成功的项目。UEditor作为百度前端团队的一款神器，在国内多个项目也在使用。所以小编抽了个时间把angular和UEditor整合起来作为一款angular的插件。
<!--more-->

### 2015年3月19日更新 关于图片上传的问题

> 由于UEditor 对图片上传的操作时插入了一个元素（上传中...）的内容但对于后续操作UEditor都采用了对该元素直接进行操作没有触发到contentChange事件。我已经向UEditor的开发团队提交了issue，所以还是需要等他们来解决这个问题。相关细节的代码参考-> [simpleupload.js](https://github.com/fex-team/ueditor/blob/dcd825c969506cef8ebed71b324e4e9e4793b335/_src/plugins/simpleupload.js#L61-L97)

## angular-ueditor

angular-ueditor 是一款整合了 angular 和 UEditor 的插件。目的是为了更方便的在angular基础上使用UEditor。

**[Github](https://github.com/zqjimlove/angular-ueditor)地址**

## 例子

[http://zqjimlove.github.io/angular-ueditor/](http://zqjimlove.github.io/angular-ueditor/)

## 安装

先引入 UEditor 的 javascript 文件

<pre class="lang:xhtml decode:true " >&lt;script type="text/javascript" src="/ueditor/ueditor.config.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="/ueditor/ueditor.all.js"&gt;&lt;/script&gt;</pre> 

然后下载最新的 [angular-ueditor](https://github.com/zqjimlove/angular-ueditor/tree/master/dist) 并且引入文件
 
<pre class="lang:xhtml decode:true " >&lt;script type="text/javascript" src="angular-ueditor.js"&gt;&lt;/script&gt;</pre> 

此时还需要把 angular-ueditor 引入到模块里

<pre class="lang:js decode:true " >angular.module('app', ['ng.ueditor'])</pre> 

### 用 Bower 安装

使用 Bower 安装时，angular-ueditor 会自动安装并且引入，但ueditor由于没有加入到 Bower 所以需要手动引入。

<pre class="lang:sh decode:true " >bower install angular-ueditor --save</pre> 

## 使用

##### 基础用法

**由于继承了NgModelController，必须绑定 `ngModel`**

<pre class="lang:xhtml decode:true " >&lt;div class="ueditor" ng-model="content"&gt;&lt;/div&gt;</pre> 

##### 自定义编辑器

<pre class="lang:xhtml decode:true " >&lt;div class="ueditor" config="config" ng-model="content"&gt;&lt;/div&gt;
...
&lt;script&gt;
    $scope.config = {
        ...
    }
&lt;/script&gt;</pre> 

## 方法

### `ready(listener)`

注册准备事件，当 editor 初始化完成后会执行回调函数。

##### 参数

参数      |类型             |详细
----------|----------------|----
listener  |function(editor)|Callback called whenever the editor ready.

##### 例子

<pre class="lang:xhtml decode:true " >&lt;div class="ueditor" ready="ready" ng-model="content"&gt;&lt;/div&gt;
...
&lt;script type="text/javascript"&gt;
    $scope.ready = function(editor){
        alert(editor.getContent());
    }
&lt;/script&gt;</pre> 

## 关于

这算是小编第一次正正经经的发布一个开源的项目，即使项目技术含量不高但实在又难以言表的愉悦。在最初开始学习编程到今天，使用过的开源项目数不胜数一直在学习、膜拜各路大神级的开源人物，这一次总算小小回馈了一下。希望在未来能带给大家更多方便好用的项目。



