# Plan for New Modules Implementation

This document outlines the plan for building a new implementation to support ECMAScript modules in Node.js. The general idea is to start with a “minimal kernel” as Phase 1, which consists of features that the @nodejs/modules group have agreed will be necessary for all potential iterations of our ESM implementation. Phase 1 does _not_ include features that preclude other potential features or implementation approaches; and Phase 1 also does not include some features that should naturally be built in a later phase of development, for example because those features depend on features planned for Phase 1. The minimal kernel/phase 1 is _not_ intended to be merged into Node core or released; it is only a starting point for gradually building layers of consensus.

At every phase, the following standards must be maintained:

* Spec compliance ([#132](https://github.com/nodejs/modules/issues/132)): We must always follow the ES spec.
* Browser equivalence ([#133](https://github.com/nodejs/modules/issues/133)): There’s room for debate in specific cases, but in general if Node is doing something that browsers also do, Node should do it in the same way. Alternatively, code that executes in both environments should produce identical results.
* Don’t break CommonJS ([#112](https://github.com/nodejs/modules/issues/112)): We cannot cause breaking changes with regards to CommonJS.

See also the [features list in the README](https://github.com/nodejs/modules#features).

## Phase 1: The Minimal Kernel

These features will be part of the first phase of development:

* `module.createRequireFromPath` ([nodejs/node#19360](https://github.com/nodejs/node/pull/19360)) is the only way to import CommonJS into an ES module, for now.
  - `import.meta.require` fails at runtime as opposed to import time. This is not desireable to all committee members
  - Hold off on `import` statements for CommonJS until more progress is made on the dynamic modules spec.
  - landed in https://github.com/nodejs/node/commit/246f6332e5a5f395d1e39a3594ee5d6fe869d622

* `import` statements will only support files with an `.mjs` extension, and will import only ES modules, for now.
  - In a later phase, the intention is to move forward with format databases to map extensions and support multiple use cases.
  - No JSON or native modules; `createRequireFromPath` can be used to get these.

* `import.meta.url`.
  - Already in the existing implementation.

* Dynamic `import()`.
  - Already in the existing implementation.

* Support for built-in modules with named exports
  - Already in the existing implementation

### How will we get from where we are to Phase 1

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

These changes are implemented in https://github.com/nodejs/ecmascript-modules/pull/6

## Phase 2

* Explore design space for virtual module from source
  - Potential implementation in: https://github.com/nodejs/ecmascript-modules/pull/8.

* Improve CommonJS interoperability.
  - Refine `createRequireFromPath`.
  - See [https://gist.github.com/SMotaal/e73c12bd801d78a3108fa30ecd303676](https://gist.github.com/SMotaal/e73c12bd801d78a3108fa30ecd303676).

* Define semantics for importing a package entry point, e.g. `import _ from 'lodash'`
  - Currently this is only possible via an explicit deep import, e.g. `import _ from 'lodash/index.mjs'`. The idea would be to somehow enable the former syntax.
  - `package.json` `module` field? `main` field?
  - Proposal: [“Package Exports” proposal](https://github.com/jkrems/proposal-pkg-exports) for bare module specifier resolution of ESM packages.
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) mentions bare module specifier resolution of CommonJS packages; complements Package Exports proposal.

* Define semantics for determining when to load sources as CommonJS or ES module for both the top-level main (`node x.js`) and dependency loading.
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) covers `import` statements of ESM files; and CommonJS files, package entry point and package deep imports.

* Implement specification changes related to dynamic module records
  - Proposal: ["Dynamic Modules Proposal"](https://github.com/nodejs/dynamic-modules/)
  - We will need to reach consensus on appropriate behavior for [`export * from 'dynamic-module'`](https://github.com/nodejs/dynamic-modules/pull/11). If consensus cannot be reached, then this feature will be deferred to a later phase. The current behavior of throwing may also be reverted at a later time.

* Define semantics for enabling ESM treatment of source code loaded via `--eval`, STDIN, and extensionless files.

## Phase 3

Phase 3 will tentatively focus on extensible loaders and deliver an environment that allows user-land experimentation.

We should try to find a loaders solution that supports all items in the [features list in our README](https://github.com/nodejs/modules/#features).

## Phase 4

Phase 4 will include addressing user feedback gathered from the experimentation enabled by Phase 3 and focus on a holistic and complete experience of ESM in Node.js.

## Future Phases

TBD.
