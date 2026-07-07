---
type: Web Page
title: Glob - Bun
description: Use Bun's fast native implementation of file globbing
resource: https://bun.sh/docs/runtime/glob
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Quickstart

**Scan a directory for files matching**:

`*.ts`**Match a string against a glob pattern**:

`Glob` class implements the following interface:
## Supported Glob Patterns

Bun supports the following glob patterns:`?` - Match any single character

`*` - Matches zero or more characters, except for path separators (`/` or `\`)

`**` - Match any number of characters including `/`

`[ab]` - Matches one of the characters contained in the brackets, as well as character ranges

`[0-9]`, `[a-z]`) and the negation operators `^` or `!` to match anything *except*the characters in the brackets (for example

`[^ab]`, `[!a-z]`).
`{a,b,c}` - Match any of the given patterns

`!` - Negates the result at the start of a pattern

`\` - Escapes any of the special characters above

## Node.js `fs.glob()` compatibility

Bun also implements Node.js’s `fs.glob()` functions with additional features:
`fs.glob()`, `fs.globSync()`, `fs.promises.glob()`) support:
- Array of patterns as the first argument
- `exclude`option to filter results

# Citations

1. Source page: https://bun.sh/docs/runtime/glob
