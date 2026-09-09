---
title: Documentation overview
description: Resolve source maps and original source paths from generated JavaScript files.
navOrder: 1
---

# source-map-resolve documentation

`@coderevivehq/source-map-resolve` is a Node.js package for locating a source map referenced by generated code, resolving the map's source paths, and loading original source content when needed.

## How resolution works

1. Read the generated JavaScript file yourself and pass its contents and URL or path to `resolve` or `resolveSourceMap`.
2. The package finds a `sourceMappingURL` comment. It parses embedded JSON data URIs directly or calls your `read` function for a referenced map file.
3. The package resolves each item in `map.sources`, using `map.sourceRoot` unless you override it.
4. `resolve` returns the parsed map together with `sourcesResolved` and `sourcesContent`.

The package does not retrieve generated JavaScript itself. It also does not inspect HTTP response headers; if a server provides a `SourceMap` header, pass that header value as the map URL to `resolve(null, mapUrl, read, callback)` or `resolveSync(null, mapUrl, read)`.

## Start here

1. [Install the package](installation.md).
2. Follow the [Usage guide](usage.md) for an asynchronous Node.js example.
3. Use the [API reference](api.md) when choosing a resolver method.

## Guides

- [Installation](installation.md)
- [Usage](usage.md)
- [API reference](api.md)
- [Examples](examples.md)
- [Troubleshooting](troubleshooting.md)
