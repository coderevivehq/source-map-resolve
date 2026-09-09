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

Resolve the source map and/or sources for a generated file. The package targets Node.js tools that need to locate a source map from generated code, resolve the source paths in that map, and optionally load the original source contents.

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

The package supports Node.js 14.16 and later.

## Documentation

Detailed documentation is maintained with the source code in the [`docs/`](docs/) directory. Start with the [documentation overview](docs/index.md), then use the [Usage guide](docs/usage.md), [API reference](docs/api.md), [Examples](docs/examples.md), and [Troubleshooting guide](docs/troubleshooting.md).

## Usage

```js
var sourceMapResolve = require("@coderevivehq/source-map-resolve")

sourceMapResolve.resolve(code, codeUrl, read, function(error, result) {
  if (error) {
    throw error
  }

  // result.map contains the parsed source map.
  // result.sourcesResolved contains fully resolved source URLs.
  // result.sourcesContent contains the loaded source contents.
})
```

For complete asynchronous and synchronous examples, result shapes, `sourceRoot` handling, data URIs, and error behavior, see the [documentation overview](docs/index.md).

## Contributing

Bug reports, focused improvements, and documentation updates are welcome through [GitHub issues](https://github.com/coderevivehq/source-map-resolve/issues) and [pull requests](https://github.com/coderevivehq/source-map-resolve/pulls). Please run `npm test` before submitting a change.

## Security & support

Report security concerns privately through the repository's [Security](https://github.com/coderevivehq/source-map-resolve/security) page. For usage questions and ordinary bugs, open a [GitHub issue](https://github.com/coderevivehq/source-map-resolve/issues) with a minimal reproduction when possible.

## Credits & license

This project is a maintained continuation of [lydell/source-map-resolve](https://github.com/lydell/source-map-resolve). Original code and CodeRevive-authored changes are licensed under the MIT License. Original copyright notices are retained in [LICENSE](LICENSE).
