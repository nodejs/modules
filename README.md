# Modules Team

## Purpose

The Node.js Modules Team maintains and actively develops the ESModules (ESM) implementation in Node.js Core.

Work includes:

* ESM Loader implementation
* ESM Module Specifier Resolution Algorithm
* Loader Hooks
* ESM and CommonJS Interoperability
* Node.js and Browser interoperability
* VM Modules implementation

## Current Efforts

> ### [Current Plan for `--experimental-modules`](./doc/plan-for-new-modules-implementation.md)

A new ECMAScript modules implementation shipped behind the `--experimental-modules` flag in Node 12. The flag is scheduled to be dropped in November 2019 as part of a minor release of Node 13. A road map of the implementation’s development is [here](./doc/plan-for-new-modules-implementation.md).


## Features

Based on [these use cases](https://docs.google.com/document/d/10BBsIqdAXB9JR2KUzQGYbCiVugYBnxE4REBakX29yyo/edit) ([#55](https://github.com/nodejs/modules/issues/55)), our implementation aims to support the following features (subject to change):

### Baseline Modules Implementation Features:
* Spec compliance ([#132](https://github.com/nodejs/modules/issues/132))
* Browser equivalence ([#133](https://github.com/nodejs/modules/issues/133))
* Don’t break CommonJS ([#112](https://github.com/nodejs/modules/issues/112))
* No refactoring ([#87](https://github.com/nodejs/modules/issues/87))

#### Browser Interop:
* Browser and Node compatibility without building ([#107](https://github.com/nodejs/modules/issues/107))
* Browser-compatible specifier resolution ([#109](https://github.com/nodejs/modules/issues/109))
* Browser-compatible builds for ESM and CommonJS ([#108](https://github.com/nodejs/modules/issues/108))

#### CommonJS Interop:
* Named exports when importing CJS ([#81](https://github.com/nodejs/modules/issues/81))
* Multi-mode packages ([#94](https://github.com/nodejs/modules/issues/94))
* Transparent interoperability for ESM importing CommonJS ([#100](https://github.com/nodejs/modules/issues/100))
* Consumer-agnostic imports ([#105](https://github.com/nodejs/modules/issues/105))
* Mixed module types within app/module; gradual migration from CommonJS to ESM ([#99](https://github.com/nodejs/modules/issues/99))
* ESM in .js files ([#151](https://github.com/nodejs/modules/issues/151))

### Existing Node.js Utility Features:

* Importing non-JavaScript files ([#115](https://github.com/nodejs/modules/issues/115))
* NodeJS contextual pathing use cases ([#121](https://github.com/nodejs/modules/issues/121))
* ESM in executable files ([#152](https://github.com/nodejs/modules/issues/152))
* Callable resolver ([#157](https://github.com/nodejs/modules/issues/157))

### Loader Features:

* Code coverage/instrumentation ([#95](https://github.com/nodejs/modules/issues/95))
* Pluggable Loaders to support multiple use cases ([#82](https://github.com/nodejs/modules/issues/82))
* Runtime loaders, transpilation at import time ([#96](https://github.com/nodejs/modules/issues/96))
* Arbitrary sources for module source text ([#97](https://github.com/nodejs/modules/issues/97))
* Mock modules (injection) ([#98](https://github.com/nodejs/modules/issues/98))
* Specifier resolution customization ([#110](https://github.com/nodejs/modules/issues/110))
* Package encapsulation ([#111](https://github.com/nodejs/modules/issues/111))
* Conditional imports ([#113](https://github.com/nodejs/modules/issues/113))

### WASM Features:

* WASM modules (#[106](https://github.com/nodejs/modules/issues/106))

### Developer and Tooling Features:

* File / path / URL resolving (#[103](https://github.com/nodejs/modules/issues/103))
* Import CommonJS without needing asynchronous syntax (#[116](https://github.com/nodejs/modules/issues/116))
* Tree shaking (#[102](https://github.com/nodejs/modules/issues/102))
* Polyfillability (#[101](https://github.com/nodejs/modules/issues/101))

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
- [@darcyclarke](https://github.com/darcyclarke) - Darcy Clarke
- [@dduleone](https://github.com/dduleone) - Dan DuLeone
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
