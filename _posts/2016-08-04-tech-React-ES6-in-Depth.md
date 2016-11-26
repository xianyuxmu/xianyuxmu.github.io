---
layout: post
title: "从 React 深入 ES6"
date: 2016-08-04 19:15:35 +0800
categories:
- Technology
tags:
- 技术
- React
- ES6
---

![react_es6_in_depth](/uploads/tech/react_es6_in_depth.png)

### 首先，

了解 ES6 —— 推荐[《ECMAScript 6入门》](http://es6.ruanyifeng.com/)(可以在线看、简明扼要、中文、适合**初学者**)，同时，生命短暂，我更加推荐给你这个[《Exploring ES6》](http://exploringjs.com/es6/index.html#toc_ch_first-steps)(同样可以在线看、文字**直接了当、简明扼要**), 你可以从这个章节中简要地了解到 ES6 的新特性: [First steps with ECMAScript 6](http://exploringjs.com/es6/ch_first-steps.html)

<!-- more -->

一个不错的 ES6 学习笔记：[ES6学习笔记](http://imweb.io/topic/5548995d9615e51472f38ac6)

---

了解完 ES6 的新特性之后，开始了解一下 React 从 ES5 迁移到 ES6 的基本注意点：

ps: 我就不加赘述，请直接移步以下文章。

[React/React Native 的ES5 ES6写法对照表](http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8)

---

在 React 从 ES5 迁移到 ES6 的过程中的一些要点：

1. 箭头函数([Arrow functions](http://exploringjs.com/es6/ch_arrow-functions.html#ch_arrow-functions)): 在 ES5 中使用函数的时候我们要时刻小心 `this` ，具体不加赘述。
	* ES6 中，箭头函数会自动绑定到其所在”周围”(surroundings)、"情境(context)"的 this 上。但是记住，这种绑定是松散的(loosely),也就是说，如果你要把所有的 objects 绑定到一个 this 上，应该采取比箭头函数自动绑定更加有效的绑定方式。
	* ES5 中，React 实现了函数的绑定到情境下的 this (例如，`this.doSomething`)，而 ES6 中的 React 的 Component 基于类(Class)实现，需要手动绑定函数到 this 才能调用 (例如，`this.doSomething.bind(this)`)。
	* 使用语句块的箭头函数不会自动返回一个值，必须显式地使用 `return` 来返回一个值。
箭头后面如果是表达式的话，那么这个表达式总是会被返回(return)。
	* 当使用箭头函数来返回一个对象时，始终使用括号将返回的对象括起来。
2. 变量命名([Variables declaration](http://exploringjs.com/es6/ch_variables.html#ch_variables))：From var to let/const。其中要注意的点是，var 的定义范围(Scope)是 Function，而 let/const	 的定义范围是块(Block)
3. 模块([Module](http://exploringjs.com/es6/ch_modules.html#ch_modules)):更加简洁的 export 和 import 机制。

---

### ES6  下的 Mixin

90% 的情况下我们不需要 Mixin，从而实现一些**高阶组件**([Higher-order Components](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html#higher-order-components-explained))。而在 10% 的情况下， Mixin 是更好的选择。

> A higher-order component is just a function that takes an existing component and returns another component that wraps it.

Mixin 在 ES6 下的实现有好几种，我觉得唯有掌握 ES6 中 Mixin 机制的内在原理，才是关键。以下是网上一些 ES6 下 Mixin 的实现机制：

以下有三种方式，但其核心思想只有一个：*将多个类(mixin 类)“混入”（mix in）到另一个类(目标类)。*


#### 方式一：[ECMAScript 6 － Mixin模式的实现](http://es6.ruanyifeng.com/#docs/class)

阮一峰老师推荐的这种方式的核心思想是：*将多个类的接口“混入”（mix in）另一个类。*

也就是把 Mixin 对象 的 `property` 和 `prototype` 分别混入到 目标对象中，使用的时候只要继承这个类即可。

我们来看看啊这种方式中关键的三句：

`Reflect.ownKeys(source);` 这条语句返回对象 source 自己所有属性的键(key)，也包括 constructor、prototype 及 name。[详见文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys)

`Object.getOwnPropertyDescriptor(source, key);` 这条语句返回对象 source 中属性名为 key 对应的属性描述(property descriptor).[详见文档](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)


`Object.defineProperty(target, key, desc);` 这条语句直接在对象 target 中定义一个新的属性，其中属性名为 key，属性描述为 desc。[详见文档](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

接着就可以理解这种实现方式的基本实现原理了。

使用方式：

``` 
class DistributedEdit extends mix(Target, mixin1, mixin2) {
  // ...
}
```

使用这种方式在 ES6 中实现 mixin，需要定义额外的函数。

#### 方式二：采用 [react-mixin](https://github.com/brigand/react-mixin) 继续使用

该组件的实现方式是 对象级别([Class level behavior](https://github.com/brigand/react-mixin#class-level-behavior))的，简而言之，react-mixin 包的做法是抽取出通过 ES5 中  React.createClass() 方法构件的 prototype 级别的 Mixin ‘对象’ 的属性和方法注入到 ES6 中的类来完成 class 级别的 Mixin。

Example:

```
var reactMixin = require('react-mixin');
var someMixin = require('some-mixin');
class Foo extends React.Component {
    render: function(){ return <div /> }    
}
reactMixin(Foo.prototype, someMixin);
reactMixin(Foo.prototype, someOtherMixin);

```

#### 方式三：[更简单地通过函数来实现 ES6 下的 Mixin](https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775)



说明：该方式在 ES6 中，通过[**高阶组件**(High-Order Component)的方式](https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775)，结合 Class 的继承机制实现 Mixin。

该方式把 Mixin 组件定义成一个“增强函数(就是下文中的 `Enhance` 变量)”，通过 `Enhance(ComposedComponent)` 的方式传入要添加 Mixin 的目标对象，在 Enhance() 函数中构建并返回一个类(注意：该类继承了 Component)，这个类的render()中返回了 `<ComposedComponent {...this.props} data={this.state.data} />`，所以，在 HigherOrderComponent.js 中 `export default Enhance(MyComponent);`这一句就是返回了一个添加了 Mixin 的新类。


Enhance.js

```
import { Component } from "React";

export var Enhance = ComposedComponent => class extends Component {
  constructor() {
    this.state = { data: null };
  }
  componentDidMount() {
    this.setState({ data: 'Hello' });
  }
  render() {
    return <ComposedComponent {...this.props} data={this.state.data} />;
  }
};

```

HigherOrderComponent.js

```
import { Enhance } from "./Enhance";

class MyComponent {
  render() {
    if (!this.data) return <div>Waiting...</div>;
    return <div>{this.data}</div>;
  }
}

export default Enhance(MyComponent); // Enhanced component

```

如果一下子看不懂 Enhance.js 的话，可能是因为它采用了箭头函数，通过以下普通函数版本或许你就可以看懂了：

```
export default function Enhance(ComposedComponent) {
  return class extends Component {
    ...
  }
}

```

再来看一个例子：

#### [Simple Mixin via Subclassing](http://exploringjs.com/es6/ch_classes.html#_simple-mixins)


通过函数实现一个 mixin，这个函数接受一个父类(superclass，也就是我们要加入 mixin 的目标对象)，同时这个函数返回输出一个**继承了这个父类**的子类(subclass)：

```
// mixin 1
const Storage = Sup => class extends Sup {
    save(database) { ··· }
};

// mixin 2
const Validation = Sup => class extends Sup {
    validate(schema) { ··· }
};

// finally, add mixin 1 & mixin 2 to target class(Employee)

class Employee extends Storage(Validation(Person)) { ··· }

```

该方法亲测有效！

同时，在 React 中，我们也可以采用一个更贴近 类继承 概念的、更直观的方式实现，如下：

```

// 1st step: define class mixin, it's a Component!
export default class Mixin extends Component {

  constructor(props){
    super(props);
  } 
  ...
}

// 2nd step: extends Mixin from TargetClass

export default class TargetClass extends Mixin {
	...
｝

```

#### 方式四：[PureRenderMixin 请移步到官方文档](https://facebook.github.io/react/docs/pure-render-mixin.html) 以及这个[优化方法](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html#performance-optimizations)

PureRenderMixin 说明：If your React component's render function is "pure" (in other words, it renders the same result given the same props and state), you can use this mixin for a performance boost in some cases. － from React Official Doc.

---

### 深入理解 ES6 中的 [Class](http://exploringjs.com/es6/ch_classes.html#_simple-mixins)：

对于 ES6 的继承机制，本质上还是单一继承树的原型继承模型，不会存在[菱形继承问题](http://blog.csdn.net/tounaobun/article/details/8443228)。也就是说 JS 中一个 Object 只会存在一个 prototype，而如果 ES6 中的类多继承的话(实际上没有现成的方法，可以间接实现)，则按照继承的先后顺序覆盖同名的属性和方法。而 ES6 中的 `extends` 允许我们从声明的时候实现继承功能，同时 extends 右手边的语句不是接收一个固定值，而是允许任何的表达式，你可以写一个函数让把两个原型(prototype)合到一块，例如：` class Foo extends combine(MyMixin, MySuperClass) {}`。并在类声明的时候调用这个表达式。所以我们看到上文中的实现方式的内在原理[就是这个](http://stackoverflow.com/questions/29879267/es6-class-multiple-inheritance)，[更具体的阐述](http://exploringjs.com/es6/ch_classes.html#_simple-mixins)。


---

### React 官方关于 Mixin 的说明：


React 官方并没有提供 ES6 下 mixin 的任何支持，同时，官方建议如果还想要使用 Mixins 的话，就继续使用 `React.createClass`。

#### No Support!

> Unfortunately, we will not launch any mixin support for ES6 classes in React. That would defeat the purpose of only using idiomatic JavaScript concepts.
There is no standard and universal way to define mixins in JavaScript. In fact, several features to support mixins were dropped from ES6 today. There are a lot of libraries with different semantics. We think that there should be one way of defining mixins that you can use for any JavaScript class. React just making another doesn’t help that effort.

 —— quoted from [Mixin in Official Doc.](https://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#mixins)



#### Why not Mixin? 

> [*Mixins break composition and make components harder to extract and move around.*](https://medium.com/@dan_abramov/the-future-of-drag-and-drop-apis-249dfea7a15f#.7muiud28g)


阐述 Mixins 在大多数场景下的劣势，并介绍了如何从 Mixin 迁出：[Mixins Considered Harmful](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html), July 13, 2016 by [Dan Abramov](https://twitter.com/dan_abramov)


#### 高阶组件(High-Order Components)

>What’s Next
I plan to use higher-order components in the next version of React DnD.
They don’t solve all the use cases for mixins, but come close. Don’t forget that the wrapper can pass arbitrary props to the wrapped component, even the callbacks. It’s possible that the higher-order components can be abused too, but unlike mixins they only rely on simple component composition instead of a bag of tricks and special cases.

[Mixins Are Dead. Long Live Composition](https://medium.com/@dan_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750#.xg5urzxad)

---

-- beta version -- by robin

