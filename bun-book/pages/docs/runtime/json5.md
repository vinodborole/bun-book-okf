---
type: Web Page
title: JSON5 - Bun
description: Use Bun's built-in support for JSON5 files through both runtime APIs
  and bundler integration
resource: https://bun.sh/docs/runtime/json5
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

- Parse and stringify JSON5 with `Bun.JSON5.parse`and`Bun.JSON5.stringify`
- `import`&- `require`JSON5 files as modules at runtime (including hot reloading & watch mode support)
- `import`&- `require`JSON5 files in frontend apps with Bun’s bundler

## Conformance

Bun’s JSON5 parser is written in Rust and passes 100% of the[official JSON5 test suite](https://github.com/json5/json5-tests). The

[translated test suite](https://github.com/oven-sh/bun/blob/main/test/js/bun/json5/json5-test-suite.test.ts)lists every test case.

## Runtime API

`Bun.JSON5.parse()`

Parse a JSON5 string into a JavaScript value.
#### Supported JSON5 Features

JSON5 is a superset of JSON based on ECMAScript 5.1 syntax. It supports:- **Comments**: single-line (- `//`) and multi-line (- `/* */`)
- **Trailing commas**: in objects and arrays
- **Unquoted keys**: any valid ECMAScript 5.1 identifier
- **Single-quoted strings**: in addition to double-quoted strings
- **Multi-line strings**: using backslash line continuations
- **Hex numbers**:- `0xFF`
- **Leading & trailing decimal points**:- `.5`and- `5.`
- **Infinity and NaN**: positive and negative
- **Explicit plus sign**:- `+42`

#### Error Handling

`Bun.JSON5.parse()` throws a `SyntaxError` if the input is invalid JSON5:
`Bun.JSON5.stringify()`

Stringify a JavaScript value to a JSON5 string.
#### Pretty Printing

Pass a`space` argument to format the output with indentation:
`space` argument can be a number (number of spaces) or a string (used as the indent character):
#### Special Values

Unlike`JSON.stringify`, `JSON5.stringify` preserves special numeric values:
## Module Import

### ES Modules

You can import JSON5 files directly as ES modules:config.json5

#### Default Import

app.ts

#### Named Imports

You can destructure top-level properties as named imports:app.ts

### CommonJS

You can also`require` JSON5 files in CommonJS:
app.ts

## Hot Reloading with JSON5

When you run your application with`bun --hot`, Bun reloads JSON5 files when they change:
config.json5

server.ts

terminal

## Bundler Integration

When you bundle with Bun, imported JSON5 files are parsed at build time and included as JavaScript modules:terminal

- Zero runtime JSON5 parsing overhead in production
- Smaller bundle sizes
- Tree shaking of unused properties (named imports)

# Citations

1. Source page: https://bun.sh/docs/runtime/json5
