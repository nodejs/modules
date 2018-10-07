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

* A `--mode` field to enable ESM support in the cases of `--eval` and STDIN input, _or_ any file: `node --mode=esm index.js`.
  - Using this in `package.json` or being scoped to packages is put off for a later phase.
  - This will be supplemented/replaced by more robust configurability such as designed in ([#160](https://github.com/nodejs/modules/pull/160)) in a later phase.

* `createRequireFromURL`, to complement the just-added `createRequireFromPath`.

* Re-introduce VM module integration.
  - Implemented in: https://github.com/nodejs/ecmascript-modules/pull/8.

## Future Phases

The following features will follow soon in upcoming phases:

* Loaders ([#82](https://github.com/nodejs/modules/issues/82)), ([#96](https://github.com/nodejs/modules/issues/96)).

* `package.json` metadata support, including an ESM-compatible design for the `main` and possibly `module` fields; and user-configurable map for file extensions to parse goals, a.k.a. `mimes` field ([#160](https://github.com/nodejs/modules/pull/160)).

* Browser-compatible specifier resolution ([#109](https://github.com/nodejs/modules/issues/109)), a.k.a. bare imports. See [package name maps](https://github.com/domenic/package-name-maps).

* Browser-compatible dynamic path searching.

* Multi-mode packages ([#94](https://github.com/nodejs/modules/issues/94)).

* ESM in executable files ([#152](https://github.com/nodejs/modules/issues/152)).

* Callable resolver ([#157](https://github.com/nodejs/modules/issues/157)).

* Mock modules (injection) ([#98](https://github.com/nodejs/modules/issues/98)).

This is an incomplete list. More features will be added in future phases, based on the [features list in our README](https://github.com/nodejs/modules/#features).
