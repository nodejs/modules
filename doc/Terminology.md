Terminology
============

This document is used to keep track of common terminology related to modules in [Node.js](https://nodejs.org), focusing primarily on ECMAScript Modules as well as other mainstream module formats.

<dl>

Normative Terms
---------------
  
<dt id=agnostic-consumers>
  Agnostic Consumers
<dd> 

  The ability for importer modules to be unaware of the imported module type (CJS vs ESM). This implies that the imported module can be migrated from CJS to ESM without impacting the consumer.

  This term alone does not specify in which direction(s) the agnosticism applies.

<dt id=require-interop>
  Require Interoperability `require(<esm>)`
<dd>
  
  The ability for a module to import an ESM module using the `require(…)` function.

  The module’s namespace is return directly, i.e. it is not wrapped in a promise.

  If it is impossible to load the graph synchronously, it is intended that the call must throw.

<dt id=import-interop>
  Import Interoperability `import(<cjs>)`
<dd>
  
  The ability for an ESM module to import a CJS module.

  Applies to both static `import … from ` and dynamic `import(…)`.

  This term alone does not specify whether named `exports` are extracted from the CJS exports object.

<dt id="named-exports">
  Named Exports `exportKeys`
<dd>
  
  The ability for a CJS module to elect to expose properties of its `exports` object as fixed set of named exports for use by ESM consumers, as opposed to having a single `export default` equal to the exports object.

  Some forms of CJS are not amenable, e.g. those that dynamically delete properties of the exports object.
  
</dl>

<dl>
  
Deprecated Terms
----------------

<dt id=transparent-interop>
  Transparent Interoperability
<dd>
        
  An imprecise term that refers to some subset of the following independent capabilities:
  
  1. [Agnostic Consumers](#agnostic-consumers) (in one or both directions)
  2. [Require Interoperability](#require-interop)
  3. [Import Interoperability](#import-interop) (with or without Named Exports from CJS)

</dl>
