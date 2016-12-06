title: HTML 5.1 预览
date: 2016-12-5 2:16:32
tags:
- HTML
categories:
- Tech
permalink: HTML51
---

两年发布的HTML5对于web开发社区是一个重大的变革，不仅仅是因为它提供了大量的新特性，还有是因为它是自1999年发布的HTML 4.1 以来最重大的一个更新。你今天仍然能看到很多网站吹嘘自己使用了“时髦”的HTML5。

很幸运的，我们无需再经历如此长时间来等待下一个版本的迭代。在2015年10月，W3C开始着手起草HTMl5.1的草案以修复现版本HTML5的问题。经过多次迭代之后，在2016年6月更新成为 Candidate Recommendation（候选推荐）状态，同年9月更新为 Proposed Recommendation（推荐标准）状态，最终在11月更新为 [W3C Recommendation][1] （W3C推荐）。在经历了这些发展后可能我们也发现这一条路十分崎岖，一些最初的 HTML 5.1 特性到最后由于设计上的缺陷和浏览器厂商的不支持导致被放弃了。

<!--more-->

即使HTML 5.1仍处于发展期间，但W3C就已经开始着手起草 [HTML 5.2 草案][2]并计划在2017年底发布。在此期间，下面将会为大家简单介绍一下HTML 5.1 的新特性和改进。尽管现有的浏览器对HTML 5，1还是缺乏支持，但本文会尽量提供可执行的例子。

## 使用`menu`和`menuitems`元素创建上下文菜单。

在5.1的草案介绍当中`menu`元素有两处不同的改动。`context`和`toolbar`。在以前它们是用来扩展原生浏览器的上下文菜单，通常用于右键菜单和自定义菜单组件。在发展过程中，`toolbar`被放弃了，但是`context`仍然保留。

现在可以通过使用[`<menu>`][3]标签包含一个或多个[`<menuitem>`][4]元素，然后其他元素通过[`contextmenu`][5]属性就行绑定。

每一个`<menuitem>`都可以三选一个类型：

- `checkbox` - 允许用户选择或反选一个选项。
- `command` - 允许用户点击后执行一个动作。
- `radio` - 允许用户从一组选项里面选择一个。

下面是一个基本的例子，但它只能在Firefox 49下工作，Chrome 54 暂时扔不支持。

<iframe id="cp_embed_bBrvRP" src="//codepen.io/SitePoint/embed/bBrvRP?height=263&amp;theme-id=6441&amp;slug-hash=bBrvRP&amp;default-tab=result&amp;user=SitePoint&amp;embed-version=2&amp;pen-title=HTML%205.1%20menu%20example" scrolling="no" frameborder="0" height="263" allowtransparency="true" allowfullscreen="true" name="CodePen Embed" title="HTML 5.1 menu example" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>

在可支持的浏览器下，我们应该可以看到如下图：

![html5.1 上下文菜单](/images/html51_1.png)

## 详情和总结元素

新的[`<details>`][6]和[`<summary>`][7]元素可以实现通过点击一个元素来显示或者隐藏额外的信息。我们以往都是通过使用JavaScript来实现的，现在我们在HTML里面使用`<details>`元素和`<summary>`元素就能做到这一点。在`<details>`元素中点击总结即可切换显示其余的内容。

接下来的例子可以执行在Firefox和Chrome下。

<iframe id="cp_embed_rWzgzg" src="//codepen.io/SitePoint/embed/rWzgzg?height=395&amp;theme-id=6441&amp;slug-hash=rWzgzg&amp;default-tab=result&amp;user=SitePoint&amp;embed-version=2&amp;pen-title=HTML%205.1%20details%20and%20summary%20demo" scrolling="no" frameborder="0" height="395" allowtransparency="true" allowfullscreen="true" name="CodePen Embed" title="HTML 5.1 details and summary demo" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>

在可支持的浏览器下，我们应该可以看到如下图：

![html5.1 详情和总结](/images/html51_2.png)

## 更多的输入框类型：`month` `week` 和 `datatime-local`

在强大的输入框类型上再扩展了三种更多的类型：[`month`][8],[`week`][9]和[`datatime-local`][10]。

