This PR is meant for discussion of responses to [this issue](https://github.com/nodejs/modules/issues/149). I’ve opened it as a PR so that we can start separate discussion threads under the various proposals, with the PR commenting-on-code interface keeping things organized. If people suggest new proposals, or revisions to current ones, I’ll update the PR accordingly.

## Proposals

### Do nothing

Fixing the issue isn’t necessary; or the least-bad solution is worse than the issue itself.

### `import.meta.require`

Node ES `import` statements would not be able to import CommonJS JavaScript, just as browsers’ `import` statements can’t import CommonJS (or Script, a.k.a. non-ESM) JavaScript. If a user wants to import CommonJS, they need to use a different API.

`import.meta.require` would be that new API, where `require` is our old friend from CommonJS (or as close as can be): `import.meta.require('./commonjs-file.js')`.

### `import { importCommonJS } from 'module'`

The same as `import.meta.require`, but not attaching a nonstandard method to `import.meta`. A new function on a Node core module would take two arguments: `import.meta`, to know the context of the importing module, and a string that’s the import specifier. For example:
	
```js
import { importCommonJS } from 'module';
importCommonJS(import.meta, './commonjs-file.js');
```

### `.cjs`

If a user wants to use the `import` statement to import a CommonJS file, that file needs to have a `.cjs` extension. This would allow intermixing of ESM `.js` files and CommonJS `.cjs` files, and potentially the use of CommonJS in browser environments. The `.cjs` extension would map to `application/node`, even on the Web.

Since no `.cjs` files currently exist, this solution either requires renaming files or creating symlinks, e.g. `file.cjs` pointing at `file.js`. This solution definitively ends the ambiguity inherent in the `.js` extension: a user could use `.mjs` and `.cjs` exclusively and purse ambiguity from their filenames.

### `package.json` flag to tell Node how to handle `.js` files

This would be https://github.com/nodejs/node/pull/18392 or something like it. That PR proposes a `"mode": "esm"` to tell Node that all `.js` files within that `package.json`’s package boundary should be treated as ESM files.

### `package.json` section to tell Node how to handle any file

Similar to https://github.com/nodejs/node/pull/18392, but this would be a new section in `package.json` where the user can treat Node like a configurable webserver, and define the MIME types it should use to “serve” files that get imported:

```json
"mimes": {
  "js": "application/node",
  "mjs": "text/javascript",
  "json": "application/json"
}
```

This makes explicit the equivalence between Node’s deciding how to interpret files and webservers choosing MIME types for those files. A user could edit the above to set `js` to be `text/javascript`, for example, and/or add `cjs` for `application/node`.

### [Unambiguous grammar](https://github.com/bmeck/UnambiguousJavaScriptGrammar), defaulting to ESM

In the case of `import './mystery.js'`, the presence of references inside `mystery.js` to any of CommonJS’ magic globals—`require`, `module`, `exports`, `__filename`, `__dirname`—would trigger parsing as CommonJS, while otherwise the file is treated as ESM.

### Unambiguous grammar, defaulting to CommonJS

The opposite of the above. The presence of an `import` or `export` statement inside `mystery.js` would cause Node to treat the file as ESM; otherwise, the file is assumed to be CommonJS. A `.js` file that doesn’t import or export anything would need an empty export, like `export {}`, to trigger detection as ESM.

This doesn’t solve the issue presented in the [demo](https://github.com/GeoffreyBooth/browser-equivalence-edge-case), because files that are truly ambiguous (fail both the “looking for CommonJS” and the “looking for ESM” tests) are treated as ESM by browsers.

### Import interoperability only via a loader, defaulting to ESM

`import` statements in Node would by default only load JavaScript as ESM, matching browser behavior. If one wanted to have an `import` statement load CommonJS, a loader would need to be used. Various loaders could be written to enable importing CommonJS in various ways, either via the `import` statement or by adding a new API like `import.meta.require` or via some other method.

### `import` statements work _only_ via loaders

By default, `import` statements don’t function at all. The user must pass in a CLI flag to tell Node what loader to use for `import` statements:

```bash
node --loader=esm index.mjs
```

The `esm` loader itself would be part of core, and would itself need to be written in CommonJS.

### `"use module"`

JavaScript files with a `"use module";` statement in the code, before any other statements, are treated as ESM. This was suggested to TC39 and rejected, but there’s nothing invalid about it. It has the advantage that it’s an author-defined signal that a file should be treated as ESM, like the `.mjs` extension; but because it’s in the source code itself, it’s compatible with build pipelines that don’t support `.mjs` or multiple extensions for JavaScript.
