title: React 那些破事儿
date: 2016-8-22 17:16:32
tags:
- JavaScript
categories:
- Tech
permalink: about-react
---
react、vue、angularjs作为这两年在前端界里面最为红红火火的几款框架，几乎每一次的技术交流、群里的讨论都少不了它们的相关内容。作为前端发开者即使并没有真正的用在开发上但至少也对这几款框架有所了解。

<!--more-->

# 最初的接触

作为当前最为火热的框架总少不了对比与骂声，小编所坚持的仍是“只有最合适，没有最好”的原则。当小编了解过刚才提到过的几款框架后，其实都比较清晰最适合他们的应用场景了：**交互繁杂、UI可高度重复使用。对于这种场景无非就是各种管理平台，如单页面应用、用户级的管理后台。**

这一次小编接到一个新的任务：移动端的内容审核后台。在分析需求时觉得十分贴合「交互繁杂、UI可高度重复使用」的条件，便决定了使用 React 作为这一次前端开发的框架，接口便是使用 Restful 方式。

### 官网入门教程过于模糊？

第一次接触显然便是从官网教程入手，当看完首页上所有的简单例子仍是难以理解的不在少数。其实官网首页的教程很好的突出了在 React 里面几个重要的元素：**React.createClass、render()、props、state、refs、Event System**。当使用Raect编写应用的时候，少不了都要去触碰这几个元素或许在看教程之前可以先了解一下这些元素都干了什么权当是预习。


**React.createClass**

在React的框架里，每一个元素、插件都以**Component（组件）**的形式存在。createClass 的方法即是创建一个 React 的 Component，可以覆写声明周期的所有方法、初始化的数据返回、上下文的返回等一些高级应用的技巧。


**render**

每一个Component（接下来都叫组件）都应当覆写render方法，该方法返回的是组件的试图也就是我们平时编写的HTML代码。最值得称道的是 react 有一套在js里面写HTML的规范：JSX。JSX 和一般的 HTML 差异并不大，但仍需一些注意的地方：样式类名需要改成 className，不能直接写HTML代码等。


**props**

每一个组件都有其Props（属性），其属性一般由其父组件传递并且自身不做改变的所以一般用来存放相对静态的数据。


**state**

和属性一样是数据模型，但State（状态）仅仅是为了互动性。**React是用两种数据模型的，Props是作为该组件的基础数据，而State则仅仅是为了互动而产生的数据改动。**

state和props是最容易混淆的一个地方，因为两者几乎是一样但在React编程思想中，对于两者有明确的区分。下面举个例子或许可以让大家更清晰的了解两者的区分：


> 一个用户的信息如nickname、gender、accountId等应该存放在props中。
而当这个用户修改头像时，isShowCropImgDialog（是否显示裁剪图片的弹窗）这种状态属性的改变应当存放在state中。（看到这里没有接触过React的人或许迷糊了，以往这种弹窗不是都是直接在js代码中一个`xxx.show()`吗?请耐心看下去，下面会有解释。）

想了解更多可阅读：[Interactivity and Dynamic UIs](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html)



**refs**

当你编写完JSX、render()之后想获取回某些元素实例的时候就需要用到refs了。如需要获取某个input的值，react并不建议直接通过获取获取DOM并获取这个值，而是通过refs的方式去获取。

说到这个点上，React 编程思想中有一个很明确的要求：**万不得已，不要直接对DOM进行操作**，众所周知React为了效率是采用了虚拟DOM，这也是为什么不能直接对dialog进行`.show()`的原因。

[more-about-refs](https://facebook.github.io/react/docs/more-about-refs.html)

﻿

**Event System**

几乎所有事件控制都不需要额外的绑定，而是在JSX上进行的绑定。详细请看：[Event System](https://facebook.github.io/react/docs/events.html)


# 下次

接下来下一篇文章或许还是说react，然后说说关于如何快速理解 react 的开发模式，redux 的一些运用。其实react并不难，但redux还是需要去理解一下。