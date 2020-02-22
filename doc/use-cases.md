# ES module use cases

> The below use cases were written before the new ECMAScript modules implementation that was created for Node 12.0.0. They were originally written in Google Docs and are archived here.

#### @ceejbot

1.  Alice is writing a new library, and she is excited to use the new ES6 syntax. However, she would like to use an older but still good package she found on npm, that exports its interface using CommonJS. She does so easily after reading the Node.js documentation on how to do this.
2.  Bob is updating a module for his work, and he needs to support existing CommonJS-using codebases as well as a new project that prefers to stick with ES6 for static analysis reasons. He publishes a package that exports both kinds of interfaces.[\[a\]](#cmnt1)
3.  Carol is updating her popular code coverage reporting tool for the new world. She is able to instrument test code as it is imported so she can get code coverage reporting on par with what she has for CommonJS.

5.  David is writing a transpiler. He’s able to transpile source on demand from his language to JavaScript.
6.  Eugenia wants to abandon the CommonJS API for her existing npm package and move to a new ESM-only API.
7.  Hermione wants to load modules from a database[\[b\]](#cmnt2) and not from the filesystem and is able to do so.
8.  Ian wants to replace the dependencies of a module with mocks while testing and is able to do so.

#### @ljharb

8.  Frances has a large application and wants to be able to gradually migrate from CJS to ESM, one file at a time, without forcing codebase-wide changes.

39.  George[\[c\]](#cmnt3), like Bob, wants to update a package to use ESM, but needs to support existing CJS-using codebases, but wants to adopt the new syntax one file at a time, without forcing changes in their own codebase and without creating a breaking (semver-major) change.

9.  Jacqueline has a non-node application that is unable to take advantage of a node-based build system, but still wants to consume JavaScript code that is typically built in node. They are able to programmatically determine if they need a <script>or <script type="module"> without having a JavaScript parser available.

10.  Karl is yet unable to use ESM in their own code, because their company is still unwilling to change their coding style and workflow. However, they want to use Eugenia's ESM-only package. They are able to access it with require and make use of it, both while still on an older version of node (that lacks ESM support) and after upgrading to a newer version of node (that has ESM support).

#### @jkrems

11.  Carol is updating her popular code coverage reporting tool for the new world. She can get code coverage reporting on par with what she has for CommonJS.

12.  Alice is using an existing npm package that is written in CommonJS. She wants to use one specific function exported from its module.exports and wants to ensure she does it in a way that bundlers will understand while tree-shaking. There are clear best practices she can look up to do so.

13.  Robin reads the source of one of their projects and finds an import statement. They want to quickly find the file being imported. After a few steps they find the imported file in their file system.
    (q: Is this runtime? Debug? Static? require.resolve)

14.  What are people doing with CommonJS in node

15.  What are people doing in other module ecosystems outside of node

16.  What do people like about using the module system in node

17.  What do people dislike / miss when using the module system in node

18.  Quinn refactors an app and switches it from its existing CommonJS implementation to using ES modules. They can rewrite the code after reading the node docs without relying on trial-and-error or reading all READMEs (or even source) of their dependencies.

19.  Joe has written an ES module that leverages WASM internally for the web. The module uses import to load the compiled WASM file and everything worked fine in the browser. Joe wants to import the ES module from a server-side module and can do so without having to change the imported file.

####

@weswigham

20.  As a maintainer of a legacy project, I frequently have to update old code. As I update bits and bobs of older code, I integrate modern features and standards piecewise. When I update one of my files, I'd like to adopt some pieces of modern ES syntax where possible (similarly to how I adopted classes or async functions), while keeping any changes I need to make local to my original update and my API unchanged.

#### @guybedford

21.  Justin, a seasoned Node.jsdeveloper, has no desire to use the new ES module features and wants to continue using CommonJS modules with his existing dependencies. He doesn't want his application to break or slow down beyond the standard deprecation paths in Node as he upgrades to new versions of Node.js in his app.
22.  Alex is writing a universal library using both ES modules and existing CommonJS modules which she would like to build for both Node.js and the browser.

#### @robpalme

23.  Mike, a maintainer of a module system that allows intra-package absolute specifiers (“/top-level-dir/module.js”)relative to package-root to avoid long unreadable “../../../top-level-dir/module.js” chains, wants to migrate to a Node-based ES module world that provides something just as terse.

24.  Thomas, a maintainer of a module system that uses “@”\-prefixed bare-specifiers to refer to dependencies via an out-of-band manifest file (that defines the mapping similar to package-name-maps), wants to ensure these can be loaded by NodeJS without a build step.

25.  Peter, a maintainer of a module system that only permits deep (non-main) inter-package imports via named entrypoints (in order to allow safe refactoring of published files that are both considered implementation details and are not exposed via the package.json “main” property), wants to ensure the same level of safe-guarding/privacy can be achieved by future Node.js packages want this and opt-into this feature.

(link to friendly package proposal: [https://github.com/mikesamuel/tc39-module-keys](https://www.google.com/url?q=https://github.com/mikesamuel/tc39-module-keys&sa=D&ust=1580622748829000))

26.  Charles, a maintainer of a module system that allows alternative resolution when running tests in order to achieve convention-driven mocking (import “./x”prefers “./x.test.js” if it exists, rather than the usual “./x.js”), would like to achieve the same in future NodeJS.

27.  Vlad, a package author who sometime forgets to list all their dependencies in package.json, would like to run their NodeJS application in a strict asserting mode where name resolution will fail if they accidentally import:

*   bare-specifiers not explicitly listed as dependencies
*   relative-specifiers that wander into dependencies, i.e. node\_modules

(The former can be achieved today for the top level project by installing dependencies with the npmoption \--global-style at the cost of having one layer of nesting of installed modules. That is to day, all transitive dependencies of your direct dependencies will be installed under the transitive dependency. This also implies less deduplication.)

#### @justinfagnani

28.  John, a web developer, wants to write a module that imports dependencies and works natively on the web, without using any build tools.

29.  Shawn, a web developer, wants to use modules from npm on the web natively, without any source transformations during development. She wants any tools she does use to understand where dependencies are: their IDE must be able to open up dependencies, their linter and build system must find them, etc. (tools to agree on what a specifier means)

30.  Amy, a web developer, is writing a small sample module on jsbin that needs to use some dependencies from npm. (e.g. the unpkg use case)


31.  Geoff, a web developer, wants to deploy his app to production, but would like to serve common modules, like RxJs, from a CDN to increase performance and cache hits for his users.

32.  Alex would like to publish a module intended to be used by web and Node developers.

33.  Sarah would like to depend on a dev build at dev time and a prod build in production.

#### @mcollina

34.  Mike develops the agent of an Application Performance Monitoring (APM), which in order to work has to do some monkey patching.
35.  Sarah uses an APM to monitor their application in production. They expect the APM provider to work with both CJS and ESM.

#### @geoffreybooth

36.  Jennifer has a test runner that loads other JavaScript files for unit testing. Some of the files to be tested are CJS and others are ESM. The test runner passes the files through transpilation before testing them, e.g.
    mocha --compilers js:babel-core/register, but the importand export statements aren’t converted to CommonJS.

37.  (Real world use case): I want to rewrite [CoffeeScript’s modules tests](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/test/modules.coffee&sa=D&ust=1580622748833000)from being string comparisons to instead actually execute importand export statements[\[d\]](#cmnt4). The [test runner](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/Cakefile%23L382-L470&sa=D&ust=1580622748834000)is written in CommonJS, and needs to test both transpiled [CommonJS strings](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/test/importing.coffee&sa=D&ust=1580622748834000) and transpiled ES module strings output from the new modules tests.

38.  Anne is using a tool that generates JavaScript, and the tool spawns a Node.jsprocess to execute it. The tool might generate code with importand exportstatements, which should get evaluated. [For example](https://www.google.com/url?q=https://github.com/nodejs/node/issues/19725&sa=D&ust=1580622748835000):node --eval 'import path from "path"; console.log(path.sep);'

Note: #39 is positioned after #8.

40.  Chris has been working on a Node.jsapp for several years. It uses Babel for transpilation, and imports many CommonJS packages; the app always uses importand exportstatements, never require. He has upgraded to the latest Node.js, and has noticed that the only Babel transform his app still needs is for modules; all other syntaxes and features that he’s using are supported natively by the Node runtime. He removes Babel from his app and it still works the same as before.

41.  (Real world use case): The CoffeeScript module uses CommonJS features that don’t exist in ESM, like [conditionally executed](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/Cakefile%23L485-L493&sa=D&ust=1580622748837000) [require](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/Cakefile%23L485-L493&sa=D&ust=1580622748837000)[ statements](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/master/Cakefile%23L485-L493&sa=D&ust=1580622748837000). I don’t plan to refactor the package for ESM, but I still want my module to be importable into either CommonJS or ESM modules.

#### @DanielRosenwasser

42.  Rachel is a TypeScript user who is importing some JavaScript code that uses CommonJS. She uses declaration files that were written on DefinitelyTyped, but were authored as ES module top-level exports as so:

    export function foo() {
    }
    export function bar() {
    }

    When she imports them from TypeScript, she gets code-completion on the namespace import for \`foo\` and \`bar\`

    import \* as thing from "some-package";
    thing./\* completions here\*/

    When she compiles her code to run on either the 'commonjs' or 'esnext' module targets, she expects both to run correctly.

@MylesBorins

43.  Nancy wants to be able to write Typescript code and have it “just work” in node with minimal setup.

44.  Jon would like to write code that just works in multiple environments

45.  Lorin wants to support esm, commonjs, and jsx without a build step

#### @smotaal

46.  Eric frequently wants to reuse modules which depend on other relative modules switching back and forth between the browser and Node, without having to change specifiers or use transpilers. They are more interested in one of two workarounds, either proactively manifesting (sorry) their path maps or retroactively being able to gracefully catch those specifiers that are not valid for the current platform during execution. Ultimately, those mechanisms should be universally aligned, ie not using attributes on the script tags, or special switches on the command line.

#### @geoffreybooth

47.  Sabrina is writing a command-line Node app that has a \--versionoption to return its version number. She doesn’t want to have to maintain the version number both in package.jsonas well as in her app code, so she writes { version } = require(‘package.json’)to pull the version number from her package.json file. (Real world example: [https://github.com/jashkenas/coffeescript/blob/bd824c73dd93f6582bacc40c125cff04a796b493/src/coffeescript.coffee#L10-L15](https://www.google.com/url?q=https://github.com/jashkenas/coffeescript/blob/bd824c73dd93f6582bacc40c125cff04a796b493/src/coffeescript.coffee%23L10-L15&sa=D&ust=1580622748839000)) Same use case applies to configuration stored in package.json, such as is currently used by Babel and Browserify and Meteor and other projects.

48.  Sabrina can write her app in a synchronous style, without needing to await promises or use callbacks. So this current code:

    const { version } = require(‘./package.json’);
    console.log(\`Running version ${version}\`);Can be written using an import statement as:

    import { version } from ‘./package.json’;
    console.log(\`Running version ${version}\`);

    Assume likewise for .js files, not just JSON.

49.  Jill can import JSON object keys that are invalid JavaScript identifiers. In other words:
    const packageJson = require(‘./package.json’);
    const browserifyOptions = packageJson\[‘browserify-options’\];

    Can be written as something like:

    import \* as packageJson from './package.json';
    const browserifyOptions = packageJson\['browserify-options'\];

    or

    packageJson.default\['browserify-options'\];

#### @ljharb

50.  Paul wants to initialize shared state (like an autoincrementing integer ID, or installing a polyfill), in a module that is used by multiple entry points - currently authored in CJS. This state must be initialized prior to all other JS code running in that entry point, so current entry points have \`require(‘./init’)\` as the top line of the file. Paul wants to migrate one of the entry points to ESM (such that \`import ‘./init’\` is the top line of the file) without having to modify the other entry points, and without having to modify the “init” module.

51.  Jordan has a shim for a new JS language feature. That shim must be loaded prior to all non-shim JS in the app, in both browser and node. The shim needs to be consumed by both CJS and ESM modules, ideally without using a build process.

#### @smotaal

52.  Mike/Thomas use a \*source transformation NodeJS ESM migration tool\* to rewrite “local” specifiers through well-defined mapping strategies- ie package-scoped absolute notation using Path mapping or prefixed/unprefixed bare notation using Name mapping - into new files which allows them to migrate their non-conforming ES modules to conforming ES2015+ modules.

53.  Mike/Thomasuse a \*translation/transpilation runtime hook\* to rewrite “local” specifiers through well-defined mapping strategies - ie package-scoped absolute notation using Path mapping or prefixed/unprefixed bare notation using Name mapping - which allows them to consume their unchanged modules in NodeJS via the custom hooks which will be configured in the package.json to optionally be picked up by the loader when configured accordingly.

54.  TypeScript/CoffeeScript use a \*NodeJS Module Resolution API to resolve module paths as well as statically defined exports for CJS and conforming ES2015+ modules.

55.  Vlad uses a \*NodeJS Module Resolution API\* or a \*NPM tool that uses the same API\* that will trace all imported and required modules which allows him to ensure that all dependencies are properly defined in his package.json and to flag any malformed references to node\_modules (ie using relative paths).[\[e\]](#cmnt5)

@MylesBorins

56.  Amir npm installs a module and writes a javascript file that uses it. They are then able to run the javascript file in both the browser and node without requiring a build step

57.  Shelly wants to import a json file.

58.  Joaquin writes a module using import / export syntax and it works identically in both the browser and node.

@devsnek / @GeoffreyBooth (per [https://github.com/nodejs/modules/issues/124#issuecomment-394782942](https://www.google.com/url?q=https://github.com/nodejs/modules/issues/124%23issuecomment-394782942&sa=D&ust=1580622748842000))

59.  Tom expects that when Node.js is doing something from the language spec, it does it correctly.

60.  Tom expects that when Node.js is doing something with modules that browsers also do, Node.js and browsers do it the same way.

[\[a\]](#cmnt_ref1)This assumes that a package needs to do something to﻿ be imported by either module mode. I don't think the use case should assume this, that's an implementation detail. Wouldn't a better implementation be where packages needn't do anything to be imported anywhere?

[\[b\]](#cmnt_ref2)Related to #38

[\[c\]](#cmnt_ref3)this one was mistakenly un-numbered, so i added it as #39

[\[d\]](#cmnt_ref4)Are we talking resolving them or actually resolve-and-instantiate?

[\[e\]](#cmnt_ref5)The following rehashes some of the previous use cases for the same users or names
