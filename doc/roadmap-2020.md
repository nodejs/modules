| This is a living document. It describes the policy and priorities as they exist today, but can evolve over time. |
|---|

# Node.js Modules Group 2020 Roadmap

_The goals below represent items currently planned for accomplishment in 2020. For additional items that have been recognized as desirable, see the [wishlist](./wishlist.md)._

---

* [Loader hooks](https://nodejs.org/api/esm.html#esm_hooks) (work in progress). The goal is to create hooks that provide enough functionality to enable users to build loaders that can achieve many of the remaining [features](./doc/features.md) we hope to support.
	- Overview: https://github.com/nodejs/modules/issues/351.
	- A [design](https://github.com/nodejs/node/pull/30986) that enables [transpilation](https://github.com/nodejs/modules/issues/96) and [arbitrary sources](https://github.com/nodejs/modules/issues/97) and [specifier customization](https://github.com/nodejs/modules/issues/110) shipped in Node 13.7.0.
	- **Status**: Work remains to be done to support further use cases including [code coverage/instrumentation](https://github.com/nodejs/modules/issues/95) and [mocks/stubs](https://github.com/nodejs/modules/issues/98).

* [WASM modules](https://nodejs.org/api/esm.html#esm_experimental_wasm_modules) (work in progress).

---

[**[ WISHLIST ]**](./wishlist.md) [**[ PREVIOUS WORK ]**](./previous-work.md)
