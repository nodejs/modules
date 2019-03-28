# Announcing a new `--experimental-modules`

Back in 2017, Node.js 8.9.0 shipped experimental support for [ECMAScript modules](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/), known for their `import` and `export` statements. This support was behind the flag `--experimental-modules`.

A lot has happened since then. All major browsers [now support](https://caniuse.com/#feat=es6-module) ECMAScript modules (ES modules) via `<script type="module">`. Various projects have sprung up to make npm packages with ES module sources available for use in browsers via `<script type="module">`. Support for [import maps](https://github.com/WICG/import-maps), which bring to browsers Node.js-style package names in `import` statements, [is coming to Chrome](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/qYeQFqgFOyA/rXJapjMaEAAJ).

All this progress toward adoption of ES modules has increased the urgency for Node.js to ship its support for ES modules. Now that there are other runtimes and environments where ES modules are in use, it’s more important than ever that Node.js support this JavaScript standard.

Node.js’s initial ES modules support remained in an experimental state in order to allow the community time to provide feedback on that design. The [Modules Team](https://github.com/nodejs/modules) was formed to act on this feedback and ship first-class support for ES modules in Node.js. This has led to a [new implementation](https://github.com/nodejs/ecmascript-modules) for supporting ES modules, that we are pleased to announce will ship as part of Node.js 12. It will replace the old `--experimental-modules` implementation, behind the same flag. We hope that this new implementation addresses many of the community’s concerns, and can ship as part of Node.js proper, without a flag, before Node.js 12 reaches [LTS status](https://nodejs.org/en/about/releases/) in October 2019.

## What’s in `--experimental-modules`

Like the previous version, this new `--experimental-modules` adds support to Node.js for the following:

- [ES2015 `import` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) can reference JavaScript files that are themselves written using ES module syntax. Files can be referenced as relative URLs (`'./file.mjs'`), absolute `file://` URLs (`'file:///opt/app/file.mjs'`), package names (`'es-module-package'`) or paths within packages (`'es-module-package/lib/file.mjs'`).

- `import` statements that reference ES module files can specify both the default export (`import _ from 'es-module-package'`), named exports (`import { shuffle } from 'es-module-package'`) and namespace exports (`import * as fs from 'fs'`). All Node.js built-in packages like `fs` and `path` support all three types of exports.

- `import` statements that reference CommonJS files (all current JavaScript code written for Node.js, [using `require` and `module.exports`](https://nodejs.org/api/modules.html)) can use the CommonJS default export (`import _ from 'commonjs-package'`) only. _This is a work in progress and may change in the future._

- [`export` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) in ES module files can specify both default and named exports for `import` statements to reference.

- Dynamic `import()` expressions can be used to import ES modules from either CommonJS or ES module files. Note that `import()` returns a promise.

- `import.meta.url` provides the `file://` URL of the current ES module file.

- Loaders can be written to modify Node.js’s runtime behavior with respect to ES modules. _This is still very much a work in progress._

- Node.js can be run with an ES module file as a program’s initial entry point.

- Files loaded as ES modules are loaded in strict mode, which in CommonJS requires adding `'use strict';` to the top of every file.

- Files ending in `.mjs` are explicitly treated as ES modules in `import` statements and when run via the `node` command.

And the new version of `--experimental-modules` adds:

## `import` and `export` syntax in `.js` files

We heard some [very](https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md) [strong](https://gist.github.com/ceejbot/b49f8789b2ab6b09548ccb72813a1054) [feedback](https://twitter.com/maybekatz/status/1062473765865512961) that Node.js needs to provide a way to use `import` and `export` syntax in `.js` files.

The new `--experimental-modules` provides two ways: a `"type"` field in `package.json` and an `--entry-type` flag. Their naming was inspired by browsers’ `<script type="module">`.

### `package.json` `"type"` field

Add `"type": "module"` to the `package.json` for your project, and Node.js will treat all `.js` files in your project as ES modules.

If some of your project’s files use CommonJS and you can’t convert your entire project all at once, you can either rename those files to use the `.cjs` extension or put them in a subfolder containing a `package.json` with `{ "type": "commonjs" }`, under which all `.js` files are treated as CommonJS.

For any file that Node.js tries to load, it will look for a `package.json` in that file’s folder, then that file’s parent folder and so on upwards until it reaches the root of the volume. This is similar to [how Babel searches for `.babelrc` files](https://babeljs.io/docs/en/config-files#file-relative-configuration). This new approach allows Node.js to use `package.json` for package-level metadata and configuration, similar to how it is already used by Babel and other tools.

### `--entry-type` flag

Use `--entry-type=module` to run a `.js` file as an ES module.

When running a file, e.g. `node --experimental-modules --entry-type=module main.js`, Node.js follows an algorithm to determine if it should load the file as CommonJS or as an ES module. First it looks for an explicit file extension (`.mjs` or `.cjs`); then it looks for a `"type"` field in the nearest parent `package.json`; and finally it looks at the `--entry-type` flag. The `--entry-type` flag can be `--entry-type=module` or `--entry-type=commonjs`.

Note that setting the type of a file via `--entry-type` does _not_ set the type for any files that that file may `import`; you probably want to add a `package.json` with a `"type"` field to define the type for a folder of files.

This flag provides a way to support ES module syntax for input via `--eval`, `--print`, or `STDIN`; and for loose single `.js` files outside of any package or project.

## `.cjs` extension

Just as the `.mjs` file extension explicitly signifies that a file should be treated as an ES module, the new `.cjs` file extension explicitly signifies that a file should be treated as CommonJS. (CommonJS is the other module system that Node.js supports, with `require` and `module.exports`.) The `.cjs` extension provides a way to save CommonJS files in a project where both `.mjs` and `.js` files are treated as ES modules.

## Explicit filenames

By default in the new `--experimental-modules`, file extensions are mandatory in `import` statements: `import './file.js'`, not `import './file'`. However, the CommonJS-style automatic extension resolution behavior (`'./file'`) can be enabled via a new flag, `--es-module-specifier-resolution=node`. (Its inverse, the default, is `--es-module-specifier-resolution=explicit`.) Package names are still just package names, e.g. `import fs from 'fs'`.

We’re providing the `--es-module-specifier-resolution=node` flag to opt in to cjs-style extension and `index` resolution. We’ve turned it off by default to collect feedback on how users feel about using fully specified paths before we unflag the `--experimental-modules` implementation. You can find our discussion on the topic [here](https://github.com/nodejs/modules/issues/268).

Since explicit file extensions are usually required on the Web, we hope that aligning with that style encourages code to be cross-platform by default.

## `module.createRequireFromPath`

The “CommonJS globals” `require`, `exports`, `module.exports`, `__filename`, `__dirname` are not defined in ES modules. However, [`module.createRequireFromPath()`](https://nodejs.org/api/modules.html#modules_module_createrequirefrompath_filename) can be used to create a CommonJS `require` function to be used in an ES module context.

## `import` for JavaScript only

The previous `--experimental-modules` allowed `import` statements of JSON and native modules. This has been removed; you may use `module.createRequireFromPath()` for these.

A separate flag `--experimental-json-modules` enables experimental support for `import` of JSON files. There is [work in progress](https://github.com/whatwg/html/issues/4315) for standardizing this feature with browsers, and Node.js hopes to align our support with the eventual standard.

There is also ongoing work to cover WASM and other future potential module types. Node.js will add support for these in spec-compliant ways over time.

## ES module code in packages

_This is a work in progress and subject to change._ You can create a package with ES module sources by using the `package.json` `"main"` field to point to an ES module package entry point. Node.js will know to load it as an ES module if the file ends in `.mjs` or if the `package.json` also contains `"type": "module"`.

Currently it is not possible to create a package that can be used via both `require('pkg')` and `import 'pkg'`. There are efforts underway to address this, and may involve changes to the above. In particular, Node.js might choose a field other than `"main"` to define a package’s ES module entry point. While we are aware that the community has embraced the `"module"` field, it is unlikely that Node.js will adopt that field as many of the packages published using `"module"` include ES module JavaScript that may not evaluate in Node.js (because extensions are left off of filenames, or the code includes `require` statements, etc.). Please do not publish any ES module packages intended for use by Node.js until this is resolved.

## Works in progress

All of the above is shipping as part of `--experimental-modules` in Node.js 12. On our [road map](https://github.com/nodejs/modules/blob/master/doc/plan-for-new-modules-implementation.md) for potential improvements before the `--experimental-modules` flag is hopefully dropped in October 2019, when Node.js 12 reaches LTS status:

**Loaders features.** Loaders are being further developed to support process isolation, multiple loaders, and multi-Realm support with lower-level hooks. The `--loader` API will still change considerably before this is unflagged.

- **Dual CommonJS/ES module packages.** We want to provide a standard way for package authors to publish a package that can be both `require`d into CommonJS or `import`ed into an ES module.

- **Easier `require`.** Using `module.createRequireFromPath()` involves a lot of boilerplate. We hope to provide a simpler way to use `require` in ES module code.

- **Package path maps.** We would like to support paths to files within packages. This would allow things like `import sdk from 'some-service/sdk'` to have `'some-service/sdk'` map to something like `./src/sdk/public-api.mjs`.

- **Automatic entry point module type detection.** This would provide a way for running `.js` files as either CommonJS or ES modules based on whether `import` or `export` statements appear in the source code of the file.

That’s it! We hope you enjoy this new `--experimental-modules`, and we look forward to hearing your feedback. The modules team’s work is public at https://github.com/nodejs/modules.
