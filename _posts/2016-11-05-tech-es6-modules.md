---
layout: post
title: "ES6 模块的 17 个要点"
date: 2016-11-05 18:55:41 +0800
categories:
- Technology
tags:
- 'ECMAScript 6'
- JavaScript
---

模块是 ES6 中一个较为重要的特性，它也将成为未来 Web 项目模块开发的一个重要基础。本文在 ES6 基础上提炼出 17 个要点，能够帮助大家更好地理解 ES6 的模块机制！

<!-- more -->

JavaScript 模块已经存在很长一段时间了。但是，模块通过库的方式实现，没有被构建到语言之中。ES6 第一次把模块构建到 JavaScript 中。

ES6 的模块被存储在文件中，有两种方式从一个模块中导出，你可以任选其一或者两者同时使用。简单讲，一个文件对应一个模块，或者一个模块对应一个文件。

基本的的使用不加赘述，大家可以看看阮一峰老师的教程即可。以下，将阐述一些 ES6 模块使用中的一些注意点！

基本要点：

1. ES6 导出模块
2. 一个文件对应一个模块，一个模块对应一个文件。一个文件中多个 export 不是导出多个模块；
3. import 对所导入的模块是只读的(Imports are read-only views on exports)；

## ES6 要点摘要：

### 1. ES6 模块设计的目标：

1. 优先默认导出(Default exports are favored)。ES6 提供了便捷的默认导出语法。
2. 静态模块结构(Static module structure)。这个点让 ES6 失去了灵活性，但使得可以在编译时决定导出和导入(这也意味着不能在模块中使用条件导入、导出)。
3. 同时支持同步加载和异步加载。静态模块结构下，代码在执行之前就已经知道所有的导入、导出依赖，因此，模块在执行之前可以去加载依赖的模块。
4. 在模块间支持循环依赖(主要的设计目标)。在小型的项目中，可以通过仔细的设计来避免循环依赖，在大型项目则将变得不可避免。因此，是否支持循环依赖变得很关键。

### 2. 导出主要分为两种：

1.表达式导出：

声明的变量、函数表达式、字面量等通过 `export <expression>` 导出，直接导出值，导出语句的末尾需要加分号 ";"。

```
function foo () {} // 这是函数声明，可以在声明时导出，也可以如下代码中，以表达式的方式导出。

export 'abc';
export foo;
export foo();
export aVar;
export 5*7;
export {a: true};
// 直接导出值
```

```
export default <expression>;
// 上面代码等价于
const __default__ = <expression>;
export {__default__ as default};
```

2.声明导出：

导出函数声明、类声明、“generator 声明”时只导出***声明***，导出语句的末尾**不需要**加分号。

```
export function foo() {} // 无分号
export function *foo() {} 
export class Bar {}
// 以上等价的代码
export function () {}
export function *() {}
export class {}
```
导出匿名的 function、class 和命名的 function、class 的区别仅仅是 export 的操作数可以是命名的声明。

声明导出转换成表达式导出需要给声明块加一个***括号***，如下：

```
export (function () {})
export (class {})
```

### 3. 为什么存在两种风格的导出机制：

`export default const foo = 1, bar = 2;`，这是错误的代码。所以，要导出这两个变量时可以通过“表达式导出”的方式进行导出。

### 4. import 和 export 必须置于模块顶层代码中：

ES6 的模块是静态的(static)，不能条件式地 import 或者 export。同时，ES6 中的模块是平面的结构，因为模块的 imports 和 exports 都置于顶层(top level)作用域中。这样子会带来很多好处，比如：在代码编译时就可以知道模块之间的依赖。

这意味着，不能这样子导出：

```
if(isOdd) {
	import 'foo'; // 语法错误
}

if(isOdd) {
	export 'bar'; // 语法错误
}
```

import 是被吊起(hoisted)，这意味着语句会被移到当前命名空间的起始处。因此，import 写在顶层代码中的哪个地方都是可以的。

### 5. import 对所导入的模块是只读的：

1. 在 CommonJS 中，导入(imports)是模块导出值的复制值(同时 `require()`动作是同步的)。也就是说，在一个模块中一个值和这个值导出被复制后的值不是同一个，两者之间不存在连接(disconnected)。
2. 在 ES6 中，导入是对导出值的只读(read-only view)。因此，模块中的值和导出后的值是同一个，存在连接(live connection)，只是在导入的模块中对这个值是只读的。“只读”说明在导入的模块中不能直接修改被导入的值，如果要修改被导入的值，可以通过调用被导入模块的函数来达到目的。

举个栗子：

```
//------ lib.js ------
export let obj = {};

//------ main.js ------
import { obj } from './lib';

obj.prop = 123; // OK
obj = {}; // TypeError
```

上述代码说明：可以改变对象里面(obj 里面)的值，但是不能改变这个被导入的值(这个值是 obj)。


### 6. “import 对所导入的模块是只读的” 的好处：

1. 能够支持循环依赖。
2. 一个大的模块可以拆成若干个小模块时也可以运行，只要不尝试修改导入(import)的值。

### 7. ES6 支持循环依赖：

循环依赖：两个模块(A、B)分别相互导入两者之间模块，并在模块中调用两者之间的函数，而形成的循环调用。

现以 CommonJS 为例，请看以下代码：

```
//------ a.js ------
var b = require('b');
function foo() {
    b.bar();
}
exports.foo = foo;

//------ b.js ------
var a = require('a'); // (i)
function bar() {
    if (Math.random()) {
        a.foo(); // (ii)
    }
}
exports.bar = bar;
```