前面两个的类型允许用户选择一个周或者一个月。在Chrome浏览器下它们均被渲染为一个下拉的时间控件用作选择一个今年特指的月份或者周。当利用JavaScript获取它们的值的时候会得到一个字符串，类似：周控件的值为 `“2016-W43”`,月控件值为`"2016-10"`。

起初，5.1草案引入了两个时间类型的输入框：`datetime` 和 `datetime-local`，其中差别就是`datatime-local`总是使用用户的时区，而`datetime`可以允许用户选择不同的时区。在发展过程中，`datetime`类型被放弃了只保留了`datatime-local`类型。`datatime-local`由两部分组成，日期部分类似`week`或`month`输入框，时间部分需要分别输入。

<iframe id="cp_embed_xRLowg" src="//codepen.io/SitePoint/embed/xRLowg?height=400&amp;theme-id=6441&amp;slug-hash=xRLowg&amp;default-tab=result&amp;user=SitePoint&amp;embed-version=2&amp;pen-title=HTML%205.1%20week%2C%20month%20and%20datetime%20inputs" scrolling="no" frameborder="0" height="400" allowtransparency="true" allowfullscreen="true" name="CodePen Embed" title="HTML 5.1 week, month and datetime inputs" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>

在可支持的浏览器下，我们应该可以看到如下图：

![html5.1 日期时间控件](/images/html51_3.png)

## 响应式图片

HTML 5.1 引入了几个新特性可以在不适用CSS的情况下实现响应式图片。这些功能每一个都能涵盖所有场景。

### 图片属性 `srcset`

图片的[`srcset`][11]属性允许开发者在不同像素比的情况下列出可替换的图片源。这个特性可以使浏览器自动去选择对于用户设备最优的图片（由像素比、缩放级别和网络速度决定）。举个例子，你可能会想要当用户在使用小屏幕或者网速较慢的手机的时候提供一张低分辨率的图片。

`srcset` 属性接受一个以逗号作为分割的带`x`作为修饰符号的地址列表，这是用于描述各个像素比（物理像素和CSS像素的比率）下合适的图片。一个简单的例子如下：

```html
<img src="images/low-res.jpg" srcset="
  images/low-res.jpg 1x, 
  images/high-res.jpg 2x, 
  images/ultra-high-res.jpg 3x"
/>
```

在上面的例子中，如果用户的设备像素比是1，就会使用`low-res.jpg`作为显示。如果是2，就会使用`high-res.jpg` 作为显示。如果是3或更高，`ultra-high-res.jpg`将会被选中使用。

```html
<img src="images/low-res.jpg" srcset="
  images/low-res.jpg 600w, 
  images/high-res.jpg 1000w, 
  images/ultra-high-res.jpg 1400w"
/>
```

在上面的例子中，600px 宽的设备会使用 `low-res` 图片，1000px 宽使用 `high-res` , 1400px 或者以上会使用 `ultra-high-res`。

### 图片属性 `sizes`

开发者可能会想要在不同的屏幕尺寸下显示不同的图片。例如，你可以展示一系列的图片再两列宽度的屏幕上和只有一列的小屏幕上。这可以借助`sizes`属性来实现。它运行开发者将屏幕的宽度转化为图片分配的空间，然后使用`srcset`属性选择适合的图片。

```html
<img src="images/low-res.jpg" sizes="(max-width: 40em) 100vw, 50vw" 
  srcset="images/low-res.jpg 600w, 
  images/high-res.jpg 1000w, 
  images/ultra-high-res.jpg 1400w"
/>
```

`sizes`属性定义了当视窗的宽度如果大于40em则图片的宽度为视窗的50%，当视窗小于等于40em时则图片宽度为视窗的100%。

### `<picture>`

如果认为以上特性还不足够去使图片适应每一个屏幕或者你一个功能需要完完全全显示不同的图片，那么你可以使用[`<picture>`][12]。它允许通过开发者用一个`<picture>`包住`<img>`和多个子元素`<source>`来为不同的屏幕定义各种不同源的图片，然后`<source>`作为图片的URL来进行加载。

```html
<picture>
  <source media="(max-width: 20em)" srcset="
    images/small/low-res.jpg 1x,
    images/small/high-res.jpg 2x, 
    images/small/ultra-high-res.jpg 3x
  "/>
  <source media="(max-width: 40em)" srcset="
    images/large/low-res.jpg 1x,
    images/large/high-res.jpg 2x, 
    images/large/ultra-high-res.jpg 3x
  "/>

  <img src="images/large/low-res.jpg" />
</picture>
```

