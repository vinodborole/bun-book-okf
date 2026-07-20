---
type: Web Page
title: TOML - Bun
description: Use Bun's built-in support for TOML files through both runtime APIs and
  bundler integration
resource: https://bun.sh/docs/runtime/toml
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

- Parse TOML strings with `Bun.TOML.parse`
- `import`&- `require`TOML files as modules at runtime (including hot reloading & watch mode support)
- `import`&- `require`TOML files in frontend apps with Bun’s bundler

## Runtime API

`Bun.TOML.parse()`

Parse a TOML string into a JavaScript object.
#### Supported TOML Features

Bun’s TOML parser implements the full[TOML v1.1.0 specification](https://github.com/toml-lang/toml/releases/tag/1.1.0)and passes the complete official

[toml-test](https://github.com/toml-lang/toml-test)conformance suite.

- **Strings**: basic (- `"..."`) and literal (- `'...'`), including multi-line, with all escapes (- `\uHHHH`,- `\UHHHHHHHH`, and TOML 1.1’s- `\xHH`and- `\e`)
- **Integers**: decimal, hex (- `0x`), octal (- `0o`), and binary (- `0b`). Integers that cannot be represented losslessly as a JavaScript number — outside ±(2^53 - 1) — throw
- **Floats**: including- `inf`and- `nan`
- **Booleans**:- `true`and- `false`
- **Date/times**: offset date-time, local date-time, local date, and local time, returned as strings of their source text
- **Arrays**: including mixed types and nested arrays
- **Tables**: standard (- `[table]`) and inline (- `{ key = "value" }`), including TOML 1.1 multi-line inline tables
- **Array of tables**:- `[[array]]`
- **Dotted keys**:- `a.b.c = "value"`
- **Comments**: using- `#`

#### Error Handling

`Bun.TOML.parse()` throws a `SyntaxError` if the TOML is invalid:
`Bun.TOML.stringify()`

Serialize a JavaScript object to a TOML document. Scalar keys come first,
followed by `[table]` and `[[array-of-tables]]` sections:
`Date`
values become TOML offset date-times. Because TOML cannot represent them,
`null` values, `BigInt`, and circular structures throw; `undefined`,
function, and symbol properties are skipped (inside arrays they throw,
since TOML arrays cannot have holes).
## Module Import

### ES Modules

Import TOML files directly as ES modules. Bun parses the TOML and exposes it as both default and named exports:config.toml

#### Default Import

app.ts

#### Named Imports

You can destructure top-level TOML tables as named imports:app.ts

app.ts

#### Import Attributes

Use an import attribute to load any file as TOML:app.ts

### CommonJS

You can also`require` TOML files in CommonJS:
app.ts

## Hot Reloading with TOML

When you run your application with`bun --hot`, Bun detects changes to TOML files and reloads them without restarting:
config.toml

server.ts

terminal

## Bundler Integration

When you bundle with Bun, the bundler parses imported TOML at build time and includes it as a JavaScript module:terminal

- Zero runtime TOML parsing overhead in production
- Smaller bundle sizes
- Tree shaking of unused properties (named imports)

# Citations

1. Source page: https://bun.sh/docs/runtime/toml
