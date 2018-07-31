Terminology
============

This document is used to keep track of common terminology related to modules in [Node.js](https://nodejs.org), focusing primarily on ECMAScript Modules as well as other mainstream module formats.

<dl>

Normative Terms
---------------
  
<dt id=agnostic-consumers>
  Agnostic Consumers
</dt>
<dd> 
  The ability for importer modules to be unaware of the imported module type (CJS vs ESM). This implies that the imported module can be migrated from CJS to ESM without impacting the consumer.

  This term alone does not specify in which direction(s) the agnosticism applies.
</dd>

<dt id=require-interop>
  Require Interoperability <code>require(<esm>)</code>
</dt>
<dd>
  
  The ability for a module to import an ESM module using the `require(…)` function.

  The module’s namespace is return directly, i.e. it is not wrapped in a promise.

  If it is impossible to load the graph synchronously, it is intended that the call must throw.
</dd>

<dt id=import-interop>
  Import Interoperability <code>import(<cjs>)</code>
</dt>
<dd>
  
  The ability for an ESM module to import a CJS module.

  Applies to both static `import … from ` and dynamic `import(…)`.

  This term alone does not specify whether named `exports` are extracted from the CJS exports object.
</dd>

<dt id="named-exports">
  Named Exports <code>exportKeys</code>
</dt>
<dd>
  
  The ability for a CJS module to elect to expose properties of its `exports` object as fixed set of named exports for use by ESM consumers, as opposed to having a single `export default` equal to the exports object.

  Some forms of CJS are not amenable, e.g. those that dynamically delete properties of the exports object.
</dd>

<dt id="consumer-disambiguation">
  Consumer-defined Disambiguation
</dt>
<dd>
  
  When importing a file into an ESM context, the parse goal of the file is determined by the importing module (the consumer). This is often accomplished via syntax, for example by using only `import` statements for ESM or `require` (or other function) for CommonJS.
  
</dd>

<dt id="author-disambiguation">
  Author-defined Disambiguation
</dt>
<dd>
  
  A file or module’s parse goal is included within itself. This is often achieved via a file extension such as `.mjs` or a field in a `package.json` file. Other suggestions have included a `"use module"` directive or unambiguous syntax, such as parsing the file for `import` or `export` statements.

</dd>
</dl>
  
Deprecated Terms
----------------

<dl>
<dt id=transparent-interop>
  Transparent Interoperability
</dt>
<dd>
        
  An imprecise term that refers to some subset of the following independent capabilities:
  
  1. [Agnostic Consumers](#agnostic-consumers) (in one or both directions)
  2. [Require Interoperability](#require-interop)
  3. [Import Interoperability](#import-interop) (with or without Named Exports from CJS)
</dd>
</dl>
