# Plan for New Modules Implementation

This document outlines the plan for building a new implementation to support ECMAScript modules in Node.js. The effort is split up into phases:

* **Phase 0** branches off of current Node but removes much of the Node 8.5.0+ `--experimental-modules` implementation so that a new implementation could be built in its place.

* **Phase 1** adds the “minimal kernel,” features that the modules working group felt would likely appear in any potential new ES modules implementation.

* **Phase 2** fleshes out the implementation with enough functionality that it should be useful to average users as a minimum viable product.

  - At the completion of Phase 2, the old `--experimental-modules` implementation was [replaced](https://github.com/nodejs/node/pull/26745) with this new one (still behind the `--experimental-modules` flag). It was released as part of Node 12 on 2019-04-23.

* **Phase 3** improves user experience and extends the MVP.

  - At the completion of Phase 3, the new implementation’s experimental flag will be dropped. The goal is to “release” (drop the `--experimental-modules` flag) by when Node 12 starts LTS in October 2019.
  
* **Phase 4** are items that were under development in earlier phases but weren’t finished when the new implementation’s experimental flag was dropped; these items may continue development after unflagging and potentially ship in later versions of Node.js.

The effort is currently in **[Phase 3](#phase-3-path-to-stability-removing---experimental-modules-flag)**.

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

Phase 2 fleshes out the implementation with enough functionality that it should be useful to average users as a minimum viable product. At the completion of Phase 2, the old `--experimental-modules` implementation was replaced with this new one (still behind the `--experimental-modules` flag).

* Define semantics for importing a package entry point, e.g. `import _ from 'lodash'`
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) covers bare module specifier resolution of CommonJS packages.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/28.

* Define semantics for determining when to load sources as CommonJS or ES module for both the top-level main (`node x.js`) and dependency loading.
  - Proposal: [“File Specifier Resolution” proposal](https://github.com/GeoffreyBooth/node-import-file-specifier-resolution-proposal) covers `import` statements of ESM files; and CommonJS files, package entry point and package deep imports.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/28.

* Define semantics for enabling ESM treatment of source code loaded via `--eval`, STDIN, and extensionless files (both with and without shebang lines).
  - Proposal: [“Entry Points Proposal”](https://github.com/geoffreybooth/node-esm-entry-points-proposal) covers non-file forms of input as well as adding `--type` flag for controlling file-based input.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/32.
  - Renamed to `--entry-type` as part of upstream [PR](https://github.com/nodejs/node/pull/26745) to Node.js core.
  - Renamed to `--intry-type` and limited to `--eval`, `--print` and `STDIN` as part of follow-up [PR](https://github.com/nodejs/node/pull/27184) to Node.js core.

* File extension and directory index searching in ESM, behind its own flag, `--es-module-specifier-resolution`.
  - See https://github.com/nodejs/modules/issues/268.
  - Landed in https://github.com/nodejs/ecmascript-modules/pull/48.

The work through the end of Phase 2 landed in Node.js `master` as part of https://github.com/nodejs/node/pull/26745 and was released in Node 12.0.0.

## Phase 3: Path to Stability: Removing `--experimental-modules` Flag

Phase 3 improves user experience and extends the MVP. Phase 3 is malleable based on how things proceed while working on this phase. At the end of this phase, the `--experimental-modules` flag is dropped.

* Better mechanism for creating `require` function: `createRequire`.
  - Landed in https://github.com/nodejs/node/pull/27405 and shipped in 12.2.0.

* `"exports"` field: for consumers of a package, map the paths of deep imports to provide encapsulation (an explicit public API); pretty specifiers (no file exensions or paths that include things like `dist/`) and flexibility for future package versions renaming or moving files without affecting the package’s public API. Applies to both ESM and CommonJS.
  - Proposal: [Package Exports Proposal](https://github.com/jkrems/proposal-pkg-exports).
  - Landed in https://github.com/nodejs/node/pull/28568 and shipped in 12.7.0 behind `--experimental-exports`. Further improvements are being made as additional PRs against core.

* Provide a way to make ESM the default instead of CommonJS.
  - Discussion: https://github.com/nodejs/modules/issues/318.
  - Proposal: https://github.com/nodejs/modules/issues/335.
  - **Tabled**. Currently one can add `--input-type=module` to `NODE_OPTIONS` to flip the default for `--input-type`; at the moment the group is deciding not to pursue providing an ability to flip the default for the `package.json` `type` field. We instead want to encourage all packages, both ESM and CommonJS, to include an explicit `type` field; leaving it out and relying on a default is something we want to discourage.
  
* Define behavior for builtin globals between CommonJS and ESM. Does modifying a builtin in one module system carry over into the other? If it does, we may have major performance concerns.
  - Issue: https://github.com/nodejs/node/pull/29426.
  - Current plan is to remove proxy support for changes in one system to affect the other.

## Phase 4: Further Improvements After Unflagging

* A loaders solution that supports all items in the [features list in our README](https://github.com/nodejs/modules/#features).
  - Discussion: https://github.com/nodejs/modules/issues/351.
  - Design: https://docs.google.com/document/d/1J0zDFkwxojLXc36t2gcv1gZ-QnoTXSzK1O6mNAMlync/edit#heading=h.xzp5p5pt8hlq.
  - **Status**: In development behind a flag.

* Shortcut to resolve to package root.
  - Proposal: [Package `"name"` Resolution Proposal](https://github.com/guybedford/package-name-resolution).
  - Discussion: https://github.com/nodejs/modules/issues/306.
  - Upstream PR: https://github.com/nodejs/node/pull/29327 (proposes `@` instead of package name).
  - **Status**: Upstream PR submitted.

* Limited module type detection.
  - PR: https://github.com/nodejs/ecmascript-modules/pull/69.
  - Upstream PR: https://github.com/nodejs/node/pull/27808.
  - **Status**: Upstream PR submitted.

* Dual CommonJS/ESM packages: Either support packages with both CommonJS and ESM sources that can be used in either environment; or decide to specifically not support dual CommonJS/ESM packages.
  - Status quo (at time of possible unflagging): `"main"` points to exactly one file, and all file extensions are mandatory (by default), so there is no possibility of an `import` specifier pointing to different files in ESM versus CommonJS. Recommended practice for dual packages is to have `"main"` point to the CommonJS entry point and have users use a deep import, e.g. `/module.mjs`, to access ESM entry point.
  - Proposal for new `package.json` field for ESM entry point: https://github.com/nodejs/modules/issues/273.
    - PR for above proposal: https://github.com/nodejs/ecmascript-modules/pull/41.
    - PR is currently blocked because of this issue: https://github.com/nodejs/modules/issues/371.
  - Proposal for `require` of ESM: https://github.com/nodejs/modules/issues/299.
  - **Status**: “New field” proposal is [blocked](https://github.com/nodejs/modules/issues/371). “`require` of ESM” proposal may ship after unflagging if it is implemented and finds approval upstream.

* Finalize behavior of `import` of CommonJS files and packages.
  - Overview: https://github.com/nodejs/modules/issues/264.
  - Status quo (at time of possible unflagging): `import` only the CommonJS default export, so `import _ from 'cjs-pkg'` but not `import { shuffle } from 'cjs-pkg'`.
  - If the spec changes to allow it, we want to implement dynamic modules to enable named exports from CommonJS: https://github.com/nodejs/modules/issues/252.
  - Another option is to specify CommonJS named exports in `package.json`: https://github.com/nodejs/modules/issues/324.
  - **Status**: Dynamic modules is stalled due to upstream concerns from TC39. Named exports in `package.json` seeks consensus but could potentially ship after unflagging.
