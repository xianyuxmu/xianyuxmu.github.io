---
layout: post
title: webpack 2 notes
date: 2017-02-27 21:28:00 +0800
categories:
- Tech
tags:
- webpack 2

---


## webpack 2 Documentation excerpts

### Entry and Context

- The **entry** object is where webpack looks to start building the bundle. The current directory is used by default.
- The **context** is an absolute string to the directory that contains the entry files. At this point the application starts executing. If an array is passed all items will be executed.

### Output

The top-level ***output*** key contains set of options instructing webpack on how and where it should output your bundles, assets and anything else you bundle or load with webpack.

> Only used when `target` is web, which uses JSONP for loading on-demand chunks, by adding script tags.


### Module

These options determine how the different types of modules within a project will be treated.

### Rule

A `Rule` can be separated into three parts — Conditions, Results and nested Rules.

There are two input values for the conditions:

- **The resource:** An absolute path to the file requested. It's already resolved according the resolve rules.
- **The issuer:** An absolute path to the file of the module which requested the resource. It's the location of the import.

**Example:** When we import "./style.css" from app.js, the resource is /path/to/style.css and the issuer is /path/to/app.js.

### Resolve

These options change how modules are resolved.

### Plugins


### Externals

`externals` configuration in webpack provides a way of not including a dependency in the bundle. 

### Module Resolution

A resolver is a library which helps in locating a module by its absolute path. 

### Loaders

webpack enables use of loaders to preprocess files. This allows you to bundle any static resource way beyond JavaScript. You can easily write your own loaders using Node.js.

----

## Comparison between webpack 2 & Rollup

[webpack-2-vs-rollup](https://github.com/raphamorim/webpack-2-vs-rollup) - A demo


### Rollup

- Well design with ES6's new feature([bindings](https://github.com/rollup/rollup/wiki/Bindings) and [cycles](https://github.com/rollup/rollup/wiki/Cycles)). Bundling the app or library using ES2015 and reducing bundling size with tree-shaking.
- Configurations are more easily. 

Rollup may be fading in trend and its designing long lasts. Still in developing with no stable version, and webpack 2 has been implementing its feature. The trend is going down in Github while webpack 2 going up.

----

## Related posts

- [webpack 2 and beyond](https://medium.com/webpack/webpack-2-and-beyond-40520af9067f#.z46x4n19m) - A short and useful introduction of webpack 2 which must read.
- [What's new in webpack 2](https://gist.github.com/sokra/27b24881210b56bbaff7#resolving-options)
- [eslint-loader doesn't check if additional .eslintrc exists #129](https://github.com/MoOx/eslint-loader/issues/129) - eslint's usages in webpack 2, another post related: [Configuring ESLint](http://eslint.org/docs/user-guide/configuring), [eslint-loader](https://github.com/MoOx/eslint-loader)
- [Env preset](https://babeljs.io/docs/plugins/preset-env/#options) - Stop Babel parsing `import` & `export`

----

Robin on Mar. 01, 2017