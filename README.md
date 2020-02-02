# Modules Team

## Purpose

The Node.js Modules Team maintains and actively develops the ECMAScript Modules (ESM) implementation in Node.js core.

Work includes:

* ESM loader implementation
* ESM specifier resolution
* Loader hooks
* ESM and CommonJS interoperability
* Node.js and browser interoperability
* VM modules implementation

## Current Efforts

### To Do / In Progress

* [Loader hooks](https://nodejs.org/api/esm.html#esm_hooks) (work in progress). The goal is to create hooks that provide enough functionality to enable users to build loaders that can achieve many of the remaining [features](./doc/features.md) we hope to support.
	- Overview: https://github.com/nodejs/modules/issues/351.
	- A [design](https://github.com/nodejs/node/pull/30986) that enables [transpilation](https://github.com/nodejs/modules/issues/96) and [arbitrary sources](https://github.com/nodejs/modules/issues/97) and [specifier customization](https://github.com/nodejs/modules/issues/110) shipped in Node 13.7.0.
	- **Status**: Work remains to be done to support further use cases including [code coverage/instrumentation](https://github.com/nodejs/modules/issues/95) and [mocks/stubs](https://github.com/nodejs/modules/issues/98).

* [WASM modules](https://nodejs.org/api/esm.html#esm_experimental_wasm_modules) (work in progress).

### Wish List (Blocked)

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

### Not Planned

* Limited module type detection.
  - PR: https://github.com/nodejs/ecmascript-modules/pull/69.
  - Upstream PR: https://github.com/nodejs/node/pull/27808.
  - **Status**: The group’s current thinking is to encourage explicit source type definition via file extension or the `"type"` field. Source type detection via parsing source code is not 100% accurate and should therefore be left to userland where users can choose to accept its risks.

* Provide a way to make ESM the default instead of CommonJS.
  - Discussion: https://github.com/nodejs/modules/issues/318.
  - Proposal: https://github.com/nodejs/modules/issues/335.
  - **Status**: Currently one can add `--input-type=module` to `NODE_OPTIONS` to flip the default for `--input-type`; at the moment the group is deciding not to pursue providing an ability to flip the default for the `package.json` `type` field. We instead want to encourage all packages, both ESM and CommonJS, to include an explicit `type` field; leaving it out and relying on a default is something we want to discourage.

## Archives

As part of creating the ECMAScript Modules implementation for Node.js 12.0.0, [use cases](./doc/use-cases.md) were brainstormed, which led to a list of [features](./doc/features.md), which guided the [plan for the implementation](./doc/plan-for-new-modules-implementation.md).

## Members

<!-- ncu-team-sync.team(nodejs/modules-active-members) -->

- [@bmeck](https://github.com/bmeck) - Bradley Farias
- [@DanielRosenwasser](https://github.com/DanielRosenwasser) - Daniel Rosenwasser
- [@devsnek](https://github.com/devsnek) - Gus Caplan
- [@GeoffreyBooth](https://github.com/GeoffreyBooth) - Geoffrey Booth
- [@guybedford](https://github.com/guybedford) - Guy Bedford
- [@jkrems](https://github.com/jkrems) - Jan Olaf Krems
- [@ljharb](https://github.com/ljharb) - Jordan Harband
- [@MylesBorins](https://github.com/MylesBorins) - Myles Borins
- [@robpalme](https://github.com/robpalme) - Rob Palmer
- [@SMotaal](https://github.com/SMotaal) - Saleh Abdel Motaal
- [@targos](https://github.com/targos) - Michaël Zasso
- [@weswigham](https://github.com/weswigham) - Wesley Wigham

<!-- ncu-team-sync end -->

## Observers

<!-- ncu-team-sync.team(nodejs/modules-observers) -->

- [@A-lxe](https://github.com/A-lxe) - Alex Aubuchon
- [@benjamingr](https://github.com/benjamingr) - Benjamin Gruenbaum
- [@benjamn](https://github.com/benjamn) - Ben Newman
- [@bmacnaughton](https://github.com/bmacnaughton) - Bruce MacNaughton
- [@bmeurer](https://github.com/bmeurer) - Benedikt Meurer
- [@bizob2828](https://github.com/bizob2828) - Bob Evans
- [@ceejbot](https://github.com/ceejbot) - C J Silverio
- [@chrisdickinson](https://github.com/chrisdickinson) - Chris Dickinson
- [@clarkbw](https://github.com/clarkbw) - Bryan Clark
- [@coreyfarrell](https://github.com/coreyfarrell) - Corey Farrell
- [@darcyclarke](https://github.com/darcyclarke) - Darcy Clarke
- [@dduleone](https://github.com/dduleone) - Dan DuLeone
- [@DerekNonGeneric](https://github.com/DerekNonGeneric) - Derek Lewis
- [@devamaz](https://github.com/devamaz) - Ahmad Abdul-Aziz
- [@devongovett](https://github.com/devongovett) - Devon Govett
- [@eugeneo](https://github.com/eugeneo) - Eugene Ostroukhov
- [@evanplaice](https://github.com/evanplaice) - Evan Plaice
- [@Fishrock123](https://github.com/Fishrock123) - Jeremiah Senkpiel
- [@giltayar](https://github.com/giltayar) - Gil Tayar
- [@hiroppy](https://github.com/hiroppy) - Yuta Hiroto
- [@iarna](https://github.com/iarna) - Rebecca Turner
- [@inidaname](https://github.com/inidaname) - Hassan Sani
- [@jamiebuilds](https://github.com/jamiebuilds) - Jamie Kyle
- [@jasnell](https://github.com/jasnell) - James M Snell
- [@jdalton](https://github.com/jdalton) - John-David Dalton
- [@justinfagnani](https://github.com/justinfagnani) - Justin Fagnani
- [@linclark](https://github.com/linclark) - Lin Clark
- [@manekinekko](https://github.com/manekinekko) - Wassim Chegham
- [@mcollina](https://github.com/mcollina) - Matteo Collina
- [@mduleone](https://github.com/mduleone) - Matt DuLeone
- [@mhdawson](https://github.com/mhdawson) - Michael Dawson
- [@refack](https://github.com/refack) - Refael Ackermann (רפאל פלחי)
- [@robwormald](https://github.com/robwormald) - Rob Wormald
- [@rubys](https://github.com/rubys) - Sam Ruby
- [@ryzokuken](https://github.com/ryzokuken) - Ujjwal Sharma
- [@sendilkumarn](https://github.com/sendilkumarn) - Sendil Kumar N
- [@soldair](https://github.com/soldair) - Ryan Day
- [@tbjers](https://github.com/tbjers) - Torgny Bjers
- [@tbranyen](https://github.com/tbranyen) - Tim Branyen
- [@TheLarkInn](https://github.com/TheLarkInn) - Sean Larkin
- [@TimothyGu](https://github.com/TimothyGu) - Timothy Gu
- [@tzmanics](https://github.com/tzmanics) - Tara Z. Manicsic
- [@WebReflection](https://github.com/WebReflection) - Andrea Giammarchi
- [@XadillaX](https://github.com/XadillaX) - Khaidi Chu
- [@xtuc](https://github.com/xtuc) - Sven Sauleau
- [@yosuke-furukawa](https://github.com/yosuke-furukawa) - Yosuke Furukawa
- [@zackschuster](https://github.com/zackschuster) - Zack Schuster
- [@zenparsing](https://github.com/zenparsing) - Kevin Smith

<!-- ncu-team-sync end -->
