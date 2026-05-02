---
layout: post
title: 移动端常见、罕见的CSS布局问题
date: 2018-07-25 20:00:00 +0800
categories:
- Tech
tags:
- CSS
- 前端

---

### 移动端点击延迟问题

- 问题描述：手机上有双击事件，所以第一次点击时浏览器会等一会(约300ms)来看看用户是不是要双击。
- 解决方案：使用[fastclick](https://www.npmjs.com/package/fastclick)

### 移动端点击穿透问题

- 问题描述：移动端事件的触发时间顺序为`touchstart 早于 touchend 早于 click`，如果触发了A元素的`touched`事件并在`click`出发前把A元素隐藏了，会触发A元素下面的B元素`click`事件。
- 解决方案：
	- 使用一致的事件，都是用`touch`事件或者`click`事件。
	- 使用`fastclick`

### iOS下的1px边框


```
// 方案一：
.some-element {
  border-width: thin;
}

// 方案二(使用less实现):
// 支持圆角的1px边框，原元素要设置相对定位 position: relative;
.one-px-border(@color, @radius) {
    border-radius: @radius;
    &:after {
        content: '';
        border: 1px solid @color;
        border-radius: @radius * 2;
        transform: scale(0.5);
        transform-origin: 0 0;
        width: 200%;
        height: 200%;
        position: absolute;
        pointer-events:none;
        top: 0;
        left: 0;
    }
}
```

### 子元素img的height不等于父元素的height

大多数的浏览器会的`<img>`样式默认值是:

``` css
// dafault browser style
img { 
    display: inline-block;
}

// html
<div class="outer">
<img src="some" height="100%"/>
</div>

```
某些情况下，我们希望`<img>`的高度等于父元素`<div>`的高度，但是会发现父元素的高度比`img`高度要大。解决方案如下:

- 方案一：设置`vertical-align: top;`
- 方案二：设置`display: block;`

参考：

- [div container larger than image inside [duplicate]](https://stackoverflow.com/questions/11447707/div-container-larger-than-image-inside/11447770#11447770)

### flex布局下的文本超长自动折断

flex布局下默认：子元素(flex item)元素的长度不能比其元素的内容的长度小，子元素CSS默认值为:

``` css
min-width: auto;
min-height: auto;
```

在子元素上设置超长文本自动折断，会失效。需要重置子元素的样式为:

``` css
// 方案一:
min-width: 0;
min-height: 0;

// 方案二:
overflow: hidden;
```

![What we want](https://cdn.css-tricks.com/wp-content/uploads/2016/05/want.gif)

我们想要的👆(image from [Flexbox and Truncated Text](https://css-tricks.com/flexbox-truncated-text/))

![The potential problem](https://cdn.css-tricks.com/wp-content/uploads/2016/05/busted.gif)

可能得到的👆(image from [Flexbox and Truncated Text](https://css-tricks.com/flexbox-truncated-text/))

参考：

- [Flexbox and Truncated Text](https://css-tricks.com/flexbox-truncated-text/)

## 相关文章

- [移动端常见bug汇总001](https://juejin.im/post/5af918636fb9a07ac5603ecb)