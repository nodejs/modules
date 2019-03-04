# Plan for New Modules Implementation

This document outlines the plan for building a new implementation to support ECMAScript modules in Node.js. The effort is split up into phases:

* **Phase 0** branches off of current Node but removes much of the Node 8.5.0+ `--experimental-modules` implementation so that a new implementation could be built in its place.

* **Phase 1** adds the “minimal kernel,” features that the modules working group felt would likely appear in any potential new ES modules implementation.

* **Phase 2** fleshes out the implementation with enough functionality that it should be useful to average users as a minimum viable product.
  - At the completion of Phase 2, the old `--experimental-modules` implementation is replaced with this new one (still behind the `--experimental-modules` flag). The goal is to “upstream” the new implementation with the release of Node 12, in April 2019.

* **Phase 3** improves user experience and extends the MVP.

  - At the completion of Phase 3, the new implementation’s experimental flag is dropped. The goal is to “release” (drop the `--experimental-modules` flag) by when Node 12 starts LTS in October 2019.

The effort is currently in **Phase 2**.

At every phase, the following standards must be maintained:

* Spec compliance ([#132](https://github.com/nodejs/modules/issues/132)): We must always follow the ES spec.
* Browser equivalence ([#133](https://github.com/nodejs/modules/issues/133)): There’s room for debate in specific cases, but in general if Node is doing something that browsers also do, Node should do it in the same way. Alternatively, code that executes in both environments should produce identical results.
* Don’t break CommonJS ([#112](https://github.com/nodejs/modules/issues/112)): We cannot cause breaking changes with regards to CommonJS.

See also the [features list in the README](https://github.com/nodejs/modules#features).

## Phase 0: Start Fresh

From current shipping Node, the following changes were made to strip out most of the Node 8.5.0+ `--experimental-modules` implementation so that a new implementation could be built in its place:

* Remove support in the `import` statement of formats other than ESM:
  - No CommonJS.
  - No JSON.
  - No native modules.

* Remove dynamic path searching:
  - No extension adding.
  - No directory resolution, including no support for `index.js` or `index.mjs`.
  - No support for `main` field for ESM.

* Remove current VM implementation

* Remove current Loader implementation

These changes were implemented in https://github.com/nodejs/ecmascript-modules/pull/6.

## Phase 1: The Minimal Kernel

The “minimal kernel” consists of features that the @nodejs/modules group have agreed will be necessary for all potential iterations of our ESM implementation. Phase 1 does _not_ include features that preclude other potential features or implementation approaches; and Phase 1 also does not include some features that should naturally be built in a later phase of development, for example because those features depend on features planned for Phase 1.

* `module.createRequireFromPath` ([nodejs/node#19360](https://github.com/nodejs/node/pull/19360)) is the only way to import CommonJS into an ES module, for now.
  - `import.meta.require` fails at runtime as opposed to import time. This is not desireable to all committee members.
  - Hold off on `import` statements for CommonJS until more progress is made on the dynamic modules spec.
  - Landed in core in https://github.com/nodejs/node/commit/246f6332e5a5f395d1e39a3594ee5d6fe869d622

* `import` statements will only support files with an `.mjs` extension, and will import only ES modules, for now.
  - No JSON or native modules; `createRequireFromPath` can be used to get these.

* `import.meta.url`.
  - Already in the existing implementation.

* Dynamic `import()`.
  - Already in the existing implementation.

* Support for built-in modules with named exports
  - Already in the existing implementation.

## Phase 2: Minimum Viable Product: Required to Upstream

Phase 2 fleshes out the implementation with enough functionality that it should be useful to average users as a minimum viable product. At the completion of Phase 2, the old `--experimental-modules` implementation is replaced with this new one (still behind the `--experimental-modules` flag).

### Core Functionality

* Define semantics for importing a package entry point, e.g. `import _ from 'lodash'`
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) covers bare module specifier resolution of CommonJS packages.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/28.

* Define semantics for determining when to load sources as CommonJS or ES module for both the top-level main (`node x.js`) and dependency loading.
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) covers `import` statements of ESM files; and CommonJS files, package entry point and package deep imports.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/28.

* Define semantics for enabling ESM treatment of source code loaded via `--eval`, STDIN, and extensionless files (both with and without shebang lines).
  - Proposal: [“Entry Points Proposal”](https://github.com/geoffreybooth/node-esm-entry-points-proposal) covers non-file forms of input as well as adding `--type` flag for controlling file-based input.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/32.

### Needs Consensus

* Add, or decide not to support, file extension and directory index searching in ESM.
  - See https://github.com/nodejs/modules/issues/268.


## Phase 3: Path to Stability: Removing `--experimental-modules` Flag

Phase 3 improves user experience and extends the MVP. Phase 3 is malleable based on how things proceed while working on this phase. At the end of this phase, the `--experimental-modules` flag is dropped.

### UX Improvements

* A loaders solution that supports all items in the [features list in our README](https://github.com/nodejs/modules/#features).
  - Should loaders be per package, per application or either?

* Dual CommonJS/ESM packages: Either support packages that can both be `require`d as CommonJS and `import`ed as ESM; or decide to specifically not support dual CommonJS/ESM packages.
  - Proposal: https://github.com/nodejs/modules/issues/273.
  - PR: https://github.com/nodejs/ecmascript-modules/pull/41.

* Better mechanism for creating `require` function.
  - See [https://gist.github.com/SMotaal/e73c12bd801d78a3108fa30ecd303676](https://gist.github.com/SMotaal/e73c12bd801d78a3108fa30ecd303676).
  - `import 'nodejs:require'`? `import.meta.require`? Or only `createRequireFromPath`?

* Map the paths within modules, providing similar functionality as the browser’s [import maps proposal](https://github.com/WICG/import-maps#packages-via-trailing-slashes).
  - Proposal: [“Package Exports Proposal”](https://github.com/jkrems/proposal-pkg-exports).

* Automatic entry point module type detection.
  - Proposal: [“Entry Points Proposal”](https://github.com/geoffreybooth/node-esm-entry-points-proposal) includes `--type=auto` flag for running `.js` files in either ESM or CommonJS based on which module system is detected.

### Needs Consensus

* Finalize support for (or removal of) `import` of CommonJS files and packages.
  - See https://github.com/nodejs/modules/issues/264.
  - Defaults only or named exports? Behind a flag or not?
