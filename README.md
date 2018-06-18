# Modules Team

## Purpose

The Node.js Modules Team maintains and actively develops the ESModules (ESM) implementation in Node.js Core.

Work includes:

* ESM Loader implementation
* ESM Module Specifier Resolution Algorithm
* Loader Hooks
* ESM and Common.js Interoperability
* Node.js and Browser interoperability
* VM Modules implementation

## Features

Based on [these use cases](https://docs.google.com/document/d/10BBsIqdAXB9JR2KUzQGYbCiVugYBnxE4REBakX29yyo/edit) ([#55](https://github.com/nodejs/modules/issues/55)), our implementation aims to support the following features:

- Spec compliance ([#132](https://github.com/nodejs/modules/issues/132))
- Browser equivalence ([#133](https://github.com/nodejs/modules/issues/133))
- No refactoring ([#87](https://github.com/nodejs/modules/issues/87))
- Pluggable Loaders to support multiple use cases ([#82](https://github.com/nodejs/modules/issues/82))
- Named exports when importing CJS ([#81](https://github.com/nodejs/modules/issues/81))
- Dual-goal packages ([#93](https://github.com/nodejs/modules/issues/93))
- Multi-mode packages ([#94](https://github.com/nodejs/modules/issues/94))
- Code coverage/instrumentation ([#95](https://github.com/nodejs/modules/issues/95))
- Runtime loaders, transpilation at import time ([#96](https://github.com/nodejs/modules/issues/96))
- Arbitrary sources for module source text ([#97](https://github.com/nodejs/modules/issues/97))
- Mock modules (injection) ([#98](https://github.com/nodejs/modules/issues/98))
- Mixed module types within app/module; gradual migration from CommonJS to ESM ([#99](https://github.com/nodejs/modules/issues/99))
- Transparent interoperability for ESM importing CommonJS ([#100](https://github.com/nodejs/modules/issues/100))
- Polyfillability ([#101](https://github.com/nodejs/modules/issues/101))
- Tree shaking ([#102](https://github.com/nodejs/modules/issues/102))
- File / path / URL resolving ([#103](https://github.com/nodejs/modules/issues/103))
- Retrievable module metadata ([#104](https://github.com/nodejs/modules/issues/104))
- Transparent migration ([#105](https://github.com/nodejs/modules/issues/105))
- WASM modules ([#106](https://github.com/nodejs/modules/issues/106))
- Browser and Node compatibility without building ([#107](https://github.com/nodejs/modules/issues/107))
- Browser-compatible build that includes CommonJS ([#108](https://github.com/nodejs/modules/issues/108))
- Browser-compatible specifier resolution ([#109](https://github.com/nodejs/modules/issues/109))
- Specifier resolution customization ([#110](https://github.com/nodejs/modules/issues/110))
- Package encapsulation ([#111](https://github.com/nodejs/modules/issues/111))
- Don’t break CommonJS ([#112](https://github.com/nodejs/modules/issues/112))
- Conditional imports ([#113](https://github.com/nodejs/modules/issues/113))
- Import JSON without needing asynchronous syntax ([#114](https://github.com/nodejs/modules/issues/114))
- Importing non-JavaScript files ([#115](https://github.com/nodejs/modules/issues/115))
- Import CommonJS without needing asynchronous syntax ([#116](https://github.com/nodejs/modules/issues/116))
- NodeJS contextual pathing use cases ([#121](https://github.com/nodejs/modules/issues/121))
- Load ESM over https ([#127](https://github.com/nodejs/modules/issues/127))


## Members

<!-- ncu-team-sync.team(nodejs/modules-active-members) -->

- [@benjamingr](https://github.com/benjamingr) - Benjamin Gruenbaum
- [@benjamn](https://github.com/benjamn) - Ben Newman
- [@bmeck](https://github.com/bmeck) - Bradley Meck
- [@ceejbot](https://github.com/ceejbot) - C J Silverio
- [@chrisdickinson](https://github.com/chrisdickinson) - Chris Dickinson
- [@DanielRosenwasser](https://github.com/DanielRosenwasser) - Daniel Rosenwasser
- [@devsnek](https://github.com/devsnek) - Gus Caplan
- [@Fishrock123](https://github.com/Fishrock123) - Jeremiah Senkpiel
- [@GeoffreyBooth](https://github.com/GeoffreyBooth) - Geoffrey Booth
- [@giltayar](https://github.com/giltayar) - Gil Tayar
- [@guybedford](https://github.com/guybedford) - Guy Bedford
- [@iarna](https://github.com/iarna) - Rebecca Turner
- [@inidaname](https://github.com/inidaname) - Hassan Sani
- [@jdalton](https://github.com/jdalton) - John-David Dalton
- [@jkrems](https://github.com/jkrems) - Jan Olaf Krems
- [@justinfagnani](https://github.com/justinfagnani) - Justin Fagnani
- [@linclark](https://github.com/linclark) - Lin Clark
- [@ljharb](https://github.com/ljharb) - Jordan Harband
- [@manekinekko](https://github.com/manekinekko) - Wassim Chegham
- [@mcollina](https://github.com/mcollina) - Matteo Collina
- [@mduleone](https://github.com/mduleone) - Matt DuLeone
- [@mhdawson](https://github.com/mhdawson) - Michael Dawson
- [@MylesBorins](https://github.com/MylesBorins) - Myles Borins
- [@robpalme](https://github.com/robpalme) - Rob Palmer
- [@targos](https://github.com/targos) - Michaël Zasso
- [@tbjers](https://github.com/tbjers) - Torgny Bjers
- [@weswigham](https://github.com/weswigham) - Wesley Wigham
- [@XadillaX](https://github.com/XadillaX) - Khaidi Chu

<!-- ncu-team-sync end -->

## Observers

<!-- ncu-team-sync.team(nodejs/modules-observers) -->

- [@bmeurer](https://github.com/bmeurer) - Benedikt Meurer
- [@dduleone](https://github.com/dduleone) - Dan DuLeone
- [@devamaz](https://github.com/devamaz) - Ahmad Abdul-Aziz
- [@eugeneo](https://github.com/eugeneo) - Eugene Ostroukhov
- [@evanplaice](https://github.com/evanplaice) - Evan Plaice
- [@hiroppy](https://github.com/hiroppy) - Yuta Hiroto
- [@jasnell](https://github.com/jasnell) - James M Snell
- [@refack](https://github.com/refack) - Refael Ackermann
- [@robwormald](https://github.com/robwormald) - Rob Wormald
- [@tbranyen](https://github.com/tbranyen) - Tim Branyen
- [@TheLarkInn](https://github.com/TheLarkInn) - Sean Larkin
- [@TimothyGu](https://github.com/TimothyGu) - Timothy Gu
- [@WebReflection](https://github.com/WebReflection) - Andrea Giammarchi
- [@xtuc](https://github.com/xtuc) - Sven SAULEAU
- [@yosuke-furukawa](https://github.com/yosuke-furukawa) - Yosuke Furukawa
- [@zackschuster](https://github.com/zackschuster) - Zack Schuster
- [@tzmanics](https://github.com/tzmanics) - Tara Z. Manicsic
- [@zenparsing](https://github.com/zenparsing) - Kevin Smith

<!-- ncu-team-sync end -->
