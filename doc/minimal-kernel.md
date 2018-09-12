# What is a Minimal Kernel for ESM in Node.js?

A minimal kernel is a subset of features that the @nodejs/modules group have agreed will be necessary for all potential iterations of our ESM implementation. It strips out any features that preclude other features 

# Why build a Minimal Kernel?

Having a minimal kernel creates a subset that we can build consensus on top of. It will also allow sharing features across proposals that potentially have different end goals and feature sets. Finally, reaching consensus on a minimal kernel is a win for the team, showing that we do indeed have a layer of consensus even if it is not entirely clear.

# What is our minimal kernel

* both browser + node need bare imports
  - implementation to be discussed
* we cannot have dynamic path searching
  - requiring the full path is an issue for tooling and a long term solution is required
  - migration strategies also have issue with this
* common.js backwards compat
  - createRequireFunction does
  - import.meta.require does not fail early enough
  - hold off on importing common.js until more progress is made on dynamic modules spec
* .mjs will be the only way to import ESM
  - intention is to move forward with format databases to map extensions and support multiple use cases
* only supporting importing ESM
  - no JSON
  - no native modules
  - createRequireFunction used to get these
  - will come back with format database
* import.meta.url
* dynamic import

# How will we get from where we are to the minimal kernel

* removing importing of formats other than ESM
  - no common.js
  - no JSON
  - no native modules
  - implemented in https://github.com/nodejs/ecmascript-modules/pull/3
* removing of dynamic path searching
  - no extension adding
  - no directory resolution
    - no support for index.js
  - still maintaining support for the main field
  - implemented in https://github.com/nodejs/ecmascript-modules/pull/2
* adding createRequireFunction
  - implemented in https://github.com/nodejs/node/pull/19360
