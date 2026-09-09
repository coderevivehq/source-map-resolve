<p align="center">
  <img src="https://raw.githubusercontent.com/coderevivehq/source-map-resolve/main/.github/assets/coderevive-hero.png" alt="source-map-resolve maintained by CodeRevive" width="460">
</p>

<h1 align="center">source-map-resolve</h1>

<p align="center">
  A maintained continuation of <a href="https://github.com/lydell/source-map-resolve">source-map-resolve</a> by <a href="https://github.com/coderevivehq">CodeRevive</a>.
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@coderevivehq/source-map-resolve"><img alt="npm version" src="https://img.shields.io/npm/v/%40coderevivehq%2Fsource-map-resolve?style=flat-square"></a>
  <a href="https://www.npmjs.com/package/@coderevivehq/source-map-resolve"><img alt="npm downloads" src="https://img.shields.io/npm/dm/%40coderevivehq%2Fsource-map-resolve?style=flat-square"></a>
  <a href="https://github.com/coderevivehq/source-map-resolve/actions/workflows/ci.yml"><img alt="build status" src="https://github.com/coderevivehq/source-map-resolve/actions/workflows/ci.yml/badge.svg"></a>
  <a href="https://github.com/coderevivehq/source-map-resolve/releases"><img alt="latest release" src="https://img.shields.io/github/v/release/coderevivehq/source-map-resolve?style=flat-square"></a>
  <a href="LICENSE"><img alt="MIT license" src="https://img.shields.io/github/license/coderevivehq/source-map-resolve?style=flat-square"></a>
</p>

## Overview

Resolve the source map and/or sources for a generated file. The module is intended for Node.js tools and browser applications that need to locate a source map from a generated file, resolve the source paths in that map, and optionally load the original source contents.

## Maintained by CodeRevive

This maintained continuation is published by [CodeRevive](https://github.com/coderevivehq). Security patches are our highest priority. We also review bug reports, feature requests, and suggestions from the community.

The project is based on the original [source-map-resolve](https://github.com/lydell/source-map-resolve) repository and its contributors.

## Quick links

- [Overview](#overview)
- [Maintained by CodeRevive](#maintained-by-coderevive)
- [Installation & setup](#installation--setup)
- [Documentation](#documentation)
- [Usage](#usage)
- [Contributing](#contributing)
- [Security & support](#security--support)
- [Credits & license](#credits--license)

## Installation & setup

```sh
npm install @coderevivehq/source-map-resolve
```

The package supports Node.js 14.16 and later. It can also be used in browser environments with a compatible asynchronous or synchronous `read` function.

## Documentation

The API documentation is maintained in this README. See [Usage](#usage) for the resolver methods and their result objects, and [Notes](#notes) for source-map header handling.

## Usage

```js
var sourceMapResolve = require("@coderevivehq/source-map-resolve")

sourceMapResolve.resolve(code, codeUrl, read, function(error, result) {
  if (error) {
    return notifyFailure(error)
  }

  // result.map contains the parsed source map.
  // result.sourcesResolved contains fully resolved source URLs.
  // result.sourcesContent contains the loaded source contents.
})
```

### `sourceMapResolve.resolveSourceMap(code, codeUrl, read, callback)`

Finds a `sourceMappingURL` comment in `code` and reads the referenced source map.

- `code` is generated code that may contain a source-map comment.
- `codeUrl` is the URL of the generated file. Relative source-map URLs are resolved against it.
- `read(url, callback)` reads a URL and calls `callback(error, content)`.
- `callback(error, result)` receives the parsed map, its URL, the URL used to resolve sources, and the original `sourceMappingURL`.

If `code` contains no source-map comment, the result is `null`.

### `sourceMapResolve.resolveSources(map, mapUrl, read, [options], callback)`

Resolves every source in a parsed source map and reads its contents. The result contains `sourcesResolved` and `sourcesContent` in the same order as `map.sources`. The optional `sourceRoot` option overrides or ignores the map's `sourceRoot` value.

### `sourceMapResolve.resolve(code, codeUrl, read, [options], callback)`

A convenience method that resolves a source map and then its sources. If `code` is `null`, `codeUrl` is treated as the source-map URL and read directly.

### Synchronous methods and parsing

`resolveSourceMapSync`, `resolveSourcesSync`, and `resolveSync` provide synchronous equivalents that return results or throw errors. `parseMapToJSON(string, [data])` strips the optional `)]}'` XSSI prefix before parsing a source map as JSON.

Errors include a `sourceMapData` property containing the partial result available when the error occurred.

## Notes

Source maps can also be supplied through a `SourceMap: <url>` response header. This module does not retrieve generated code, so callers that need this behavior must read the header while retrieving the generated file and then call `resolve(null, sourceMapUrl, read, ...)`.

## Contributing

Bug reports, focused improvements, and documentation updates are welcome through [GitHub issues](https://github.com/coderevivehq/source-map-resolve/issues) and [pull requests](https://github.com/coderevivehq/source-map-resolve/pulls). Please run `npm test` before submitting a change.

## Security & support

Report security concerns privately through the repository's [Security](https://github.com/coderevivehq/source-map-resolve/security) page. For usage questions and ordinary bugs, open a [GitHub issue](https://github.com/coderevivehq/source-map-resolve/issues) with a minimal reproduction when possible.

## Credits & license

This project is a maintained continuation of [lydell/source-map-resolve](https://github.com/lydell/source-map-resolve). Original code and CodeRevive-authored changes are licensed under the MIT License. Original copyright notices are retained in [LICENSE](LICENSE).
