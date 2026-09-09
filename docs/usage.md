---
title: Usage
description: Resolve a generated file's source map and original source contents in Node.js.
navOrder: 3
---

# Usage

The asynchronous `resolve` method handles the normal case: generated code contains a `sourceMappingURL` comment, the referenced map lists original sources, and you want their contents.

## Resolve a generated file

```js
var fs = require("fs")
var path = require("path")
var sourceMapResolve = require("@coderevivehq/source-map-resolve")

var generatedFile = path.resolve("dist/app.js")

function read(url, callback) {
  fs.readFile(url, "utf8", callback)
}

fs.readFile(generatedFile, "utf8", function(error, code) {
  if (error) {
    throw error
  }

  sourceMapResolve.resolve(code, generatedFile, read, function(error, result) {
    if (error) {
      throw error
    }

    if (result === null) {
      console.log("No sourceMappingURL comment was found.")
      return
    }

    console.log(result.map)
    console.log(result.sourcesResolved)
    console.log(result.sourcesContent)
  })
})
```

`result.map` is the parsed source map. `result.sourcesResolved` contains fully resolved source paths in the same order as `map.sources`. `result.sourcesContent` contains the corresponding embedded source string, loaded source string, or an `Error` when an individual source could not be read.

## Synchronous use

Use `resolveSync` only when synchronous file reads are appropriate for your application:

```js
var fs = require("fs")
var path = require("path")
var sourceMapResolve = require("@coderevivehq/source-map-resolve")

var generatedFile = path.resolve("dist/app.js")
var code = fs.readFileSync(generatedFile, "utf8")

var result = sourceMapResolve.resolveSync(code, generatedFile, function read(url) {
  return fs.readFileSync(url, "utf8")
})

if (result !== null) {
  console.log(result.sourcesResolved)
}
```

## Next steps

- See the [API reference](api.md) for every resolver method and its result shape.
- See [Examples](examples.md) for resolving only paths, changing `sourceRoot`, and reading a map URL directly.
