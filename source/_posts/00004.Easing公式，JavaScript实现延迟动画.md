title: Easing公式，JavaScript实现延迟动画
date: 2015-2-17 12:16:32
tags:
- JavaScript
categories:
- Tech
permalink: easing-equations
---
好久没更新了，回到老家连抢红包都心塞的网络迫使回归原始生活一段时间，所以连一篇恭贺新禧的文章都懒得发了。

在过去的马年最后几天，博主在无所事事的时候研究了一下各种各样的`easing`的公式（博主数学学得烂不要问原理，我只是搬运工）。

<!--more-->

[去看看效果](http://jsbin.com/nojanu/)

简单的说说这个公式是怎么运用的，分别用4个参数起含义可以看代码的注释，后b、c、d参数基本是固定的值。博主简单举个栗子：

现在有一个直线动画，由0px（**参数b**）到800px（**参数c**）拆分成1000次运算（**参数d**），当参数t由0逐渐接近参数d的时候，得到的值也会由参数b逐渐接近参数c（也就是说由0px逐渐接近800px）。



```javascript
/**
 * @param  {Number} t [当前时间点]
 * @param  {Number} b [开始数值，一般都是0]
 * @param  {Number} c [最大的值]
 * @param  {Number} d [时间]
 * @return {Number}   [算出来的值咯]
 */
var EasingMatch = {
        linearIn: function(t, b, c, d) {
            return c * t / d + b;
        },
        quinticIn: function(t, b, c, d) {
            t /= d;
            return c * t * t * t * t * t + b;
        },
        quinticOut: function(t, b, c, d) {
            t /= d;
            t--;
            return c * (t * t * t * t * t + 1) + b;
        },
        quinticIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * t * t * t * t * t + b;
            t -= 2;
            return c / 2 * (t * t * t * t * t + 2) + b;
        },
        quadraticIn: function(t, b, c, d) {
            t /= d;
            return c * t * t + b;
        },
        quadraticOut: function(t, b, c, d) {
            t /= d;
            return -c * t * (t - 2) + b;
        },
        quadraticIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * t * t + b;
            t--;
            return -c / 2 * (t * (t - 2) - 1) + b;
        },
        sinusoidalIn: function(t, b, c, d) {
            return -c * Math.cos(t / d * (Math.PI / 2)) + c + b;
        },
        sinusoidalOut: function(t, b, c, d) {
            return c * Math.sin(t / d * (Math.PI / 2)) + b;
        },
        sinusoidalIO: function(t, b, c, d) {
            return -c / 2 * (Math.cos(Math.PI * t / d) - 1) + b;
        },
        cubicIn: function(t, b, c, d) {
            t /= d;
            return c * t * t * t + b;
        },
        cubicOut: function(t, b, c, d) {
            t /= d;
            t--;
            return c * (t * t * t + 1) + b;
        },
        cubicIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * t * t * t + b;
            t -= 2;
            return c / 2 * (t * t * t + 2) + b;
        },
        expIn: function(t, b, c, d) {
            return c * Math.pow(2, 10 * (t / d - 1)) + b;
        },
        expOut: function(t, b, c, d) {
            return c * (-Math.pow(2, -10 * t / d) + 1) + b;
        },
        expIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * Math.pow(2, 10 * (t - 1)) + b;
            t--;
            return c / 2 * (-Math.pow(2, -10 * t) + 2) + b;
        },
        quartiIn: function(t, b, c, d) {
            t /= d;
            return c * t * t * t * t + b;
        },
        quartiOut: function(t, b, c, d) {
            t /= d;
            t--;
            return -c * (t * t * t * t - 1) + b;
        },
        quartiIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return c / 2 * t * t * t * t + b;
            t -= 2;
            return -c / 2 * (t * t * t * t - 2) + b;
        },
        circularIn: function(t, b, c, d) {
            t /= d;
            return -c * (Math.sqrt(1 - t * t) - 1) + b;
        },
        circularOut: function(t, b, c, d) {
            t /= d;
            t--;
            return c * Math.sqrt(1 - t * t) + b;
        },
        circularIO: function(t, b, c, d) {
            t /= d / 2;
            if (t < 1) return -c / 2 * (Math.sqrt(1 - t * t) - 1) + b;
            t -= 2;
            return c / 2 * (Math.sqrt(1 - t * t) + 1) + b;
        }
    };
```
```javascript
//简单使用例子
var curTime  = 0,
makeAnimation = function(left) {
    $('#animationBall').css({
        left: (left + 10) + 'px'
    });
    if (left >= maxWidth){
        curTime = 0;
    }
    setTimeout(function(){
        curTime++;
        makeAnimation(EasingMatch.circularIO(curTime, 0, maxWidth, 800));
    }, 0);
};
makeAnimation(EasingMatch.circularIO(curTime, 0, maxWidth, 800));
```
如果不想使用Js的话，下面送你们一个CSS的……

[CSS3 公式](http://matthewlein.com/ceaser/)