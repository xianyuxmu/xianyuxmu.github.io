---
layout: post
title: 'Redux: reducer 和 combineReducers() 使用注意点'
date: 2017-01-11 20:24:48 +0800
categories:
- Technology
tags:
- Redux
---

## 前言

使用 React + [Redux](http://redux.js.org/) 构建大型项目时，通常会采用分模块开发，而每个模块也会被拆做许多的单页。在单个模块中，我们需要通过分割 reducer 的形式来管理 状态，然后通过 `combineReducers()` 来合并这些 reducer，再通过 `createStore()` 创建 `store`。示例代码，请直接访问文档：[Using combineReducers](http://redux.js.org/docs/recipes/reducers/UsingCombineReducers.html#defining-state-shape)。

单个模块中的多个页面，共享一个 `store`，不同的单页可以通过 `store.getState()` 的形式来获取到整个 `store`，然后通过 `store` 访问不同页面对应的 `reducer`。

在跨页面中通过 `store` 使用其它页面的 `reducer` 时，我们需要了解 `combineReducers()` 是起什么作用？怎么作用？

## `reducer` 以及 `combineReducers()`

简单讲，`combineReducers()` 把多个 `reducer` 函数合并成一个，并返回一个新的 `reducer` 函数。

原始 `reducer` 要点：

1. `reducer` 描述一个状态模型(State Shape)。在 `Redux` 下，应用的状态存储在一个对象类型的数据结构中。尽管你可以在运行过程中在这个对象中添加其他数据，但是最好在写代码之前设计好这个状态模型、以更加直观。
2. `reducer` 处理 Action(Handling Actions)。总体的规则就是 `(previousState, action) => newState`，一个 `Action` 触发，根据这个 Action 的类型进行相应的操作(请求数据、处理数据等)，然后对 `previousState` 进行修改，并返回一个新的状态 `newState`。原文的描述更佳直观：*"Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API calls. No mutations. Just a calculation."*

在项目中，常常要处理很多的 Action，同时 `reducer` 中的 State 也会变得很大，这时候就需要对 Action 和 `reducer` 进行分割。对于 `reducer` 进行分割后，我们常常需要 `combineReducers()` 合并成一个新的 `reducer`。

[combineReducers()](http://redux.js.org/docs/api/combineReducers.html) 要点：

1. `combineReducers()` 不是必须的。你可以选择不使用。而如果使用 `combineReducers()` 时，`reducer` 的写法要遵循一定的规则，详见：[combineReducers(reducers)](http://redux.js.org/docs/api/combineReducers.html)。
2. 每个 diapatch 的 `Action` 会调用 `combineReducers(REDUCERS)` 中所有的 `REDUCERS` 进行处理，一个 Action 会被每个 `REDUCER` 都处理一遍。
3. `combineReducers()` 可以多层次地组合。例如： `ABC = combineReducers(A, b, C)`，然后 `ABCDEF = combineReducers(ABC, DEF)`。

## `combineReducers()` 使用注意点：

1. 在 ES6 中，reducer 函数为`(state = initialState, action = {}) => {}`，记得在 switch 语句 default 中返回 `return state;` 而不是 `return initialState;` ，否者在跨页面跳转时上一个组件的 reducer 会变为 。 假设有 reducerA、reducerB 通过 `combineReducers()` 合并到一块，成为 **BIGREDUCER**(我自己命名的)。我们从 reducerA 对应的组件 A 跳转到 reducerB 对应的组件 B 时，这个 `BIGREDUCER` 中 reducerA 的状态会变成 initialState。因为每个 Action 都会经过 `BIGREDUCER` 中的每一个子 reducer(包括 reducerA 和 reducerB)，这时候，这个 Action 是由 B 组件发出的，而在 reducerA 的 switch 中匹配不到这个 case，执行了 `return initialState;`，于是就出现了 reducerA 的 state 在切换到 B 组件的时候被重新初始化为 initialState 的情况。

2. 注意 `Action.type` 的命名，一个 Action 可能会被多个 `reducer` 匹配到。比如：一个 `Action.type` 为 'QUERY_DATA' 的 Action 由 A 组件发出，本来应该只有 A 组件对应的 reducerA 来处理，但是 reducerB 中也匹配到了，这是 reducerB 组件也会处理一次，造成数据串改。

----

finished on January 12, 2017 Thursday
