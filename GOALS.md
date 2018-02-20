# Preface: Design Principles

These are a list of commitments that can be used to define reasons for goals existing.

* We are committed to shipping ES Modules (ESM)
* We are committed to ESM being first class in Node.js
* We are committed to having the Node.js and Web platform as first class runtimes for modules.
* Modules installed though a package manager should be able to run after installation without requiring a [preprocessing step](#goal-define-preprocessing-step).
* We are committed to not breaking our existing ecosystem for modules
* We are committed to providing APIs or conventions such that users will be able to use both CommonJS (CJS) and ESM with support for both workflows.
* We are commited to having security be a primary concern for ESM in Node.js.
* We are commited to having performance be a primary concern for ESM in Node.js.

# Goals

1. Enable use of ESM in Node without requiring a flag
2. Have files be unable to collide between module systems
3. Define a path to write code that runs in both Node and the Browser without a preprocessing step
    1. <a name="goal-define-preprocessing-step"></a>Define what a preprocessing step is
    2. A subset of functionality that is compatible between environments, not requiring either environment implement all features of the other
4. Define a path to write libraries that support being used by both
    * Node versions supporting ESM
    * Node versions that do not support ESM
5. Be standards compliant in the nature of our loading of ESM
    1. Contextual module APIs exposed to ESM by default will be placed under `import.meta.*`. This does not relate to per Realm globals nor module specifier resolution.
    2. Should a choice be made to have a feature that is not compliant to ESM for any reason, Node will provide a way to create an ESM compliant implementation.
6. Support developer centric workflows for:
    1. Intercepting dependencies and replacing them
    2. Application Performance Monitoring (APM) integration
    3. Multiple distributions of both ESM and CJS in the same package
    4. Creation of userland module systems (JSDOM)
    5. Incremental code base migration from CJS to ESM by migrating individual files
7. 100% backwards compatibility with existing CJS code bases if left unchanged
8. Provide a path to have singleton references that can be obtained in both module systems

# Non-Goals

1. Use `.js` file extension for both CJS and ESM without a disambiguation mechanism
    * e.g. `require` falling back to CJS when it is unsure about the format of a resolved path
2. Interoperability of Module formats with environments that do not support Module formats that Node seeks to support
3. Forwards compatibility support for the transition of existing packages from CJS to ESM
    1. loading unknown file formats and `.js` as CJS
    2. usage of `require.extensions`
    3. usage of `require.cache`
    4. usage of `NODE_PATH`, `~/.node_libraries`, `~/.node_modules`, and `$PREFIX/lib/node`
4. Standardizing and implementing APIs that explicitly are tailored to APM
5. Standardizing and implementing APIs that explicitly are tailored to testing libraries
6. Standardizing and implementing APIs that explicitly are tailored to transpilers
7. Standardizing and implementing APIs that explicitly are tailored to browser stacks
8. Standardizing and implementing APIs that explicitly are tailored to package managers
9. Modification of existing `require` behavior
    * Modification of implementation details is allowed only if it would be backwards compatible

# Goals that are not expected for the first iteration

1. 100% stable Loader APIs
    * Any Loader APIs shipped must have a forward compatible approach towards their usage, such as providing a feature dection mechanism or versioned API.
2. Treatment of `.js` file extension as ESM by default
3. Support for the following URL schemes: `data:`, `blob:`, and `https:`

# Direction

* Build upon the existing work in Node that has designs around using `.mjs`
* Enable usage of `.js` for ESM through compatible means of out of band data
* Build upon the existing Loader hooks to aid usability
    * Creation of in memory Module records
    * Composition of Loaders
    * Find a solution to the live-ness problem
    * Discuss issues/benefits around static vs dynamic mutation of loader hooks

# Forwards compatibility concerns

* Interoperability with WASM
* Interoperability with browser support for more module formats
* Resource integrity checks
* Preparation for introduction of more JS spec breaking changes
    * new parsers added to the JS spec
    * new out of band data added to the JS spec
* Enable a future where ESM is the default module system across JS environments