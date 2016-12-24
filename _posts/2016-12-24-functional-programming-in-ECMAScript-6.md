---
layout: post
title: 'ES6 下的函数式编程'
date: 2016-12-24 13:19:00 +0800
categories:
- Technology
tags:
- 函数式编程
- JavaScript

---

在编程领域，常见的编程方式有函数式编程(Functional Programing)和面向对象编程(Object-oriented Programing)。函数式编程侧重于过程，即加工数据；面向对象编程侧重于封装数据和对数据操作。本文将只对函数式编程进行介绍。关于函数式编程的基本概念请先阅读一下文章：

[函数式编程初探](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html) - 记得看评论

[**Functors, Applicatives, And Monads In Pictures**](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)，中文版***[Functor, Applicative, 以及 Monad 的图片阐释](http://jiyinyiyong.github.io/monads-in-pictures/)***


## 函数式编程的要点

1. 函数同普通变量一样，可以当作参数被传递和调用。
2. 函数的确定性。传入固定类型的参数，函数加工出一致的返回值。
3. 类似数学中的函数，不同函数可以嵌套成新的表达式。
4. 函数主要功能是根据传入的“东西”，进行一系列的加工，返回一件新的“东西”。

## 几个重要的概念

1. 柯里化(Currying)：是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。 - [Wikipedia](https://en.wikipedia.org/wiki/Currying)。简单说就是「接受一个参数；返回一个值；」。柯里化是[高阶函数](https://zh.wikipedia.org/wiki/%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0)的一种。
2. 这几个自己看，😄[**Functors, Applicatives, And Monads In Pictures**](http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html)。

## ECMAScript 6 下的函数式编程

### ECMAScript 6 函数

函数式编程可用于 ECMAScript 6 非异步的情况下的函数来处理数据；而在异步的情况下要结合 Generator 函数或者 Promise 对象才能得以实现。

### redux applyMiddlewares()

什么是中间件？

> 是提供系统软件和应用软件之间连接的软体。 - Wikipedia

而英文的描述更加形象：

> Middleware is computer software that provides services to software applications beyond those available from the operating system. It can be described as "software glue". - [Wikipedia](https://en.wikipedia.org/wiki/Middleware)

`applyMiddlewares()` 源代码：

``` javascript
export default function applyMiddleware(...middlewares) {
  return (createStore) => (reducer, preloadedState, enhancer) => {
    var store = createStore(reducer, preloadedState, enhancer);
    var dispatch = store.dispatch;
    var chain = [];

    var middlewareAPI = {
      getState: store.getState,
      dispatch: (action) => dispatch(action)
    };
    chain = middlewares.map(middleware => middleware(middlewareAPI));
    dispatch = compose(...chain)(store.dispatch);

    return {...store, dispatch}
  }
}
```

> 上面代码中，所有中间件被放进了一个数组 `chain`，然后嵌套执行，最后执行 `store.dispatch`。可以看到，中间件内部（`middlewareAPI`）可以拿到 `getState` 和 `dispatch` 这两个方法。

我们得出几个结论：

1. redux 中间件的格式：`({dispatch, store}) => (next) => (action) => {}`。
2. redux 中间件是对初识 `dispatch` 的包装。在 `applyMiddlewares()` 函数中，调用 `compose()` 函数来合并中间件 `dispatch = compose(...chain)(store.dispatch)`。
3. 中间件是从左到右执行的。从 `middlewares.map(middleware => middleware(middlewareAPI))` 中可以得知执行顺序。
4. 在中间件中调用下一个中间件使用 `next(action)` 方式进行调用，等调用结束后，继续执行 `next(action)` 之后的代码。中间件有点像洋葱一层包着一层，中间件的执行就像从洋葱的一面穿过另外一面。最外层最先执行也最晚完成，最里层最晚执行缺最早完成。但不一定是这种情况，有可能执行到一半时调用了 `dispatch(newAction)`，这时候就要直接跳出当前中间件的执行，重新开始执行“穿过洋葱头”。
5. 中间件的返回的值只能被上一层的中间件捕捉到。例如，redux 中间件的执行是从左往右的，左边先执行的中间件 A 中调用下一个中间件 B，B 又调用了中间件 C，因此 B 能够获得到 C 中间件执行完的返回值，而 A 可以获得到 B 的返回值，A 却不能直接获取到 C 的返回值。

----

### redux-thunk 中间件

在实际的业务场景中，我们常常需要串行执行两个 Action 操作，也就是说，执行完 `Action A` 后再执行 `Action B`。在 react-redux 中，Action 的触发是串行的，例如：

``` javascript
dispatch({type: 'ActionA'}); // A
dispatch({type: 'ActionB'}); // B
```

实际的调用中，`Action A` 比 `Action B` 更早执行，但对于异步的 JavaScript 来说，`Aciton A` 不一定比 `Action B` 更早完成。

但是我们又要要求在 `Aciton A` 执行完之后在 调用 `Aciton B`，这是就需要自己手动写个 `Promise` 对象进行处理。而 `redux-thunk` 则允许我们把 Action 写成函数的形式，在执行到中间件 `redux-thunk` 时，直接调用执行 `Action()`。

`redux-thunk` 中间件对 `store.dispatch` 进行改造，使得 `store.dispatch` 可以接收**函数**作为参数，这样就可以在 `typeof action == 'function'` 的情况下使用直接执行 `action()`。代码如下：

``` javascript
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

----

### Koa 中间件

> 这里将的是 Koa1，不是 Koa2。

Koa 中间件的思路和 redux 中间件的思路极为相似，而具体的实现细节中，Koa 主要借鉴 ES6 中 generater 的设计思想。

下面我们先看 `app.use()` 的代码：

``` javascript
/**
 * Use the given middleware `fn`.
 *
 * @param {GeneratorFunction} fn
 * @return {Application} self
 * @api public
 */

app.use = function(fn){
  if (!this.experimental) {
    // es7 async functions are not allowed,
    // so we have to make sure that `fn` is a generator function
    assert(fn && 'GeneratorFunction' == fn.constructor.name, 'app.use() requires a generator function');
  }
  debug('use %s', fn._name || fn.name || '-');

  // fn 是 generator function
  this.middleware.push(fn);
  return this;
};
```
在看一眼 `app.listen()` 的代码：

``` javascript
/**
 * Shorthand for:
 *
 *    http.createServer(app.callback()).listen(...)
 *
 * @param {Mixed} ...
 * @return {Server}
 * @api public
 */

app.listen = function(){
  debug('listen');

  // 传入 app.callback()，然后使用 createServer 创建服务，
  var server = http.createServer(this.callback());
  return server.listen.apply(server, arguments);
};
```
再瞧一眼 `app.callback()` 的代码：

``` javascript
/**
 * Return a request handler callback
 * for node's native http server.
 *
 * @return {Function}
 * @api public
 */

app.callback = function(){
  if (this.experimental) {
    console.error('Experimental ES7 Async Function support is deprecated. Please look into Koa v2 as the middleware signature has changed.')
  }

  var fn = this.experimental
    ? compose_es7(this.middleware)
    : co.wrap(compose(this.middleware)); // 把中间件串起来
  var self = this;

  if (!this.listeners('error').length) this.on('error', this.onerror);

  return function(req, res){
    res.statusCode = 404;
    var ctx = self.createContext(req, res);
    onFinished(res, ctx.onerror);
    fn.call(ctx).then(function () {
      respond.call(ctx);
    }).catch(ctx.onerror);
  }
};
```

`app.callback()` 代码中比较关键的是 `compose(this.middleware)` 这句。接着我们打开 `koa-compose` 看看 `compose()` 是如何实现的：

``` javascript
/**
 * Expose compositor.
 */

module.exports = compose;

/**
 * Compose `middleware` returning
 * a fully valid middleware comprised
 * of all those which are passed.
 *
 * @param {Array} middleware
 * @return {Function}
 * @api public
 */

function compose(middleware){
  return function *(next){
    if (!next) next = noop();

    var i = middleware.length;

    while (i--) {
      next = middleware[i].call(this, next);
    }

    return yield *next;
  }
}

/**
 * Noop.
 *
 * @api private
 */

function *noop(){}
```

好的，看完这几段代码之后就可以来看看 Koa 中间件的执行过程：

1. `app.use(fn)` 执行 `this.middleware.push(fn)`，然后返回 `this`，中间件的执行顺序是`FILO`的。也就是，最先开始执行的最晚执行结束，最晚开始执行的最早执行结束;
2. 通过 `next` 将中间件连接起来起来，通过最外层的 generator function 触发开始整个执行过程；
3. 在 generator function 内部调用另外一个 generator function 必须使用 `yield (generator function)`；
4. `yield` 只能用于 ***function, promise, generator, array, or object***，执行 `yield 1;` 将会报错；
5. Koa 中间件的执行类似 Django 的中间件论坛中出现过的一张图，我取名为洋葱图：

![洋葱图](/uploads/tech/onion.png)
洋葱图

----

参考及资料：

[Redux 入门教程（二）：中间件与异步操作](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)

[redux中间件实战](https://github.com/jabez128/jabez128.github.io/issues/7)

[KOA 源码阅读系列（一） - 理解 KOA 中间件的执行](https://gold.xitu.io/entry/570946095bbb5000510466bc)

[深入浅出 Koa2](https://github.com/berwin/Blog/issues/9)

[JS函数式编程指南](https://www.gitbook.com/book/llh911001/mostly-adequate-guide-chinese/details)

----

December 24, 2016 Christmas Eve

Wangjing CBD.