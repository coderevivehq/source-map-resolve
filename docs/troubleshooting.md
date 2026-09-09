---
title: Troubleshooting
description: Resolve common source-map-resolve results and errors.
navOrder: 6
---

# Troubleshooting

## `resolve` returns `null`

`resolve` and `resolveSourceMap` return `null` when the generated code has no recognised `sourceMappingURL` comment. Confirm that you passed the generated JavaScript contents as a string and that the file contains a source-map comment.

If the source map was advertised through an HTTP `SourceMap` header instead, this package cannot discover it from the generated code. Read the header while fetching the generated file, then call `resolve(null, sourceMapUrl, read, callback)`.

## Reading the source map fails

Map read and parse errors are returned through the asynchronous callback or thrown by synchronous methods. They include an `error.sourceMapData` property containing the partial resolver result, such as the source-map URL and the unparsed map data.

Check that the generated file URL or path is correct, that relative map paths resolve from that location, and that the `read` function can read the decoded URL it receives.

## An embedded data URI fails to parse

Embedded maps must use an `application/json` or `text/json` media type. Invalid JSON, invalid URI encoding, invalid UTF-8 in a base64 map, or another media type causes a resolver error with `sourceMapData` attached.

## One original source cannot be read

`resolveSources` and `resolve` do not fail the whole operation when one original source read fails. Instead, the matching `sourcesContent` entry is an `Error` object. Check each entry before treating it as source text:

```js
result.sourcesContent.forEach(function(content, index) {
  if (content instanceof Error) {
    console.error(result.sourcesResolved[index], content.message)
    return
  }

  console.log(content)
})
```

## `sourceRoot` produces unexpected paths

By default, `resolveSources` uses a string `map.sourceRoot` as the base for `map.sources`. Pass `{sourceRoot: false}` to ignore it, or pass another string to replace it. See the [sourceRoot examples](examples.md#ignore-a-maps-sourceroot).
