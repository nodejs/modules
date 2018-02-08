# Resources

## A collection of items related to Node.js module work

### Reading
* [The Current State of Implementation and Planning for ESModules](https://medium.com/the-node-js-collection/the-current-state-of-implementation-and-planning-for-esmodules-a4ecb2aac07a)
* [ESM modules in node: npm edition](https://gist.github.com/ceejbot/b49f8789b2ab6b09548ccb72813a1054)

### Meetings
* [Meeting availability #1](https://doodle.com/poll/cqavkzwxtxzccs4z)
* [Meeting availability #2](https://doodle.com/poll/vdb8cgz48q3zzt2t)

### Prior Art

Following a list of ESM and CommonJS relevant projects.

#### Transpilers
  * [Babel](http://babeljs.io) transpiled ES6+ including modules downgraded to CommonJS with an `exports.default` strategy
  * [TypeScript](https://www.typescriptlang.org) transpiled classes including ES6 like/inspired modules

#### Bundlers
  * [Webpack](https://webpack.js.org) multi purpose bundler compatible with CommonJS `exports.default` strategy. Actually diverging from npm resolution preferring a `browser` field in the `package.json` instead of actual CommonJS.
  * [Rollup](https://rollupjs.org) next-generation ES6 module bundler with tree shaking capability
  * [Parceljs](https://parceljs.org/) multi purpose bundler focused on zero config and performance
  * [Browserify](http://browserify.org) the CommonJS module bundler that brought NodeJS to the Web

#### Compilers
  * [Reify](https://www.npmjs.com/package/reify) compiles ESM syntax to CommonJS, supporting live bindings without rewriting imported variable names
  * [@std/esm](https://github.com/standard-things/esm) is a fork of Reify focused on following Node's ESM support roadmap while providing a bridge from the CJS/ESM usage of today to the ESM of tomorrow.

#### Runtime Utilities
  * [cjs4esm](https://github.com/WebReflection/cjs4esm#cjs4esm--) brings CommonJS modules and `require` to ESM compatible environments such [SpiderMonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey), [JSC](https://trac.webkit.org/wiki/JavaScriptCore), or ESM compatible browsers
  * [import.js](https://github.com/WebReflection/import.js#importjs) is a dynamic `import(...)` polyfill
  * [SpiderMonkey](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/SpiderMonkey) [ESM Loader](https://searchfox.org/mozilla-central/source/js/src/shell/ModuleLoader.js)
  * [CGJS](https://github.com/cgjs/cgjs#cgjs-) is a CommonJS runtime for [GJS](https://wiki.gnome.org/Projects/Gjs)
