title: last-of-type 选择器的发现
date: 2016-11-21 12:16:32
tags:
- Css
categories:
- Tech
permalink: about-last-of-type-selectors
---
`:last-of-type` 是CSS3新增的选择器，表示指定元素名称位于父元素的字元素列表中的最后一个元素。简而言之就是**最后的一个元素**。在[W3C][1]的说明和[MDN][2]的说明页面上都是以元素作为前置选择，但其实`:last-of-type `也可以作为样式名的伪类选择器。

<!--more-->

## MDN例子

```css
p em:last-of-type {
 color: lime;
}
```

```html
	<p>
	  <em>I'm not lime :(</em>
	  <strong>I'm not lime :(</strong>
	  <em>I'm lime :D</em>
	  <strong>I'm also not lime :(</strong>
	</p>

	<p>
	  <em>I'm not lime :(</em>
	  <span><em>I am lime!</em></span>
	  <strong>I'm not lime :(</strong>
	  <em>I'm lime :D</em>
	  <span><em>I am also lime!</em> <strike> I'm not lime </strike></span>
	  <strong>I'm also not lime :(</strong>
	</p>
```

### 结果

<style>
.lot-example{
	margin:0 0 15px;
	padding:10px;
	background-color: #eee;
}
.lot-example p:last-child{
	margin:0;
}
.lot-example p em:last-of-type {
 color: lime;
}
</style>

<div class="lot-example"><p>		<em>I'm not lime :(</em>		<strong>I'm not lime :(</strong>		<em>I'm lime :D</em>		<strong>I'm also not lime :(</strong>	</p>	<p>		<em>I'm not lime :(</em>		<span><em>I am lime!</em></span>		<strong>I'm not lime :(</strong>		<em>I'm lime :D</em>		<span><em>I am also lime!</em><strike> I'm not lime</strike></span>		<strong>I'm also not lime :(</strong>	</p></div>

如果按照上面的代码理解，所有`p`元素下的最后一个`em`元素的字体将会是`lime`颜色。

## 样式名之后的`last-of-type`

看完MDN的说法应该也对`last-of-type`的选择器有了一定的了解，当所有比较官方的例子中都以元素作为前置的选择器那么如果以样式名作为前置选择器会怎么样呢？

### 是否有效？

```css
.wrap .a:last-of-type{
  color: lime;
}

.wrap .b:last-of-type{
  color: red;
}
```

```html
	<div class="wrap">
	  <p class="a">我是一段文字</p>
	  <p class="b">我是一段文字</p>
	  <p class="c">我是一段文字</p>
	  <p class="b">我是一段文字</p>
	  <p class="a">我是一段文字</p>
	</div>
```

### 结果

<style>
.wrap .a:last-of-type{
	color: lime;
}
.wrap .b:last-of-type{
	color: red;
}
</style>

<div class="wrap lot-example"><p class="a">我是一段文字a</p><p class="b">我是一段文字b</p><p class="c">我是一段文字c</p><p class="b">我是一段文字b</p><p class="a">我是一段文字a</p></div>

### 总结

看到执行结果可能会有读者感到很疑惑，为何`.wrap .a:last-of-type`选择符是有效果的而`.wrap .b:last-of-type`却失效了呢？

首先可以肯定的是`:last-of-type`是支持样式名选择器作为前置的，但只是要符合以下条件：

**样式名选择器所选择的元素为当前父元素的子元素列表中的最后一个**，也就是说不但要符合选择器`.b`并且要符合`${element}:last-of-type`。在

> 针对上面HTML的结构来生成一个表达式的话，
`.wrap .b:last-of-type` === `.wrap .b && p:last-of-type`。

如果把上面的HTMl代码改成如下：

```html
	<div class="wrap">
	  <p class="a">我是一段文字</p>
	  <p class="b">我是一段文字</p>
	  <p class="c">我是一段文字</p>
	  <div class="b">我是一段文字</div>
	  <p class="a">我是一段文字</p>
	</div>
```

<div class="wrap lot-example"><p class="a">我是一段文字a</p><p class="b">我是一段文字b</p><p class="c">我是一段文字c</p><div class="b">我是一段文字b</div><p class="a">我是一段文字a</p></div>


按上述修改后，最后一个`.b`把元素修改成`div`则符合了`${element}:last-of-type`这一个要求，所以就生效了。


[1]:	http://www.w3schools.com/cssref/sel_last-of-type.asp "CSS3 :last-of-type Selector - W3Schools"
[2]:	https://developer.mozilla.org/en-US/docs/Web/CSS/:last-of-type ":last-of-type - CSS | MDN"