## The Module Manifesto

We the undersigned agree on the the following,

* Getting modules "right" is one of the biggest existential threats to JavaScript
* This affects a large number of people on this planet (and future planets), with varying interests including but not limited to:
  - web applications
  - serverside applications and services
  - command line interfaces
  - libraries and frameworks
  - transpilers
  - robots
  - art and music
* Getting modules "right" is hard because of these varying interests
* Creating a good developer experience is the number one priority
* The aim will be to not compromise the Node.js Developer Experience, but we recognize that may be necessary to improve the health of the JavaScript ecosystem
* While compromise is neccessary, it is important to recognize when Node.js needs to lead and attempt to influence change at an industry level
* The closer the Node.js and Web platform remain the better the developer experience will be

* The Node.js implementation of ESModules must have the following features:
  - Compliant to ECMA-262. It is important to follow the standard. If we need changes in the standard we should attempt to accomplish them at TC39.
  - Interoperability. It is important to be able to utilize ESModule code from CommonJS and vice versa.
  - Hooks. It is important to be able to have hooks into the lifecycle of a Module.
  - Multiple Goals. e.g. wasm. It is important that our implementation scales to any number of future goals.
* Packages written with the Node.js implementation of ESModules must have the following features.
  - Dual mode. It is important that a single package should be capable of being loaded as both ESM and CJS. This does not have to be the default.
  - Web Compatible. It is important that a published package be capable of running on both the web and node.js platform without any source changes. This does not mean all APIs need to be compatible.

Without working together there will be no chance of a successful module system. "Together" is larger than the undersigned.

Signed,

Myles Borins (@MylesBorins)
Gus Caplan (@devsnek)
Jordan Harband (@ljharb)
Matteo Collina (@mcollina)
Benjamin Gruenbaum (@benjamingr)
Hassan Sani (@inidaname)
Michaël Zasso (@targos)
Ahmad Abdul-Aziz (@devamaz)
Jan Krems (@jkrems)
