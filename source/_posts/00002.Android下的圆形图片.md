title: Android下的borderRadius
date: 2015-2-14 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: android-borderRadius
---
<code>border-radius</code> 的出现算是一次革命性的更新，在老一辈的记忆中圆角的实现都是采用图片的统一解决方案，但随着CSS3发布和浏览器支持圆角的实现逐渐变得轻松起来，<code>border-radius</code> 也开始被广泛应用。

<!--more-->

随着移动设备的普及，前端的工作不在仅仅在电脑上的浏览器做兼容，也需要去做移动设备的兼容，虽然 <code>border-radius</code> 在桌面上已经相当成熟，但在安卓设备上仍是需要花费不小心思去做兼容。

这里以做一个圆形的图片为例子，来探讨一下Android下都有什么坑。

<h2>Android 2.3 不支持 % 百分号</h2>

一般需要实现圆形的图片只需要 <code>border-radius:50%</code> 就可以实现圆形的效果，但偏偏Android 2.3 版本并不支持百分号的写法。

<pre class="decode:true lang:css">//在android 2.3 下没办法实现圆形图片
.round img{
    border-radius: 50%; //实现圆形
    border: 4px solid white; //描边
}

//修改后
.round img{
    border-radius: 999px; //实现圆形，设置较大的圆角像素强迫形成一个圆形
    border: 4px solid white; //描边
}
</pre>

<h2>某些低版本的 Android 和 Safari 会存在圆角问题</h2>

某些低版本的设备对于同时存在 <code>border</code> 和 <code>border-radius</code> 的 <code>img</code> 元素会出问题，圆形还是圆形但是图片是正方的。

<a href="http://inhu.net/wp-content/uploads/2015/02/未标题-1.jpg"><img src="http://inhu.net/wp-content/uploads/2015/02/未标题-1.jpg" alt="未标题-1" width="400" height="200" class="alignnone size-full wp-image-968" /></a>

左侧为希望实现的效果图，但在某些低版本设备中会出现右侧的情况。很显然这种现实效果不是我们希望看到的。

让我们通过下面的图片探究一下为什么会这样吧。

<a href="http://inhu.net/wp-content/uploads/2015/02/border-radius-01.jpg"><img src="http://inhu.net/wp-content/uploads/2015/02/border-radius-01.jpg" alt="border-radius-01" width="600" height="800" class="alignnone size-full wp-image-969" /></a>

显然这一切都是因为浏览器以最外边距开始裁剪导致的问题，那么我只需要保持内容区域以外都是”干净“的就可以了。

实现方式为在图片内容的外层添加边框。

<pre class="decode:true lang:css">.round{
    display-inline: block;
    border-radius: 999px; //为了外层的边框也是圆形
    vertical-align: middle; //使图片垂直居中,如果不垂直居中将会发现上边
    border: 4px solid white; //描边
}
.round img{
    border-radius: 999px; //实现圆形，设置较大的圆角像素强迫形成一个圆形
    vertical-align: middle; //使图片垂直居中
}
</pre>

<h2>圆形背景色不裁剪</h2>

同时设置<code>border-radius</code>和<code>background-color</code>时，背景色并没有裁剪。这显然也是某些版本的浏览器的BUG，这个时候只需要添加<code>background-clip: padding-box;</code>来修复。

<pre class="decode:true lang:css">.round{
    display-inline: block;
    border-radius: 999px; //为了外层的边框也是圆形
    vertical-align: middle; //使图片垂直居中,如果不垂直居中将会发现上边
    border: 4px solid white; //描边
}
.round img{
    border-radius: 999px; //实现圆形，设置较大的圆角像素强迫形成一个圆形
    vertical-align: middle; //使图片垂直居中
    background-color: #eee;
    background-clip: padding-box;
}
</pre>

<h3>其他问题</h3>

据另外一篇文章说，还有一种情况就是不支持<code>border-radius</code>缩写，需要完整的写出扩写属性。但笔者还没有遇到过这种情况，如果发现<code>border-radius</code>不好使的时候，可以试试以下方法：

<pre class="decode:true lang:css">    border-top-left-radius: 999px; // 左上角
    border-top-right-radius: 999px; // 右上角
    border-bottom-right-radius: 999px; // 右下角
    border-bottom-left-radius: 999px; // 左下角
</pre>