---
title: API reference
description: Reference for the supported source-map-resolve public API.
navOrder: 4
---

# API reference

```js
var sourceMapResolve = require("@coderevivehq/source-map-resolve")
```

## Shared inputs

`code` is generated JavaScript as a string. `codeUrl` and `mapUrl` are file paths or URLs used as the base for relative map and source paths. The package passes a URI-decoded URL to each `read` function.

Asynchronous `read` functions use this signature:

```js
function read(url, callback) {
  callback(error, contents)
}
```

Synchronous `read` functions receive `url` and return contents or throw an error.

## `resolveSourceMap(code, codeUrl, read, callback)`

Finds the `sourceMappingURL` comment in generated code and resolves its source map.

`callback(error, result)` receives `null` when no source-map comment exists. Otherwise, `result` has this shape:

| Property | Description |
| --- | --- |
| `sourceMappingURL` | The exact value found in the generated code. |
| `url` | The resolved map URL, or `null` for an embedded data URI. |
| `sourcesRelativeTo` | The URL or path used as the base when resolving `map.sources`. |
| `map` | The parsed source map object. |

`resolveSourceMapSync(code, codeUrl, read)` is the synchronous equivalent. It returns the same result or `null`, and throws when it cannot resolve or parse the map.

## `resolveSources(map, mapUrl, read, [options], callback)`

Resolves the entries in `map.sources` and loads their contents. `callback(error, result)` receives:

| Property | Description |
| --- | --- |
| `sourcesResolved` | Fully resolved source URLs or paths, in the same order as `map.sources`. |
| `sourcesContent` | Embedded source strings, loaded source strings, or `Error` objects for individual source-read failures. |

If `map.sources` is missing or empty, both arrays are empty. A failure to read one original source is stored at the corresponding `sourcesContent` index; it does not make the outer callback fail.

`resolveSourcesSync(map, mapUrl, read, [options])` returns the same result. Pass `null` as `read` to resolve only source URLs without loading source contents.

### `sourceRoot` option

The optional `sourceRoot` setting controls the base path used for every entry in `map.sources`:

| Value | Behavior |
| --- | --- |
| Omitted | Uses `map.sourceRoot` when it is a string. |
| A string | Replaces `map.sourceRoot`. |
| `false` | Ignores `map.sourceRoot`. |

An empty `map.sourceRoot` is treated as if it were not set.

## `resolve(code, codeUrl, read, [options], callback)`

Combines `resolveSourceMap` and `resolveSources`. It returns `null` through the callback when generated code has no `sourceMappingURL` comment. Otherwise, the result includes the source-map properties and the two source-result arrays:

```js
{
  sourceMappingURL: "app.js.map",
  url: "/absolute/or/resolved/path/app.js.map",
  sourcesRelativeTo: "/absolute/or/resolved/path/app.js.map",
  map: {},
  sourcesResolved: [],
  sourcesContent: []
}
```

To read a known map URL directly, pass `null` for `code`. In that form, `codeUrl` is treated as the map URL:

```js
sourceMapResolve.resolve(null, mapUrl, read, callback)
```

`resolveSync(code, codeUrl, read, [options])` is the synchronous equivalent. It returns the same result or `null`, and throws for map-read or map-parse errors.

## `parseMapToJSON(string, [data])`

Parses source-map JSON. It removes an optional leading `)]}'` XSSI prefix before calling `JSON.parse`.

If parsing fails, the thrown error receives a `sourceMapData` property set to the optional `data` argument. Map-reading and map-parsing errors from the resolver methods also receive `sourceMapData`, containing the partial result available at the point of failure.
