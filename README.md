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

Based on these use cases (#55), our implementation aims to support the following features (subject to change):

### Baseline Modules Implementation Features:
* Spec compliance (#132)
* Browser equivalence (#133)
* Don’t break CommonJS (#112)
* No refactoring (#87)

#### Browser Interop:
* Browser and Node compatibility without building (#107)
* Browser-compatible specifier resolution (#109)
* Browser-compatible builds for ESM and CommonJS (#108)

#### CommonJS Interop:
* Named exports when importing CJS (#81)
* Multi-mode packages (#94)
* Transparent interoperability for ESM importing CommonJS (#100)
* Consumer-agnostic imports (#105)
* Mixed module types within app/module; gradual migration from CommonJS to ESM (#99)
* ESM in .js files (#151)

### Existing Node.js Utility Features:

* Importing non-JavaScript files (#115)
* NodeJS contextual pathing use cases (#121)
* ESM in executable files (#152)
* Callable resolver (#157)

### Loader Features

* Code coverage/instrumentation (#95)
* Pluggable Loaders to support multiple use cases (#82)
* Runtime loaders, transpilation at import time (#96)
* Arbitrary sources for module source text (#97)
* Mock modules (injection) (#98)
* Specifier resolution customization (#110)
* Package encapsulation (#111)
* Conditional imports (#113)

### WASM Features:

* WASM modules (#106)

### Developer and Tooling Features:

* File / path / URL resolving (#103)
* Import CommonJS without needing asynchronous syntax (#116)
* Tree shaking (#102)
* Polyfillability (#101)

Based on [these use cases](https://docs.google.com/document/d/10BBsIqdAXB9JR2KUzQGYbCiVugYBnxE4REBakX29yyo/edit) ([#55](https://github.com/nodejs/modules/issues/55)), our implementation aims to support the following features (subject to change):

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
- [@xtuc](https://github.com/xtuc) - Sven Sauleau
- [@yosuke-furukawa](https://github.com/yosuke-furukawa) - Yosuke Furukawa
- [@zackschuster](https://github.com/zackschuster) - Zack Schuster
- [@tzmanics](https://github.com/tzmanics) - Tara Z. Manicsic
- [@zenparsing](https://github.com/zenparsing) - Kevin Smith

<!-- ncu-team-sync end -->
