| This is a living document. It describes the policy and priorities as they exist today, but can evolve over time. |
|---|

# Node.js Modules Group Wishlist

*The items below represent functionality that has been recognized as desirable, but cannot be implemented due to either being currently blocked or unplanned. For items that are currently planned, see the [2020 roadmap](./roadmap-2020.md).*

## Blocked

* Named exports for CommonJS modules, e.g. `import { shuffle } from 'cjs-pkg'` in addition to the currently supported `import _ from 'cjs-pkg'`.
  - Overview: https://github.com/nodejs/modules/issues/264.
  - Discussion: https://github.com/nodejs/modules/issues/448.
  - Proposal: https://github.com/nodejs/modules/issues/252.
  - **Status**: As far as we can tell, currently there is no spec-compliant way to achieve this.

	CommonJS is not statically analyzable (that’s one of the primary benefits of `import`/`export` syntax) and therefore the exported names cannot be known to Node.js simply by parsing JavaScript source code: the code needs to actually be evaluated in order for Node.js to discover what identifiers get attached to `module.exports` for each file. But evaluating the CommonJS code in what the ECMAScript Modules spec calls the “linking” phase (when the module graph is determined) is not permitted; this would mean that CommonJS code is evaluated early, before any ES module code, whereas the spec defines the order of modules’ evaluation based on the depth of imports. This early evaluation presents other issues in that if a dependency gets converted from CommonJS to ESM, its order of evaluation would change in the user’s application; and early evaluation might trigger side effects if reading the values from `module.exports` were getters or proxies.

	A [“dynamic modules” proposal](https://github.com/nodejs/modules/issues/252) was created and presented to TC39, the standards body responsible for the ECMAScript Modules spec. This was a proposal to allow new export names to be discovered during the execution phase, which happens after linking. This made named exports from CommonJS work, but had the caveat that `export *` would not be supported for CommonJS sources. Without a solution for this edge case, TC39 deemed the proposal inadequate. There are currently no other proposals in development to try to surmount these obstacles.

	The approach taken by the [`standard-things/esm`](https://github.com/standard-things/esm) package is to transpile ES module sources into CommonJS on the fly, and have Node.js evaluate the CommonJS sources. This is not considered a viable option for Node.js because it means that the actual source code being executed by the V8 JavaScript engine is not the code that the user wrote.

* `require` of ES modules: Support using `require` (in addition to `import()`) to load ES modules into a CommonJS context.
	- Discussion: https://github.com/nodejs/modules/issues/299.
	- PR: https://github.com/nodejs/node/pull/30891.
	- **Status**: Blocked due to various issues discussed in https://github.com/nodejs/modules/issues/454.

## Not Planned

* Limited module type detection.
  - PR: https://github.com/nodejs/ecmascript-modules/pull/69.
  - Upstream PR: https://github.com/nodejs/node/pull/27808.
  - **Status**: The group’s current thinking is to encourage explicit source type definition via file extension or the `"type"` field. Source type detection via parsing source code is not 100% accurate and should therefore be left to userland where users can choose to accept its risks.

* Provide a way to make ESM the default instead of CommonJS.
  - Discussion: https://github.com/nodejs/modules/issues/318.
  - Proposal: https://github.com/nodejs/modules/issues/335.
  - **Status**: Currently one can add `--input-type=module` to `NODE_OPTIONS` to flip the default for `--input-type`; at the moment the group is deciding not to pursue providing an ability to flip the default for the `package.json` `type` field. We instead want to encourage all packages, both ESM and CommonJS, to include an explicit `type` field; leaving it out and relying on a default is something we want to discourage.

---

[**[ ROADMAP ]**](./roadmap-2020.md) [**[ PREVIOUS WORK ]**](./previous-work.md)