假设程序先调用了 b.js，在 (i) 行处导入 a 模块，在 b 模块成功导入 a 模块之前，b 模块不能够访问 a.foo(也就是说，在 a 模块完成加载后，b 模块中 `var a = require('a');` 代码才能执行完成); 而在 b 模块导入 a 模块时，a 模块需要先加载完成其自身的模块依赖，这时 a 模块需要执行 `var b = require('b');` 去加载 b 模块。

这样就产生了循环依赖。


而 ES6 自动地支持循环依赖！前面提到“import 对所导入的模块是只读的”，因而下面代码时可以执行的，执行过程中可以间接调用“导入的值”。

例如：

```
//------ a.js ------
import {bar} from 'b'; // (i)
export function foo() {
    bar(); // (ii)
}

//------ b.js ------
import {foo} from 'a'; // (iii)
export function bar() {
    if (Math.random()) {
        foo(); // (iv)
    }
}
```

### 8. 空的导入(empty import)：

对于空的导入(empty import)将直接执行这个模块中的代码。

### 9. 默认的导出(default export)是另外一种形式的命名导出(named export):

```
//------ module1.js ------
export default function foo() {} // function declaration!

//------ module2.js ------
function foo() {}
export { foo as default };
```

### 10. "default" 应作为导出的名字，不能作为变量名：

default 为 ES6 的保留字，不能将保留字作为变量名，但是把 default 用于模块导出(同时，default 在 ES5 中还可以用作属性名)。

在重复导出(Re-exporting)中，可以这样使用 default：

```
export { myFunc as default } from 'foo';
export { default as otherFunc } from 'foo';

// The following two statements are equivalent:
export { default } from 'foo';
export { default as default } from 'foo';
```

### 11. ES6 的模块加载 API(module loader API):

ES6 的模块采用了声明式语法进行工作，同时 ES6 也支持模块加载 API。模块加载 API 允许我们一定程度上控制模块的加载和工作，但该 API 不在 ES6 标准中，而作为一份独立的文档存在[*"JavaScript Loader Standard"*](https://whatwg.github.io/loader/)！模块加载 API 的标准还在制定中，下面所阐述的模块加载 API 的特性是试验性的，不是最终确定的：

1. 条件导入、导出模块；
2. 使用 `<script>` 标签；
3. 有许多的 hooks 贯穿模块加载的过程，以便开发调用。


### 12. 浏览器环境中的 ES6 模块：

在浏览器中支持 ES6 模块还在制定之中。在浏览器环境中，有两种入口：*脚本*和*模块*(scripts & modules)，其中的区别不加赘述，请直接[查看这里](http://exploringjs.com/es6/ch_modules.html#sec_modules-in-browsers)。

### 13. 模块默认导出(default-exporting)的细节：

```
export default 123;

// 上面这行代码等价于

const *default* = 123; // *not* legal JavaScript
export { *default* as default };
```

所以，当模块中存在 default 导出的时候，存在两个不一样的变量名，以防止命名冲突：

1. 本模块中的变量名：`*default*`;
2. 导出的变量名：`default`。

对于 function、generator、class 在默认导出时都是一样的：

```
export default function foo() {}

// 等价于

function foo() {}
export { foo as default };
```

### 14. 导出值的入口(export entries)：

export entries 在模块调用执行之前已经构建，webpack 可以基于这个在打包过程中进行优化。


### 15. 静态模块结构的优势：

1. 在打包时消除死代码(dead code)。在项目开发的过程中，我们可以借助 ES6 实现模块化开发；在部署的时候，可以把这些分散的模块集中打包集成(打包减少了网络请求的数量，这点并不时很关键，因为在 HTTP/2 中将会有所改变；打包压缩了代码，减少了代码的体积；在打包过程中，没有用到的代码将会被移除)。
2. 简洁高效的打包，不存在定制的打包格式(compact bundling, no custom bundle format)。ES6 模块可以被高效地合并，是因为所有的模块都被当作一个单独的作用域(single scope)(通过重命名变量来消除名冲突)。这些得益于 ES6 模块的两个特性：
	* 静态模块结构不存在模块的条件加载(但是仍可以通过把模块放到函数中来实现);
	* 导入对到导出的值只读，这意味着我们无需复制导出的值，而直接访问导出的值。
3. 更快的导入检索(faster lookup of imports):
	* 在 CommonJS 中，通过复制来导入，同时模块存在动态机制(导出、导入)，因而在查找属性的时候更加慢；
	* ES6 模块时静态的，意味着在模块执行之前就已经知道导入的值是什么，可以优化访问。
4. 变量检查。同样得益于 ES6 的静态模块结构，我们可以在执行前进行导入、导出的变量检查。
5. 为宏指令做准备(ready for macros)。macros 会是 JavaScript 发展蓝图中的一个 milestone。目前可以通过一些第三方的库来实现宏定义。
6. 静态类型(static types)定义.
7. 静态结构为支持其他编译型语言提供了可能。

更多阅读：[Static module resolution](http://calculist.org/blog/2012/06/29/static-module-resolution/)

### 16. 不能在导入时使用结构(destructuring):

import 的语法只是比较像解构，但两者不相同(static\ imports are views):

```
// Illegal syntax:
import { foo: { bar } } from 'some_module';
```

### 17. 不能通过 `eval()` 来调用模块：

ES6 中的模块必须置于顶层作用域中，`eval()` 接收脚本script，不接收模块。对于 `eval()` 来说，模块是比 `eval()` 更高的结构。

----

#### 疑问：

在调用模块之前已经构建的 export entries，如何分析两个模块是否一样？

----

其他阅读：

模块如何调用执行？[ModuleEvaluation() Concrete Method](http://www.ecma-international.org/ecma-262/6.0/#sec-moduleevaluation)

----

参考：

[Modules - Exploring ES6](http://exploringjs.com/es6/ch_modules.html)

