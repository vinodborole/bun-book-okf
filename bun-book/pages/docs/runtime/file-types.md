---
type: Web Page
title: File Types - Bun
description: File types and loaders supported by Bun's bundler and runtime
resource: https://bun.sh/docs/runtime/file-types
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`.js` `.cjs` `.mjs` `.mts` `.cts` `.ts` `.tsx` `.jsx` `.css` `.json` `.jsonc` `.json5` `.toml` `.yaml` `.yml` `.txt` `.wasm` `.node` `.html` `.sh`
Bun uses the file extension to pick the built-in *loader*that parses the file. Every loader has a name, such as

`js`, `tsx`, or `json`. These names are used when building plugins that extend Bun with custom loaders.
To specify a loader explicitly, use the `type` import attribute.
## Built-in loaders

`js`

**JavaScript**. Default for

`.cjs` and `.mjs`.
Parses the code and applies a set of default transforms like dead-code elimination and tree shaking. Bun does not down-convert syntax.
`jsx`

**JavaScript + JSX**. Default for

`.js` and `.jsx`.
Same as the `js` loader, but JSX syntax is supported. By default, JSX is down-converted to plain JavaScript; the details depend on the `jsx*` compiler options in your `tsconfig.json`. Refer to the TypeScript documentation on JSX.
`ts`

**TypeScript loader**. Default for

`.ts`, `.mts`, and `.cts`.
Strips out all TypeScript syntax, then behaves identically to the `js` loader. Bun does not perform typechecking.
`tsx`

**TypeScript + JSX loader**. Default for

`.tsx`. Transpiles both TypeScript and JSX to vanilla JavaScript.
`json`

**JSON loader**. Default for

`.json`.
JSON files can be directly imported.
`.json` file is passed as an entrypoint to the bundler, it is converted to a `.js` module that `export default`s the parsed object.
`jsonc`

**JSON with Comments loader**. Default for

`.jsonc`.
JSONC (JSON with Comments) files can be directly imported. Bun parses them, stripping out comments and trailing commas.
`json` loader.
Bun automatically uses the 

`jsonc` loader for `tsconfig.json`, `jsconfig.json`, `package.json`, and `bun.lock` files.`toml`

**TOML loader**. Default for

`.toml`.
TOML files can be directly imported. Bun parses them with its fast native TOML parser.
`.toml` file is passed as an entrypoint, it is converted to a `.js` module that `export default`s the parsed object.
`yaml`

**YAML loader**. Default for

`.yaml` and `.yml`.
YAML files can be directly imported. Bun parses them with its fast native YAML parser.
`.yaml` or `.yml` file is passed as an entrypoint, it is converted to a `.js` module that `export default`s the parsed object.
`json5`

**JSON5 loader**. Default for

`.json5`.
JSON5 files can be directly imported. Bun parses them with its fast native JSON5 parser. JSON5 is a superset of JSON that adds comments, trailing commas, unquoted keys, single-quoted strings, and more.
`.json5` file is passed as an entrypoint, it is converted to a `.js` module that `export default`s the parsed object.
`text`

**Text loader**. Default for

`.txt`.
Text files can be directly imported. The file is read and returned as a string.
`.txt` file is passed as an entrypoint, it is converted to a `.js` module that `export default`s the file contents.
`napi`

**Native addon loader**. Default for

`.node`.
In the runtime, native addons can be directly imported.
`.node` files are handled using the `file` loader.
`sqlite`

**SQLite loader**.

`with { "type": "sqlite" }` import attribute
In the runtime and bundler, SQLite databases can be directly imported. The database is loaded with `bun:sqlite`.
`target` is `bun`.
By default, the database is external to the bundle: the on-disk database file isn’t bundled into the final output, so you can use a database loaded elsewhere.
You can change this behavior with the `"embed"` attribute:
`outdir` with a hashed filename.
`html`

The `html` loader processes HTML files and bundles any referenced assets. It:
- Bundles and hashes referenced JavaScript files (`<script src="...">`)
- Bundles and hashes referenced CSS files (`<link rel="stylesheet" href="...">`)
- Hashes referenced images (`<img src="...">`)
- Preserves external URLs (by default, anything starting with `http://`or`https://`)

`lol-html` to extract script and link tags as entrypoints, and other assets as external.
The list of selectors is:
- `audio[src]`
- `iframe[src]`
- `img[src]`
- `img[srcset]`
- `link:not([rel~='stylesheet']):not([rel~='modulepreload']):not([rel~='manifest']):not([rel~='icon']):not([rel~='apple-touch-icon'])[href]`
- `link[as='font'][href], link[type^='font/'][href]`
- `link[as='image'][href]`
- `link[as='style'][href]`
- `link[as='video'][href], link[as='audio'][href]`
- `link[as='worker'][href]`
- `link[rel='icon'][href], link[rel='apple-touch-icon'][href]`
- `link[rel='manifest'][href]`
- `link[rel='stylesheet'][href]`
- `script[src]`
- `source[src]`
- `source[srcset]`
- `video[poster]`
- `video[src]`

**HTML Loader Behavior in Different Contexts**The

`html` loader behaves differently depending on how it’s used:- 
**Static Build:**When you run`bun build ./index.html`, Bun produces a static site with all assets bundled and hashed.
- 
**Runtime:**When you run`bun run server.ts`(where`server.ts`imports an HTML file), Bun bundles assets on-the-fly during development, enabling features like hot module replacement.
- 
**Full-stack Build:**When you run`bun build --target=bun server.ts`(where`server.ts`imports an HTML file), the import resolves to a manifest object that`Bun.serve`uses to efficiently serve pre-bundled assets in production.

`css`

**CSS loader**. Default for

`.css`.
CSS files can be directly imported. This is primarily useful for full-stack applications where CSS is bundled alongside HTML.
`sh` loader

**Bun Shell loader**. Default for

`.sh` files
This loader parses Bun Shell scripts. It’s only supported when starting Bun itself, so it’s not available in the bundler or in the runtime.
`file`

**File loader**. Default for all unrecognized file types. The file loader resolves the import as a

*path/URL*to the imported file. It’s commonly used for referencing media or font assets.

logo.ts

*In the runtime*, Bun checks that the

`logo.svg` file exists and resolves the import to its absolute path on disk.
*In the bundler*, the file is copied into

`outdir` as-is, and the import resolves to a relative path pointing to the copied file.
Output

`publicPath` is set, the import uses its value as a prefix to construct an absolute path/URL.
| Public path | Resolved import | 
|---|---|
| `""`(default) | `/logo.svg` | 
| `"/assets"` | `/assets/logo.svg` | 
| `"https://cdn.example.com/"` | `https://cdn.example.com/logo.svg` | 

The location and file name of the copied file is determined by the value of 

`naming.asset`.Fixing TypeScript import errors

Fixing TypeScript import errors

If you’re using TypeScript, you may get an error like this:To fix this, create a This tells TypeScript that any default imports from 

`*.d.ts` file anywhere in your project (any name works) with the following contents:`.svg` should be treated as a string.

# Citations

1. Source page: https://bun.sh/docs/runtime/file-types
