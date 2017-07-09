---
layout: post
title: React 性能优化
date: 2017-02-23 19:52:00 +0800
categories:
- Tech
tags:
- 性能优化
- React

---

## 优化的出发点

1. 尽量只做该做的事。
2. 尽量提高该做的事的效率。

----

## 优化建议

React 官方提供的性能优化建议已经写得很好了，请查看：[Optimizing Performance - React](https://facebook.github.io/react/docs/optimizing-performance.html)


- 通过 `shuoldComponentUpdate()` 方法阻止 React 重绘。未来的 React 将视 `shuoldComponentUpdate()` 的返回值为一个暗示而不是严格的命令，返回 `false` 时仍可能重绘。
- 使用纯组件 `React.PureComponent`，纯组件 `shouldComponentUpdate()` 方法的实现为“浅比较(shallow prop and state comparison)”，意味着引用类型的值在其属性改变而本身引用不变时，不会重绘。
- 保持数组中各个元素的每个 `key` 独立，避免数组中各个元素的 `key` 相互影响。

	* > Keys help React identify which items have changed, are added, or are removed. ***Keys should be given to the elements inside the array*** to give the elements a stable identity. Keys only make sense in the context of the surrounding array. More Info: [in-depth explanation about why keys are necessary](https://facebook.github.io/react/docs/reconciliation.html#recursing-on-children)、[Keyed Fragments](https://facebook.github.io/react/docs/create-fragment.html).
- 避免其他的组件间数据的相互影响。比如父组件在重绘时会引起子组件的重绘，而有的子组件并不需要重绘。这种情况下可以在子组件的 `shouldComponentUpdate()` 对比一下 `this.props` 和 `nextProps`。

----

## React 生命周期各阶段的优化

### Mounting：组件初始化渲染阶段(从初始化到插入到 DOM)

`Mounting` 阶段一共调用了 `constructor()`、`componentWillMount()`、`render()` 和 `componentDidMount()` 四个方法。

- `constructor()`——该方法在组件渲染之前调用，应避免长时间的操作。
- `componentWillMount()`——该方法在 `render()` 调用前执行，该方法相当于在渲染前给个通知的机会，用于服务器端渲染居多。
- `render()`——渲染的代码要尽量简单，对于渲染时要用到网络异步调用返回的数据时，可以放在 `componentDidMount()` 方法中。
- `componentDidMount()`——在这里进行一些数据的异步获取请求、较复杂的计算等。

### Updating：组件重绘阶段(props 或者 state 的改变引起 update)

`Updating` 阶段一共调用了 `componentWillReceiveProps()`、`shouldComponentUpdate()`、`componentWillUpdate()`、`render()` 和 `componentDidUpdate()` 五个方法。

- `componentWillReceiveProps(nextProps)`——在这个阶段可以通过 `this.setState()` 方法提前把 `nextProps` 需要的值更新到组件的 `state` 中。调用 `this.setState()` 一般不会触发 `componentWillReceiveProps()`
- `shouldComponentUpdate(nextProps, nextState)`——这个方法通过返回值高速组件该不该重绘。
-  `componentWillUpdate(nextProps, nextState)`——重绘前最后一次操作的机会。当 `shouldComponentUpdate()` 返回 `false` 时，该方法不会被调用。
- `render()`——重绘。
- `componentDidUpdate(prevProps, prevState)`——重绘完成后操作 DOM 的机会，比如 `prop` 更改后进行网络请求。当 `shouldComponentUpdate()` 返回 `false` 时，该方法不会被调用。

### Unmounting：组件卸载阶段(组件从 DOM 中被移除时调用)

`Unmounting` 阶段只调用了 `componentWillUnmount()` 方法。

- `componentWillUnmount()`——组件卸载前进行必要操作的机会。

详见：[React.Component](https://facebook.github.io/react/docs/react-component.html)

----

## 相关工具

- Performance Tools：[React Perf](https://facebook.github.io/react/docs/perf.html) - The Perf object can be used with React in development mode only. 

----

Ribin on February 23, 2017 Thursday

> Snowed the day before yesterday, February 21, 2017.