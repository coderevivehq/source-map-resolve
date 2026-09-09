---
title: Installation
description: Install source-map-resolve in a supported Node.js project.
navOrder: 2
---

# Installation

## Requirements

- Node.js 14.16 or later.

## Install

```sh
npm install @coderevivehq/source-map-resolve
```

## Verify the installation

```sh
node -e 'console.log(typeof require("@coderevivehq/source-map-resolve").resolve)'
```

The command prints `function` when Node.js can load the package.

## Next steps

Continue to the [Usage guide](usage.md).
