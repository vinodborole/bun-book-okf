---
type: Web Page
title: YAML - Bun
description: Use Bun's built-in support for YAML files through both runtime APIs and
  bundler integration
resource: https://bun.sh/docs/runtime/yaml
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

- Parse YAML strings with `Bun.YAML.parse`
- `import`&- `require`YAML files as modules at runtime (including hot reloading & watch mode support)
- `import`&- `require`YAML files in frontend apps with Bun’s bundler

## Conformance

Bun’s YAML parser, written in Rust, passes over 90% of the official YAML test suite and covers the vast majority of real-world use cases. We’re working toward 100% conformance.## Runtime API

`Bun.YAML.parse()`

Parse a YAML string into a JavaScript object.
#### Multi-document YAML

When parsing YAML with multiple documents (separated by`---`), `Bun.YAML.parse()` returns an array:
#### Supported YAML Features

Bun’s YAML parser supports the full YAML 1.2 specification, including:- **Scalars**: strings, numbers, booleans, null values
- **Collections**: sequences (arrays) and mappings (objects)
- **Anchors and Aliases**: reusable nodes with- `&`and- `*`
- **Tags**: type hints like- `!!str`,- `!!int`,- `!!float`,- `!!bool`,- `!!null`
- **Multi-line strings**: literal (- `|`) and folded (- `>`) scalars
- **Comments**: using- `#`
- **Directives**:- `%YAML`and- `%TAG`

#### Error Handling

`Bun.YAML.parse()` throws a `SyntaxError` if the YAML is invalid:
## Module Import

### ES Modules

Import YAML files directly as ES modules. Bun parses the content and exposes it as both default and named exports:config.yaml

#### Default Import

app.ts

#### Named Imports

Top-level YAML properties are available as named imports:app.ts

app.ts

### CommonJS

You can also`require` YAML files in CommonJS:
app.ts

## Hot Reloading with YAML

When you run your application with`bun --hot`, Bun detects changes to YAML files and reloads them without closing connections.
### Configuration Hot Reloading

config.yaml

server.ts

terminal

`config.yaml`, the running application picks up the change: you can adjust settings or toggle feature flags during development without restarting.
## Configuration Management

### Environment-Based Configuration

One YAML file can hold configuration for several environments, sharing defaults with anchors:config.yaml

app.ts

### Feature Flags Configuration

features.yaml

feature-flags.ts

### Database Configuration

database.yaml

db.ts

### Bundler Integration

When you bundle an application that imports YAML files, Bun parses the YAML at build time and includes it as a JavaScript module:terminal

- No runtime YAML parsing overhead in production
- Smaller bundle sizes
- Tree shaking of unused configuration (named imports)

### Dynamic Imports

Dynamically import YAML files to load configuration on demand:Load configuration based on environment

# Citations

1. Source page: https://bun.sh/docs/runtime/yaml
