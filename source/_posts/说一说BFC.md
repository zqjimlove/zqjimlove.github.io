title: 说一说BFC
date: 2015-4-1 12:16:32
tags:
- Css
categories:
- Tech
permalink: what-is-bfc
---
对于一个前端来说BFC是一个必须知道的知识点至少笔者是这么觉得的，因为在很多知识点上都涉及了BFC如：清除浮动、间距折叠等问题的解决办法其实都是和BFC有着千丝万缕的关系。

<!--more-->

## W3C对BFC的解释

http://www.w3.org/TR/CSS2/visuren.html#block-formatting

> 浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells, 和 table-captions），以及overflow值不为“visiable”的块级盒子，都会为他们的内容创建新的BFC（块级格式上下文）。
>
> 在BFC中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的margin 值所决定的。在一个BFC中，两个相邻的块级盒子的垂直外边距会产生折叠。
>
> 在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）。

![http://inhu.qiniudn.com/BFC-0.jpg](http://inhu.qiniudn.com/BFC-0.jpg)

## 通俗讲解

首先明确一点BFC并非技术或者方法，而是一个概念一个规范。那个所谓的BFC我们该怎么更简单容易地理解它呢？我们可以把BFC理解成为网页布局中独立出一个布局环境，不受外界任何影响。也就是在BFC内的元素是不受外界影响。由于这里涉及到了关于网页布局的一些问题，那么我们先简单熟识一下网页的几个布局格式。

但如果嫌文章过长，那么总结一句就是：** 另起一行，重新布局**！

### 布局

**常规流 Normal Flow**

在普通流中，所有块级元素会按照 HTML 中编码的先后顺序由上至下。而行内元素由左至右横向排列直到该行被占满然后换行。

**浮动 Floats**

在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

**绝对定位**

在绝对定位布局中，元素会整体脱离常规流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。

![http://inhu.qiniudn.com/BFC-1.jpg](http://inhu.qiniudn.com/BFC-1.jpg)

看完了关于网页的布局之后，我们回到BFC这个话题上。认识并且熟识BFC是对于前端在做布局上面是必要的，很多前端或者非前端因为不认识BFC导致了他们所谓莫名其妙的问题。其实BFC是解决垂直间距折叠、清除浮动这些常见问题的根源。

### 如何触发BFC

根据 [Block formatting context-MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context) 的文章，只需满足以下条件之一即可：

* 根节点或其他东西（好想吐槽一下这个其他东西是什么鬼？求高手指点）。
* 浮动（左或右，不包括none）。
* 绝对定位（absolute，fixed）。
* display 为以下其中之一的值 inline-blocks，table-cells，table-captions。
* overflow 除了 visible 以外的值。
* CSS3新增的flex boxes（display 为 flex 或 inline-flex）

### BFC的特性

* 根据 [CSS 2.1 8.3.1 Collapsing margins](http://www.w3.org/TR/CSS21/box.html#collapsing-margins) 第一条，常规流中的块框在垂直位置的外边距会发生折叠现象。也就是处于同一个BFC中的两个垂直窗口的margin会重叠。
* 根据 [CSS 2.1 8.3.1 Collapsing margins](http://www.w3.org/TR/CSS21/box.html#collapsing-margins) 第三条，产生了BFC的元素不会和在流中的子元素发生外边距折叠。所以解决外边距折叠的方法就是产生新的BFC。
* 在 BFC 中，每一个元素左外边与包含块的左边相接触（对于从右到左的格式化，右外边接触右边）， 即使存在浮动也是如此（尽管一个元素的内容区域会由于浮动而压缩），除非这个元素也创建了一个新的 BFC 。简单来说可以控制浮动元素的浮动区域。

## BFC的实际运用

### 阻止外边距折叠

阻止外边距折叠有多个办法，其中一个就是生成一个新的BFC。只要触发一个BFC条件，外边距的折叠就会被阻止。除了新建一个BFC还可以利用 `padding`、`border-top-width` 设置一个非0的值也可。

![http://inhu.qiniudn.com/BFC-2.jpg](http://inhu.qiniudn.com/BFC-2.jpg)

### 阻止元素被浮动元素覆盖

浮动元素的块级兄弟元素会无视浮动元素的位置尽量去占满一行，但这样就会导致浮动元素遮挡了兄弟元素。要解决这个遮挡的话，就需要在兄弟的块级元素上实现BFC。

![http://inhu.qiniudn.com/BFC-3.jpg](http://inhu.qiniudn.com/BFC-3.jpg)

**需要注意的是，当兄弟元素的宽度大于容器所剩下的宽度的时候便会产生换行。即我们平时清除浮动其实就是使兄弟元素产生新的BFC，以达到换行效果。**


## BFC 与 hasLayout

说到W3C的标准就必须在最后不上IE系列的某些问题，IE6-7由于不支持 W3C 的 BFC 标准，而是使用了私有属性 hasLayout。虽然 hasLayout 与 BFC 在某些方面表现十分相近，但 hasLayout 自身也存在着诸多的问题。hasLayout 笔者不再在这里写了，毕竟谷歌一下都有很详尽的文章。
