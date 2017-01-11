title: 混合应用的JavaScript Bridge（二）
date: 2016-1-25 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: hybrid-app-javascript-bridge-2
---
通过<a href="http://inhu.net/hybrid-app-javascript-bridge-1.html" title="混合应用的JavaScript Bridge（一）">《混合应用的JavaScript Bridge（一）》</a>大家应该对 IOS 和 Android 平台如何实现与 JavaScript 通讯有了简单的了解，并且也说到了两平台之间的差异。既然我们要实现一个跨平台的、统一的 JSSDK 就需要把它们之间的差异抹平，实现接口统一。

<!--more-->

<h2>统一接口</h2>

首先第一步要做的事我们必须先规定好对外的接口，也就是我们所说的API。我们需要定义两个简单的接口：<strong>发送、接收</strong>。发送，就是从JS发送到Native上，接收则反之。

<pre class="decode:true lang:javascript"><code>window.inhuBridge = {
    /**
     * 发送信息到Native
     * 
     * @param  {String} api  原生的api
     * @param  {Object} params 调用参数
     * @param {Function} successCallback 回调
     * @param {Function} failCallback 失败的回调
     */
    send:function(api,params,successCallback,failCallback){

    },
    /**
     * 接收信息从Native
     * 
     * @param  {Object} respond 调用原生api后返回的对象
     * @param {Number} respond.status 0/1，调用成功或失败。
     * @param {String} respond.message 失败的信息
     * @param {String} respond.callbackId 会话ID
     * @param {Object} respond.data 成功后返回的数据。
     */
    receive:function(respond){

    }
}
</code></pre>

<h2>原生API</h2>

从以上的代码，我们定义了最简单最直白最明了的对外接口。当然仅仅靠这些注释还是会模糊不清，特别是发送信息中的<code>api</code>更不知从何而来。

首先我们需要搞清楚的是，实现这个bridge就是为了能让webview获得由原生支持的功能，例如调起分享的功能、录制音频视频等，所以我们不仅仅需要在<strong>JavaScript上面做接口定义，我们还需要在原生部分实现我们想要的功能，如分享到xx</strong>。

原生的api我们简单地用以下字符串的方式作为定义：

<pre><code>Module[:Channel]:Action

// share:weibo:config  配置微博分享内容
// share:showMenu 显示分享菜单
</code></pre>

<em>当然，如果需要更复杂的业务也可以通过自己定义相关格式。</em>

<h2>CallbackId</h2>

接下来我们要说的是回调。当成功请求了原生里预先定以好的接口后会应答会得到一个响应，如分享是否成功、地理位置的数据等，但由于JavaScript和原生相互调用都是通过注入、修改URL等方式，无论是this还是caller都是没办法和调用的方法关联起来也就是说没办法获取到调用时设置好的回调方法。

那唯一的办法就只剩下使用唯一标识的方法，<strong>callbackId</strong>就是唯一的标识。当我们调用原生方法的时候我们先生成一个 callbackId 然后，将回调方法一callbackId作为key缓存到一个对象中，然后连同 callbackId 作为参数之一调用原生方法，当原生方法需要反馈到JavaScript的时候把callbackId也传回去，当JavaScript接到一个从原生返回的反馈的时候通过callbackId调起相对应的回调方法。

<h2>生成callbackId</h2>

生成callbackId其中必须保证每一个callbackId都只是对应一次的调用而且是唯一的，由于文章篇幅的关系什么uuid、guid的就不考虑了，以最简单的递增方式来说清楚就好了

<pre class="decode:true lang:javascript"><code>var gCallbackId = (function(){
    var _id = 1;
    return function(){
        return _id++;
    }
})()
</code></pre>

上面使用了闭包是为了防止<code>_id</code>受外部的脏代码影响。关于闭包的知识点大家可以从网上搜索一下。

这一篇就大致说了我们先要规范好、统一好接口，那接下来我们说说Android的实现，最后再说IOS的实现方式。