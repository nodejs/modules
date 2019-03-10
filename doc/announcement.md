# Announcing a New `--experimental-modules`

Back in 2017, Node.js 8.9.0 shipped experimental support for [ECMAScript modules](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/), known for their `import` and `export` statements. This support was behind the flag `--experimental-modules`.

A lot has happened since then. All major browsers [now support](https://caniuse.com/#feat=es6-module) ECMAScript modules (ES modules) via `<script type="module">`. Various projects have sprung up to make NPM packages with ES module sources available for use in browsers via `<script type="module">`. Support for [import maps](https://github.com/WICG/import-maps), which bring to browsers Node-style package names in `import` statements, [is coming to Chrome](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/qYeQFqgFOyA/rXJapjMaEAAJ).

All this progress toward adoption of ES modules has increased the urgency for Node.js to ship its support for ES modules. Especially now that there are other runtimes and environments where ES modules are in use, it’s more important than ever that Node.js join the JavaScript standard.

Yet Node’s initial ES modules support has remained in an experimental state, largely due to strong pushback from the community to several decisions that were made in its design. In response, the [Modules Team](https://github.com/nodejs/modules) was formed to take a fresh look at implementing support for ES modules in Node.js. This has led to a [new implementation](https://github.com/nodejs/ecmascript-modules) for supporting ES modules, that we are pleased to announce will ship as part of Node.js 12. It will replace the old `--experimental-modules` implementation, behind the same flag. We hope that this new implementation addresses many of the community’s concerns, and can ship as part of Node.js proper, without a flag, before Node.js 12 reaches [LTS status](https://nodejs.org/en/about/releases/) in October 2019.

## What’s in `--experimental-modules`

Like the previous version, this new `--experimental-modules` adds support to Node.js for the following:

- [ES2015 `import` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) can reference JavaScript files that are themselves written using ES module syntax. Files can be referenced as relative URLs (`'./file.mjs'`), absolute `file://` URLs (`'file:///opt/app/file.mjs'`), package names (`'es-module-package'`) or paths within packages (`'es-module-package/lib/file.mjs'`).

- `import` statements that reference ES module files can specify both the default export (`import _ from 'es-module-package'`) or named exports (`import { shuffle } from 'es-module-package'`). All Node.js built-in packages like `fs` and `path` support both default and named exports.

- `import` statements that reference CommonJS files (all current JavaScript code written for Node.js, [using `require` and `module.exports`](https://nodejs.org/api/modules.html)) can use the CommonJS default export (`import _ from 'commonjs-package'`) only. _This is a work in progress and may change in the future._

- [`export` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) in ES module files can specify both default and named exports for `import` statements to reference.

- Dynamic `import()` expressions can be used to import ES modules into either CommonJS or ES module files. Note that `import()` returns a promise.

- `import.meta.url` provides the `file://` URL of the current ES module file.

- Node.js can be run with an ES module file as a program’s initial entry point.

- Loaders can be written to modify Node’s runtime behavior with respect to ES modules. _This is still very much a work in progress._

So if that’s what’s carried over from the previous implementation, what’s new?

## `import` and `export` syntax in `.js` files

Yes, we heard [you](https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md). And [you](https://gist.github.com/ceejbot/b49f8789b2ab6b09548ccb72813a1054). And [you](https://twitter.com/maybekatz/status/1062473765865512961). You can now use `import` and `export` syntax in `.js` files.

The `.mjs` extension is still supported, and still always explicitly means that its file uses the Module parse goal (and therefore is an ES module). We also created a new `.cjs` file extension, to explicitly signify that a file should be treated as CommonJS. (CommonJS is the other module system that Node.js supports, with `require` and `module.exports`.) These two extensions allow individual files to define how they should be loaded, as ES modules or as CommonJS.

As it turns out, a lot of people didn’t like needing to use a new file extension for JavaScript. Browsers don’t require it, tools that build and process JavaScript have issues with it, and languages that transpile into JavaScript such as TypeScript and CoffeeScript don’t necessarily know when to save their output as `.js` or as `.mjs`. So while `.mjs` and `.cjs` have use cases that only they can solve (ES module and CommonJS files side-by-side in the same folder, loose JavaScript files outside of any package) the modules group searched for solutions other than `.mjs` to let Node.js know when to interpret a file as an ES module.

We came up with two things: a `"type"` field in `package.json` and a `--type` flag. Their naming was inspired by browsers’ `<script type="module">`.

### `package.json` `"type"` field

Add `"type": "module"` to the `package.json` for your project, and Node will treat all `.js` files in your project as ES modules.

If some of your project’s files use CommonJS and you can’t convert your entire project all at once, you can either rename those files to use the `.cjs` extension or put them in a subfolder containing a `package.json` with `{ "type": "commonjs" }`, under which all `.js` files are treated as CommonJS.

For any file that Node.js tries to load, it will look for a `package.json` in that file’s folder, then that file’s parent folder and so on upwards until it reaches the root of the volume. This is similar to [how Babel searches for `.babelrc` files](https://babeljs.io/docs/en/config-files#file-relative-configuration). This new approach allows Node.js to use `package.json` for package-level metadata and configuration, similar to how it is already used by Babel and other tools.

### `--type` flag

Use `--type=module`, or the shorthand `-m`, to run a `.js` file as an ES module.

When running a file, e.g. `node file.js`, Node follows an algorithm to determine if it should load the file as CommonJS or as an ES module. First it looks for an explicit file extension (`.mjs` or `.cjs`); then it looks for a `"type"` field in the nearest parent `package.json`; and finally it looks at the `--type` flag. The `--type` flag can be `--type=module` (or `-m`) or `--type=commonjs`. You can also set this flag in `NODE_OPTIONS` if you’d like to change how Node behaves systemwide.

This flag provides a way to support ES module syntax for input via `--eval`, `--print` or `STDIN`.

## Mandatory file extensions

In the new `--experimental-modules`, file extensions are mandatory in `import` statements: `import './file.js'`, not `import './file'`. However, the CommonJS-style automatic extension resolution behavior (`'./file'`) can be enabled via a new flag, `--es-module-specifier-resolution=node`. (Its inverse, the default, is `--es-module-specifier-resolution=explicit`.) Package names are still just package names, e.g. `import fs from 'fs'`.

This change isn’t because we’re trying to annoy an entirely new constituency now that `.mjs` is no longer required. We recognize that there are years’ worth of JavaScript out there written in the extensionless style, and this will feel like a huge breaking change for all that code. That’s why we’re providing the `--es-module-specifier-resolution=node` flag, and this is also subject to change before `--experimental-modules` itself ships unflagged.

We’re making this change to align with browsers, where [extensions are required](https://github.com/WICG/import-maps#extension-less-imports). Part of the reason for Node.js to support ES modules in the first place is because ES modules are the standard, and the point of aligning to standards is to increase compatibility and interoperability between systems. In this case, Node.js supporting ES modules means that more JavaScript code will be capable of running in either Node or in browsers, without requiring some kind of translation process to make the code compatible with one or the other. Of course, it will always remain possible for users to write code that can’t function cross-platform (JavaScript referencing `window`, or importing `fs`, for example) but by aligning as much as we can, we encourage code to be cross-platform by default.

Also, [import maps may bring support for automatic extension resolution to browsers](https://github.com/WICG/import-maps#extension-less-imports). It would be premature for Node to implement its own solution to supporting extensionless specifiers while efforts are underway to provide a cross-platform standard. Again, this is subject to change.

## `module.createRequireFromPath`

The “CommonJS globals” `require`, `exports`, `module.exports`, `__filename`, `__dirname` are undefined in ES modules. However, [`module.createRequireFromPath()`](https://nodejs.org/api/modules.html#modules_module_createrequirefrompath_filename) can be used to create a CommonJS `require` function to be used in an ES module context.

## `import` for JavaScript only

The previous `--experimental-modules` allowed `import` statements of JSON and native modules. This has been removed; you may use `module.createRequireFromPath()` for these.

There is ongoing work in the standards space to cover JSON and WASM and other future potential module types. Node.js will add support for these in compliant ways when the standard is finalized.

## ES module code in packages

_This is a work in progress and subject to change._ You can create a package with ES module sources by using the `package.json` `"main"` field to point to an ES module package entry point. Node will know to load it as an ES module if the file ends in `.mjs` or if the `package.json` also contains `"type": "module"`.

Currently it is not possible to create a package that is usable both via `require` (as CommonJS) and via `import` (as an ES module). There are efforts underway to address this, and may involve changes to the above (especially concerning `"main"`). Please do not publish any ES module packages intended for use by Node.js until this is resolved.

## Coming soon

All of the above is shipping as part of `--experimental-modules` in Node.js 12. On our [road map](https://github.com/nodejs/modules/blob/master/doc/plan-for-new-modules-implementation.md) for improvements before the `--experimental-modules` flag is hopefully dropped in October 2019, when Node.js 12 reaches LTS status:

- **Better loaders.** The current loaders code is a holdover from the previous `--experimental-modules` and doesn’t fully integrate with the new system. There is much work to be done here.

- **Dual CommonJS/ES module packages.** We want to provide a standard way for package authors to publish a package that can be both `require`d into CommonJS or `import`ed into an ES module.

- **Easier `require`.** Using `module.createRequireFromPath()` involves a lot of boilerplate. We hope to provide a simpler way to use `require` in ES module code.

- **Package path maps.** Along the lines of the browser [import maps proposal](https://github.com/WICG/import-maps), we would like to support paths to files within packages. This would allow things like `import sdk from 'some-service/sdk'` to have `'some-service/sdk'` map to something like `./src/sdk/index.mjs`.

- **Automatic entry point module type detection.** This would provide a way for running `.js` files as either CommonJS or ES modules based on whether `import` or `export` statements appear in the source code of the file.

That’s it! We hope you enjoy this new `--experimental-modules`, and we look forward to hearing your feedback. The modules team’s work is public at https://github.com/nodejs/modules.
