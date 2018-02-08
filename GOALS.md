# Goals

1. Enable use of ESM in Node without requiring a flag
2. Have files be unable to collide between module systems
3. Define a path to write code that runs in both Node and the Browser without a preprocessing step
    1. Define what a preprocessing step is
    2. A subset of functionality that is compatible between environments, not requiring either environment implement all features of the other
4. Define a path to write libraries that support being used by both
    * Node versions supporting ESM
    * Node versions that do not support ESM
5. Be standards compliant in the nature of our loading of ESM
    1. All platform specific APIs exposed to ESM by default will be placed under `import.meta.*`
6. Support developer centric workflows for:
    1. Intercepting dependencies and replacing them
    2. Application Performance Monitoring (APM) integration
    3. Multiple distributions of both ESM and CJS in the same package
    4. Creation of userland module systems (JSDOM)
7. 100% backwards compatibility with existing CJS code bases if left unchanged

# Non-Goals

1. Use `.js` file extension for both CJS and ESM without a disambiguation mechanism
2. Interoperability of Module formats with environments that do not support Module formats that Node seeks to support
3. Forwards compatibility support for the transition of existing packages from CJS to ESM
    1. loading unknown file formats and `.js` as CJS
    2. usage of `require.extensions`
    3. usage of `require.cache`
    4. usage of `NODE_PATH`, `~/.node_libraries`, `~/.node_modules`, and `$PREFIX/lib/node`
4. Direct integration with APMs
5. Direct integration with testing libraries
6. Direct integration with transpilers
7. Direct integration with browser stacks
8. Direct integration with package managers

# Goals that are not expected for the first iteration

1. 100% stable Loader APIs
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
* Scaling of more JS goals and out of band flags
* Enable a future where ESM is the default module system across JS environments