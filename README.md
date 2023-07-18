# ESM Package Testing

## Issue

We have a package dependency with _both_ `package.json:exports` and a `"type": "module"`  declaration. The issue is -- can CJS work at all?

## Conclusion

There is no way to `require` files from a `package.json` that has `"type": "module"` declared, even if it has `package.json:exports.*.require` entries.

## Usage

### ESM

Try the ESM version first and verify it works...

```sh
$ node index.mjs
{ msg: 'ESM' }
```

### CJS

Now try the CJS version and get a failure:

```sh
$ node index.js
# Expected: { msg: 'CJS' }
/Users/rye/scm/fmd/esm-test-pkg/index.js:2
const msg = require("my-pkg");
            ^

Error [ERR_REQUIRE_ESM]: require() of ES Module /Users/rye/scm/fmd/esm-test-pkg/node_modules/my-pkg/cjs.js from /Users/rye/scm/fmd/esm-test-pkg/index.js not supported.
cjs.js is treated as an ES module file as it is a .js file whose nearest parent package.json contains "type": "module" which declares all .js files in that package scope as ES modules.
Instead rename cjs.js to end in .cjs, change the requiring code to use dynamic import() which is available in all CommonJS modules, or change "type": "module" to "type": "commonjs" in /Users/rye/scm/fmd/esm-test-pkg/node_modules/my-pkg/package.json to treat all .js files as CommonJS (using .mjs for all ES modules instead).

    at Object.<anonymous> (/Users/rye/scm/fmd/esm-test-pkg/index.js:2:13) {
  code: 'ERR_REQUIRE_ESM'
}

Node.js v18.7.0
```
