# Announcing core Node.js support for ECMAScript modules

Node.js 13.2.0 ships support for [ECMAScript modules](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/), known for their `import` and `export` statements. This support was previously behind the `--experimental-modules` flag, which is no longer required; however the implementation remains experimental and subject to change.

## Enabling

Node.js will treat the following as ES modules when passed to `node` as the initial input, or when referenced by `import`:

* Files ending in `.mjs`.

* Files ending in `.js`, or extensionless files, when the nearest parent `package.json` file contains a top-level field `"type"` with a value of `"module"`.

* Strings passed in as an argument to `--eval` or piped to `node` via `STDIN`, with the flag `--input-type=module`.

Node.js will treat as CommonJS all other forms of input, such as `.js` files where the nearest parent `package.json` file contains no top-level `"type"` field, or string input without the flag `--input-type`. This behavior is to preserve backward compatibility. However, now that Node.js supports both CommonJS and ES modules, it is best to be explicit whenever possible.

Node.js will treat the following as CommonJS when passed to `node` as the initial input, or when referenced by `import`:

* Files ending in `.cjs`.

* Files ending in `.js`, or extensionless files, when the nearest parent `package.json` file contains a top-level field `"type"` with a value of `"commonjs"`.

* Strings passed in as an argument to `--eval` or piped to `node` via `STDIN`, with the flag `--input-type=commonjs`.

For more information, see [Package Scope and File Extensions](https://nodejs.org/api/esm.html#esm_package_scope_and_file_extensions) and [`--input-type` flag](https://nodejs.org/api/esm.html#esm_code_input_type_code_flag).

## `import` and `export` syntax

Within ES module files and string input, [`import` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) can reference JavaScript files. Files can be referenced as:

- Relative URLs (`'./file.mjs'`)

- Absolute `file://` URLs (`'file:///opt/app/file.mjs'`)

- Package names (`'es-module-package'`)

- Paths within packages (`'es-module-package/lib/file.mjs'`)

`import` statements that reference ES module files can specify the default export (`import _ from 'es-module-package'`), named exports (`import { shuffle } from 'es-module-package'`) and namespace exports (`import * as fs from 'fs'`). All Node.js built-in packages like `fs` and `path` support all three types of exports.

`import` statements that reference CommonJS files (all current JavaScript code written for Node.js, [using `require` and `module.exports`](https://nodejs.org/api/modules.html)) can only use the CommonJS default export (`import _ from 'commonjs-package'`).

`import` statements that reference other types of files such as JSON or Wasm are experimental and require the [`--experimental-json-modules`](https://nodejs.org/api/esm.html#esm_experimental_json_modules) or [`--experimental-wasm-modules`](https://nodejs.org/api/esm.html#esm_experimental_wasm_modules) flags. The non-experimental [`module.createRequire`](https://nodejs.org/api/modules.html#modules_module_createrequire_filename) can be used to import JSON or native addons.

[`export` statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) in ES module files can specify both default and named exports for `import` statements to reference.

Dynamic `import()` expressions can be used to import ES modules from either CommonJS or ES module files. Note that `import()` returns a promise.

[`import.meta.url`](https://nodejs.org/api/esm.html#esm_import_meta) provides the absolute URL of the current ES module file.

## Files and the `package.json` `"type"` field

Add `"type": "module"` to the `package.json` for your project, and Node.js will treat all `.js` files in your project as ES modules.

If some of your project’s files use CommonJS and you can’t convert your entire project all at once, you can either rename those files to use the `.cjs` extension or put them in a subfolder containing a `package.json` with `{ "type": "commonjs" }`, under which all `.js` files are treated as CommonJS.

For any file that Node.js tries to load, it will look for a `package.json` in that file’s folder, then that file’s parent folder and so on upwards until it reaches the root of the volume. This is similar to [how Babel searches for `.babelrc` files](https://babeljs.io/docs/en/config-files#file-relative-configuration). This new approach allows Node.js to use `package.json` for package-level metadata and configuration, similar to how it is already used by Babel and other tools.

We encourage all packages to include the `"type"` field, even if the `"type"` is `"commonjs"`.

## Package entry points and the `package.json` `"exports"` field

There are now two fields that can define [entry points for a package](https://nodejs.org/api/esm.html#esm_package_entry_points): `"main"` and [`"exports"`](https://nodejs.org/api/esm.html#esm_package_exports). The `"main"` field is supported in all versions of Node.js, but its capabilities are limited: it only defines the main entry point of the package. A new `package.json` field `"exports"` can also define the main entry point, along with subpaths. It also provides encapsulation, where only the paths explicitly defined in `"exports"` are available for importing. `"exports"` applies to both CommonJS and ES module packages, whether used via `require` or `import`.

This allows statements like `import 'pkg/feature'` to map to a path like `'./node_modules/pkg/esm/feature.js`. It also causes an error to be thrown for statements like `import 'pkg/esm/feature.js'` unless that path is defined in `"exports"`.

An experimental feature, [conditional exports](https://nodejs.org/api/esm.html#esm_conditional_exports), would add support for an exported path, including the package root, to map to different files in different environments. This would allow a package to provide CommonJS sources for `require('pkg')` and ES module sources for `import 'pkg'`, though writing such a package is not without [hazards](https://nodejs.org/api/esm.html#esm_dual_commonjses_module_packages). Conditional exports can be enabled via the `--experimental-conditional-exports` flag.

## Common Gotchas

### Mandatory file extensions

A file extension must be provided when using the `import` keyword. Directory indexes (e.g. `'./startup/index.js'`) must also be fully specified.

This behavior matches how `import` behaves in browser environments, assuming a typically configured server.

### No <code>require</code>, <code>exports</code>, <code>module.exports</code>, <code>\_\_filename</code>, <code>\_\_dirname</code>

These CommonJS variables are not available in ES modules.

`require` can be imported into an ES module using [`module.createRequire()`](https://nodejs.org/api/modules.html#modules_module_createrequire_filename).

Equivalents of `__filename` and `__dirname` can be created inside of each file via [`import.meta.url`](https://nodejs.org/api/esm.html#esm_import_meta).

## Writing packages

At the moment, we recommend that packages are written using either entirely CommonJS or entirely ES module syntax for the JavaScript sources intended for Node.js. The modules team is working on better support for “dual” packages, that provide CommonJS sources for package consumers using `require` and ES module sources for package consumers using `import`. The current experimental feature covering this use case is [conditional exports](https://nodejs.org/api/esm.html#esm_conditional_exports), and the team hopes to ship that or an alternative by the end of January 2020 if not earlier. See recommended patterns in [Dual CommonJS/ES Module Packages](https://nodejs.org/api/esm.html#esm_dual_commonjses_module_packages).

## Works in progress

- **Loaders.** Work is ongoing for an API to allow for custom loaders that can support use cases like runtime transpilation, rewriting specifiers (package or file paths) and code instrumentation. The [`--experimental-loader` API](https://nodejs.org/api/esm.html#esm_experimental_loader_hooks) will still change considerably before this is unflagged.

- **Dual CommonJS/ES module packages.** We want to provide a standard way for package authors to publish a package that can be both `require`d into CommonJS or `import`ed into an ES module. See [Dual CommonJS/ES Module Packages](https://nodejs.org/api/esm.html#esm_dual_commonjses_module_packages). The plan is for this to be finalized by the end of January 2020 if not earlier.

That’s it! We hope this new support for ECMAScript modules brings Node.js closer to JavaScript standards and increases opportunities for ecosystem-wide compatibility. The modules team’s work is public at [https://github.com/nodejs/modules](https://github.com/nodejs/modules).
