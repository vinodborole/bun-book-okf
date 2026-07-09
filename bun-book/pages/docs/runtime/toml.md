---
type: Web Page
title: TOML - Bun
description: Use Bun's built-in support for TOML files through both runtime APIs and
  bundler integration
resource: https://bun.sh/docs/runtime/toml
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

- Parse TOML strings with `Bun.TOML.parse`
- `import`&- `require`TOML files as modules at runtime (including hot reloading & watch mode support)
- `import`&- `require`TOML files in frontend apps with Bun’s bundler

## Runtime API

`Bun.TOML.parse()`

Parse a TOML string into a JavaScript object.
#### Supported TOML Features

Bun’s TOML parser supports the[TOML v1.0 specification](https://toml.io/en/v1.0.0), including:

- **Strings**: basic (- `"..."`) and literal (- `'...'`), including multi-line
- **Integers**: decimal, hex (- `0x`), octal (- `0o`), and binary (- `0b`)
- **Floats**
- **Booleans**:- `true`and- `false`
- **Arrays**: including mixed types and nested arrays
- **Tables**: standard (- `[table]`) and inline (- `{ key = "value" }`)
- **Array of tables**:- `[[array]]`
- **Dotted keys**:- `a.b.c = "value"`
- **Comments**: using- `#`

#### Error Handling

`Bun.TOML.parse()` throws if the TOML is invalid:
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
