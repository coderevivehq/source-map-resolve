---
title: Examples
description: Resolve source maps, source paths, and source-root variations in common Node.js scenarios.
navOrder: 5
---

# Examples

## Resolve only source paths

Use the synchronous resolver with `read` set to `null` when you only need the fully resolved source paths and do not need source contents:

```js
var sourceMapResolve = require("@coderevivehq/source-map-resolve")

var map = {
  sourceRoot: "/assets/src",
  sources: ["main.ts", "shared/math.ts"]
}

var result = sourceMapResolve.resolveSourcesSync(
  map,
  "https://example.com/dist/app.js.map",
  null
)

console.log(result.sourcesResolved)
// [
//   "https://example.com/assets/src/main.ts",
//   "https://example.com/assets/src/shared/math.ts"
// ]
```

## Ignore a map's `sourceRoot`

Pass `sourceRoot: false` when the source map's `sourceRoot` should not take part in URL resolution:

```js
var result = sourceMapResolve.resolveSourcesSync(
  {
    sourceRoot: "/assets/src",
    sources: ["main.ts"]
  },
  "https://example.com/dist/app.js.map",
  null,
  {sourceRoot: false}
)

console.log(result.sourcesResolved)
// ["https://example.com/dist/main.ts"]
```

## Read a known source-map URL

When your HTTP client has already obtained a `SourceMap` response header, pass its value as the map URL rather than searching generated code for a comment:

```js
sourceMapResolve.resolve(null, sourceMapUrl, read, function(error, result) {
  if (error) {
    throw error
  }

  console.log(result.sourcesResolved)
})
```

## Resolve an embedded data URI map

`resolveSourceMap` parses `application/json` and `text/json` data URIs without calling `read`:

```js
var code = "//# sourceMappingURL=data:application/json,%7B%22version%22%3A3%2C%22sources%22%3A%5B%5D%7D"

sourceMapResolve.resolveSourceMap(code, "app.js", function unusedRead() {
  throw new Error("read is not called for a valid embedded source map")
}, function(error, result) {
  if (error) {
    throw error
  }

  console.log(result.url)
  // null
})
```
