---
layout: post
title: JavaScript Modules and Bundlers
date: 2017-03-01 19:16:00 +0800
categories:
- Tech
tags:
- Bundler

---

## Modules

### Why modules

- Pollution of global namespace - hand to handle scope.
- Code Reusability - copy ang paste?
- Dependency Resolution - hard to handle nested dependencies.

### Modules' History

- 2009 - [CommonJS](http://www.commonjs.org/) was born when JavaScript being brought  to server side. 
- 2011 - [AMD(Asynchronous Module Definition)](https://github.com/amdjs/amdjs-api). CommonJs is synchronous, and there some problems when using it.
- 2010 - [RequireJS](http://requirejs.org/). It is a JavaScript ***module loader*** based on AMD.
- 2011 - [Browserify](http://browserify.org/). AMD syntax is too verbose, list of dependencies in the array must match the list of arguments in the function and loading many small files can degrade the performance under HTTP 1.1. So ***module bundler*** Browserify is coming.
- 2011 - [UMD(Universal Module Definition)](https://github.com/umdjs/umd) - used to handle different module types(i.e., global module object, CommonJS and AMD) through if/esle.
- 2015 - ES6 modules defined in language spec, now modules are part of JavaScript.
- now - ? using latest or stable, it is up to you.

----

## Bundlers

### Why bundle modules

- Fewer files for more easily distributing.
- Compressing file's size.
- Eliminating unused codes.
- Obfuscating codes.
- Processing codes before releasing.

### How bundlers work

A JavaScript bundler start from entry file, then recursively imports until whole library in a single file.


### Bundling tools List

| Timeline                 | Bundling Tools | First Release | Last Release |
| ------------------------ | ----------- | ----------- | ----------- |
| 2011 - 2016 | browserify | [Aug 30, 2011](https://github.com/substack/node-browserify/releases/tag/1.4.4) | [May 6, 2016](https://github.com/substack/node-browserify/releases/tag/13.0.1) |
|2015 - 2017 with no stable release|[Rollup](https://github.com/rollup/rollup)| [May 21, 2015](https://github.com/rollup/rollup/releases/tag/v0.3.1) | [Jan 15, 2017](https://github.com/rollup/rollup/releases/tag/v0.41.4) |
|2013 - 2016 | [webpack 1](https://webpack.github.io/) | [Dec 20, 2013](https://github.com/webpack/webpack/releases/tag/v1.0.0-beta2) | [Aug 18, 2016](https://github.com/webpack/webpack/releases?after=v2.1.0-beta.22) |
|2015 - now | [webpack 2](https://webpack.js.org/) | [Nov 2, 2015](https://github.com/webpack/webpack/releases/tag/v2.0.0-beta) ||

#### Browserify

[Browserify](https://github.com/substack/node-browserify/) - browser-side require() the node.js way, `require('modules')` in the browser, and a node-style require() to organize your browser code and load modules installed by npm.

#### webpack 1

[webpack 1](https://webpack.github.io/) - webpack v1 is deprecated.

#### webpack 2

[webpack 2](https://webpack.js.org/) - Reading its new feature from [webpack 2 and beyond](https://medium.com/webpack/webpack-2-and-beyond-40520af9067f#.z46x4n19m) & [official introduction](https://github.com/webpack/webpack#introduction)

- 🙅 ES6 Support 🙅‍
- 🌳 Tree shaking 🌳  Depending on the static structure of ES6 modules (imports and exports can’t be changed at runtime) to detect which exports are unused.
- And more milestones are ahead!

#### Rollup

[Rollup](http://rollupjs.org/) - A next-generation ES6 module bundler, cliking link ahead the line to see the demos.

- **Tree-shaking** - Including the code you actually need, eliminating unused library code with tree-shaking. The resulting bundle size <= other tools'.
- Use via JavaScript API or a Command Line Interface.
- Plugin
- Start from entry file, then recursively imports until whole library in a single file.

### Future's

- HTTP/2
- Native JavaScript modules

----

## References

- [Tree-shaking with webpack 2 and Babel 6](http://www.2ality.com/2015/12/webpack-tree-shaking.html) - Intros to tree-sharking.
- [Brief history of JavaScript Modules](https://medium.com/@sungyeol.choi/javascript-module-module-loader-module-bundler-es6-module-confused-yet-6343510e7bde)
- [The future of bundling JavaScript modules](http://www.2ality.com/2015/12/bundling-modules-future.html)
- [The Evolution of JavaScript Modularity](https://github.com/myshov/history-of-javascript/tree/master/4_evolution_of_js_modularity)


----

Robin on Fisrt day of March, 2017 Wednesday