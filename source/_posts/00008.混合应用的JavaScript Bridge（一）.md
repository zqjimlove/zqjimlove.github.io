title: 混合应用的JavaScript Bridge（一）
date: 2016-1-15 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: hybrid-app-javascript-bridge-1
---
在移动互联快速发展的今天，越来越多的应用采用Hybrid（混合模式）的开发方式。所谓的混合模式就是采用原生应用和网页应用相结合，我们熟知的<strong>手机淘宝</strong>就是采用了混合模式的应用，而<strong>微信甚至提供了JSSDK</strong>作为对公众号开发WEB应用的支持和扩展。

混合模式开发最大的好处就是能快速迭代并且跨平台。快速迭代是因为所有视图和业务层面的代码都将由WEB去进行处理，对于节日活动、更换风格、修改逻辑等非核心功能的修改只需更新WEB的线上代码而无需对原生代码进行修改，这样的方式无需用户对手机上的应用进行下载更新便可达到线上更新。跨平台也是显而易见的优势，主流移动设备主要是IOS和Android两大平台，对HTML5都有着很好的支持。

那今天主要以前端的视角来带大家看一下如何从零开始实现原生应用和JavaScript的通讯。

<!--more-->

<h2>平台之分</h2>

当今主流的移动设备平台主要分为 <em>IOS</em> 和 <em>Android</em>（WinPhone市场占有率低到只能忽略），由于它们之间的架构差异导致了对 WebView 进行通讯的方式也不一样，而我们需要实现的是一个跨平台的通用的“接口”，所以在开始之前我们先要梳理一下它们平台之间的差异化。

<h4>Android</h4>

Android 平台下可对 WebView 添加 JavaScript 对象，在创建一个WebView 之后可调用 [addJavascriptInterface](http://developer.android.com/reference/android/webkit/WebView.html#addJavascriptInterface(java.lang.Object, java.lang.String)) 插入一个 JavaScript 的对象并且绑定到原生的对象上。

<blockquote>
  Injects the supplied Java object into this WebView. The object is injected into the JavaScript context of the main frame, using the supplied name. This allows the Java object's methods to be accessed from JavaScript.
</blockquote>

<pre class="decode:true lang:java"><code> class JsObject {
    @JavascriptInterface
    public String toString() { return "injectedObject"; }
 }
 webView.addJavascriptInterface(new JsObject(), "injectedObject");
 webView.loadData("", "text/html", null);
 webView.loadUrl("javascript:alert(injectedObject.toString())");
</code></pre>

上面的官方例子中将 Java 对象 JsObject 通过 <code>addJavascriptInterface (Object object, String name)</code> 的方法以 <code>injectedObject</code> 的名字插入到 WebView 的 JavaScript 上下文中，即 <code>window.injectedObject</code>。

Android 提供了 <code>addJavascriptInterface</code> 方法，显然对于 JavaScript 发送信息到原生应用上实现起来就更简单了。那么原生应用发送信息到 JavaScript 则通过更改url实现。

<pre class="decode:true lang:java"><code><br />webView.loadUrl("javascript:alert(injectedObject.toString())");
</code></pre>

<h4>IOS</h4>

IOS 相比 Android 则显得没那么友好，需要通过对 <strong>UrlScheme</strong> 监听以达到获取心的消息（来自WebView）通知的效果。

原生应用设置好监听的 UrlScheme 后，当 WebView 需要发送消息的时候则请求访问约定的链接，原生应用利用<code>stringByEvaluatingJavaScriptFromString</code>或者<code>evaluateJavascript</code>获取详细信息并且进行处理。<strong>IOS平台与Android平台实现通讯的最大区别在于，Android 被动接收由 JavaScript 推送过来的内容，但 IOS 上则是通过监听由 WebView 发起一个指定URL的请求后，主动执行 JavaScript 的方法并且获取返回值</strong>。

需要注意的是 UIWebView 使用的是<a href="https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/#//apple_ref/occ/instm/UIWebView/stringByEvaluatingJavaScriptFromString:">stringByEvaluatingJavaScriptFromString</a>，而 WKWebView 使用的是<a href="https://developer.apple.com/library/ios/documentation/WebKit/Reference/WKWebView_Ref/index.html#//apple_ref/occ/instm/WKWebView/evaluateJavaScript:completionHandler:">evaluateJavascript</a>。

<pre class="decode:true lang:object-c"><code>(void)evaluateJavaScript:(NSString *)javaScriptString
         completionHandler:(void (^)(id,
                                     NSError *error))completionHandler

- (NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script
</code></pre>

<em>其中官方文档中说明IOS8之后WKWebView将会替代UIWebView.</em>

<h2>总结</h2>

通过对以上两个平台如何实现与JavaScript通讯的了解后，发现他们之间的实现方式和通讯方式天差地别，所以我们要实现一个跨平台的通讯方式只有从 JavaScript 上面入手。下一篇文章将介绍如何设计一个跨平台的 JSSDK。