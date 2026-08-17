---
type: Web Page
title: Glob | Bun Docs
description: Use Bun's fast native implementation of file globbing
resource: https://bun.sh/docs/runtime/glob
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Glob

Use Bun's fast native implementation of file globbing

## Quickstart

**Scan a directory for files matching `*.ts`**:

```
import { Glob } from "bun";
const glob = new Glob("**/*.ts");
// Scans the current working directory and each of its sub-directories recursively
for await (const file of glob.scan(".")) {
  console.log(file); // => "index.ts"
}
```
**Match a string against a glob pattern**:

```
import { Glob } from "bun";
const glob = new Glob("*.ts");
glob.match("index.ts"); // => true
glob.match("index.js"); // => false
```
The `Glob` class implements the following interface:

```
class Glob {
  scan(root: string | ScanOptions): AsyncIterable<string>;
  scanSync(root: string | ScanOptions): Iterable<string>;
  match(path: string): boolean;
}
interface ScanOptions {
  /**
   * The root directory to start matching from. Defaults to `process.cwd()`
   */
  cwd?: string;
  /**
   * Allow patterns to match entries that begin with a period (`.`).
   *
   * @default false
   */
  dot?: boolean;
  /**
   * Return the absolute path for entries.
   *
   * @default false
   */
  absolute?: boolean;
  /**
   * Indicates whether to traverse descendants of symbolic link directories.
   *
   * @default false
   */
  followSymlinks?: boolean;
  /**
   * Throw an error when symbolic link is broken
   *
   * @default false
   */
  throwErrorOnBrokenSymlink?: boolean;
  /**
   * Return only files.
   *
   * @default true
   */
  onlyFiles?: boolean;
}
```
## Supported Glob Patterns

Bun supports the following glob patterns:

### `?` - Match any single character

```
const glob = new Glob("???.ts");
glob.match("foo.ts"); // => true
glob.match("foobar.ts"); // => false
```
### `*` - Matches zero or more characters, except for path separators (`/` or `\`)

```
const glob = new Glob("*.ts");
glob.match("index.ts"); // => true
glob.match("src/index.ts"); // => false
```
### `**` - Match any number of characters including `/`

```
const glob = new Glob("**/*.ts");
glob.match("index.ts"); // => true
glob.match("src/index.ts"); // => true
glob.match("src/index.js"); // => false
```
### `[ab]` - Matches one of the characters contained in the brackets, as well as character ranges

```
const glob = new Glob("ba[rz].ts");
glob.match("bar.ts"); // => true
glob.match("baz.ts"); // => true
glob.match("bat.ts"); // => false
```
You can use character ranges (for example `[0-9]`, `[a-z]`). The negation operators `^` or `!` match anything *except* the characters in the brackets (for example `[^ab]`, `[!a-z]`).

```
const glob = new Glob("ba[a-z][0-9][^4-9].ts");
glob.match("bar01.ts"); // => true
glob.match("baz83.ts"); // => true
glob.match("bat22.ts"); // => true
glob.match("bat24.ts"); // => false
glob.match("ba0a8.ts"); // => false
```
### `{a,b,c}` - Match any of the given patterns

```
const glob = new Glob("{a,b,c}.ts");
glob.match("a.ts"); // => true
glob.match("b.ts"); // => true
glob.match("c.ts"); // => true
glob.match("d.ts"); // => false
```
You can nest these patterns up to 10 levels deep, and they can contain any of the earlier wildcards.

### `!` - Negates the result at the start of a pattern

```
const glob = new Glob("!index.ts");
glob.match("index.ts"); // => false
glob.match("foo.ts"); // => true
```
### `\` - Escapes any of the special characters above

```
const glob = new Glob("\\!index.ts");
glob.match("!index.ts"); // => true
glob.match("index.ts"); // => false
```
## Node.js `fs.glob()` compatibility

Bun also implements Node.js's `fs.glob()` functions:

```
import { glob, globSync, promises } from "node:fs";
// Array of patterns
const files = await Array.fromAsync(promises.glob(["**/*.ts", "**/*.js"]));
// Exclude patterns
const filtered = await Array.fromAsync(
  promises.glob("**/*", {
    exclude: ["node_modules/**", "**/*.test.*"],
  }),
);
```
All three functions (`fs.glob()`, `fs.globSync()`, `fs.promises.glob()`) support:

- Array of patterns as the first argument
- `exclude` option to filter results

# Citations

1. Source page: https://bun.sh/docs/runtime/glob
