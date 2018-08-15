These are proposals for the user experience of an addition to `package.json` that configures Node to treat `.js` files as ESM within a package boundary. This assumes the `--experimental-modules` implementation as a baseline. This is spun off from [#150](https://github.com/nodejs/modules/issues/150), and relates to [nodejs/node/pull/18392](https://github.com/nodejs/node/pull/18392).

### [nodejs/node/pull/18392](https://github.com/nodejs/node/pull/18392)

This pull request adds support for a `"mode": "esm"` flag to be added to `package.json`, that tells Node to treat all `.js` files within the package boundary of that `package.json` file as ESM. The package boundary of a `package.json` file is deemed to be all modules in that folder and subfolders until the next nested `package.json` file.

In [#150](https://github.com/nodejs/modules/issues/150) we found consensus on adding a little more customizability than what would be offered by a simple boolean flag, but I wanted to include this here for completeness. Also, I expect that this PR would serve as the basis for a more full-featured implementation.

### MIME types/webserver as metaphor

In browsers, users must configure their webservers to serve `.js` or `.mjs` files with a JavaScript MIME type like `text/javascript` in order for the browser to recognize the file as ESM. The browsers pay no attention to file extensions, but most webservers use file extensions to determine what MIME types to use to serve files. This is similar to `AddType video/webm .webm` that you may have seen in Apache webserver configuration files.

The idea here is to mimic this webserver configuration in a new `package.json` section called `mimes`:

```json
"mimes": {
  "js": "application/node",
  "mjs": "text/javascript",
  "json": "application/json"
}
```

In this example, `.js` files would be treated as CommonJS, which has the MIME type `application/node`. `.mjs` files would be treated as ESM (`text/javascript`, or `application/javascript`) and `.json` files would be treated as JSON. The configuration in this example would be the default, what Node uses if a `mimes` field was missing, as historically `.js` files were always treated as CommonJS.

Here’s another example:

```json
"mimes": {
  "js": "text/javascript",
  "cjs": "application/node"
}
```

This tells Node to treat `.js` files within this package boundary as ESM, and a new `.cjs` file extension as CommonJS. The `.cjs` extension could be anything—just as in Apache a user could add the configuration `AddType video/webm .foo`, this `mimes` block provides the flexibility for new file extensions to be defined and mapped to MIME types that Node understands. Since `.mjs` isn’t listed here, Node falls back to the default mapping for it (`text/javascript`), and likewise for any other file extensions that Node recognizes by default (`.json`, etc.).

### Preconfigured MIMEs databases

This is based on [this proposal](https://gist.github.com/bmeck/7ee7eb2147e2dafe3167c856d9b4151a) by @bmeck, and is similar to the previous proposal. In this version, the `mimes` field takes a string or array of strings, where each string indicates a preset MIME database:

```json
"mimes": "cjs"
```

This would be the default, which corresponds to the first `mimes` block in the previous proposal (and to the `--experimental-modules` current behavior).

```json
"mimes": [null, "esm"]
```

This would blank out any default extension/MIME mapping within the current package boundary, and then apply the `"esm"` preset (which maps both `.mjs` and `.js` to ESM).

### Map from extension to extension

This is based on [this comment](https://github.com/nodejs/modules/pull/150#issuecomment-406838613) from @ljharb. It would map file extensions to other file extensions (@ljharb please expand upon your idea and I’ll update this section, as there wasn’t an example in the comment):

```json
"extensions": {
  "js": "mjs"
}
```

This would tell Node to treat all `.js` files as `.mjs` files within the package boundary—in other words, to treat all `.js` files as ESM.

### Map from extension to parse goal

Similar to the previous, this would define parse goals for extensions directly:

```json
"parseGoals": {
  "mjs": "module",
  "js": "commonjs"
}
```

I presume that this would be limited to JavaScript-like files, unless things like `.json` or `.node` could have parse goals.

### Consensus: “MIME types/webserver as metaphor” with links to external JSON files

Start with the second proposal from this list, “MIME types/webserver as metaphor”, and its new `mimes` section in `package.json` that can be an object, e.g.:

```json
"mimes": {
  "js": "text/javascript",
  "cjs": "application/node"
}
```

That `mimes` section could alternatively take an array of strings, which would be relative or resolved paths to JSON files:

```json
"mimes": [
  "./my-mimes.json",
  "typescript/mimes.json"
]
```

Each JSON file would be an object with “extension: mime” mappings like the first example. They would be combined using `Object.assign`. If the first element in the array is `null`, Node’s default MIME mappings are discarded first before the array elements are merged, e.g. something like:

```js
const nodeDefaultMimeMappings = require('module').mimeMappings;

const packageJsonMimes = require('./package.json').mimes;
if (Array.isArray(packageJsonMimes)) {
  let mimeMappings = (packageJsonMimes[0] === null) ? Object.create(null) : nodeDefaultMimeMappings;
  packageJsonMimes.forEach((filePath, index) => {
    if (filePath === null && index !== 0) {
      throw new Error('Only the first element of a mimes array may be null');
    }
    Object.assign(mimeMappings, require(filePath));
  });
  return mimeMappings;
} else {
  return Object.assign(nodeDefaultMimeMappings, packageJsonMimes);
}
```