如果想了解更多关于响应式的图片，可以访问 [ How to Build Responsive Images with srcset ][17] 这篇文章

## 验证表单 `form.reportValidity()`

HTML5中提供了[`form.checkValidity()`][13]的方法用于校验一个表单下的所有定义了验证器的输入框，并且返回一个布尔值。新的`reportValidity()`和这个十分相似，它们同样允许你验证一个表单和获得结果，但是浏览器会另外向用户报告错误。

<iframe id="cp_embed_eBEwjg" src="//codepen.io/SitePoint/embed/eBEwjg?height=234&amp;theme-id=6441&amp;slug-hash=eBEwjg&amp;default-tab=result&amp;user=SitePoint&amp;embed-version=2&amp;pen-title=HTML%205.1%20report%20validity%20demo" scrolling="no" frameborder="0" height="234" allowtransparency="true" allowfullscreen="true" name="CodePen Embed" title="HTML 5.1 report validity demo" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>

"First Name"输入框当为空的时候会提醒用户为必填错误。

![](/images/html51_4.png)

## Frames 的 allowfullscreen

新的布尔属性[`allowfullscreen`][14]允许开发者通过调用[`requestFullscreen()`][15]方法来控制他们的内容是否可以在全屏显示。

##`element.forceSpellCheck()` 拼写检查

新的[`element.forceSpellCheck()`][16]方法可以允许你对文本元素进行拼写检查。同时这也是首个这种类型的特性，但暂时没有浏览器支持。或者它至少用于对用户无法直接编辑的元素进行拼写检查。

## 那些被放弃的特性

在第一版的草案中很多特性最终都被放弃，主要是由于缺乏浏览器厂商的支持。下面有一些有趣的想法：

### `inert` 属性

禁止所有子元素，类似于对每一个子元素添加一个`disabled`属性。

### `<dialog>`

`<dialog>`元素提供了一个弹窗的原生实现，甚至还有一个方便集成的方案——在`<dialog>`上设置方法属性可以阻止表单提高到服务器，而是关闭对话框并将值返回。

<iframe id="cp_embed_XNaLOg" src="//codepen.io/SitePoint/embed/XNaLOg?height=300&amp;theme-id=6441&amp;slug-hash=XNaLOg&amp;default-tab=result&amp;user=SitePoint&amp;embed-version=2&amp;pen-title=HTML%20dialog%20element" scrolling="no" frameborder="0" height="300" allowtransparency="true" allowfullscreen="true" name="CodePen Embed" title="HTML dialog element" class="cp_embed_iframe " style="width: 100%; overflow: hidden;"></iframe>



[1]: https://www.w3.org/TR/html/
[2]: https://www.w3.org/TR/html52/
[3]: https://www.w3.org/TR/html/interactive-elements.html#the-menu-element
[4]: https://www.w3.org/TR/html/interactive-elements.html#the-menuitem-element
[5]: https://www.w3.org/TR/html/interactive-elements.html#element-attrdef-global-contextmenu
[6]: https://www.w3.org/TR/html/interactive-elements.html#the-details-element
[7]: https://www.w3.org/TR/html/interactive-elements.html#the-summary-element
[8]: https://www.w3.org/TR/html/sec-forms.html#month-state-typemonth
[9]: https://www.w3.org/TR/html/index.html#contents
[10]: https://www.w3.org/TR/html/sec-forms.html#local-date-and-time-state-typedatetimelocal
[11]: https://www.w3.org/TR/html/single-page.html#element-attrdef-img-srcset
[12]: https://www.w3.org/TR/html/single-page.html#elementdef-picture
[13]:https://www.w3.org/TR/html/single-page.html#dom-htmlobjectelement-checkvalidity
[14]:https://www.w3.org/TR/html/single-page.html#element-attrdef-iframe-allowfullscreen
[15]:https://fullscreen.spec.whatwg.org/#dom-element-requestfullscreen
[16]:https://html.spec.whatwg.org/multipage/interaction.html#dom-forcespellcheck
[17]:https://www.sitepoint.com/how-to-build-responsive-images-with-srcset