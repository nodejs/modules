## ESM Resolver Algorithm Specification

As with the CommonJS resolver, one goal of the modules implementation is to provide a straightforward resolver specifiation that can be followed by tools and bundlers to properly adhere to the Node.js resolution semantics.

The resolver algorithm here covers the current plans for the [Minimal Kernel Modules implementation](https://github.com/nodejs/modules/blob/master/doc/plan-for-new-modules-implementation.md) and will change as that work progresses.

### Features

Currently, as per the minimal kernel, the resolver has the following properties:

* FileURL-based resolution as is used by ES modules
* Relative and absolute URL resolution
* No default extensions
* No directory lookups
* Bare specifier package resolution lookup through node_modules

### Package main

This implementation differs from the CommonJS resolver in detecting a package main request based on the package name format itself being either `package` or `@scoped/package` and then applying the main lookup only for those cases.

The main lookup is then provided by _CHECK_PACKAGE_MAIN_ which is still an experimental approach under discussion.

Currently the main lookup will only check for _index.mjs_.

### Resolver Algorithm

The algorithm to resolve an ES module specifier is provided through _ESM_RESOLVE_:

ESM_RESOLVE(specifier, parentURL)
> 1. If _specifier_ starts with _"/", _"./"_ or _"../"_ then,
>    1. Return the URL resolution of _specifier_ to _parentURL_.
> 1. If _specifier_ is a valid URL then,
>    1. Return the result of parsing and reserializing _specifier_ as a URL.
> 1. Note: _name_ is now a bare specifier.
> 1. Let _packageName_ be _undefined_.
> 1. Let _packagePath_ be _undefined_.
> 1. If _name_ does not start with _"@"_ then,
>    1. Set _packageName_ to the substring of _specifier_ until the first _"/"_ separator or the end of the string.
> 1. If _name_ starts with _"@"_ then,
>    1. If _name_ does not contain a _"/"_ separator then,
>       1. Throw a _Invalid Package Name_ error.
>    1. Set _packageName_ to the substring of _specifier_ until the second _"/"_ separator or the end of the string.
> 1. Let _packagePath_ be the substring of _specifier_ from the position at the length of _packageName_ plus one, if any.
> 1. Return the result of _PACKAGE_RESOLVE(specifier, parentURL)_.

PACKAGE_RESOLVE(packageName, packagePath, parentURL)
> 1. Assert: _packagePath_ contains no leading separator and can be empty.
> 1. Assert: _packageName_ is a valid package name or scoped package name.
> 1. Note: Further package name encoding validations can be implemented here.
> 1. Let _parentURL_ be the parent folder URL of _parentURL_.
> 1. While _parentURL_ contains a non-empty _pathname_,
>    1. Let _packageURL_ be equal to _"${parentPath}/node_modules/${packageName}"_.
>    1. If _packagePath_ is not empty then,
>       1. Let _url_ be equal to _"${packageURL}/${packagePath}"_.
>       1. If the file at _url_ exists then,
>          1. Return _url_.
>    1. Otherwise,
>       1. Let _packageMain_ be the result of _CHECK_PACKAGE_MAIN(packageURL)_.
>       1. If _packageMain_ is not _undefined_ then,
>          1. Return _packageMain_.
>    1. Set _parentURL_ to the parent URL path of _parentURL_.
> 1. Throw a _Module Not Found_ error.

CHECK_PACKAGE_MAIN(packageURL)
> 1. If the file at _"${packageURL}/index.mjs"_ exists then,
>    1. Return _"${packageURL}/index.mjs"_.
> 1. Return _undefined_.
