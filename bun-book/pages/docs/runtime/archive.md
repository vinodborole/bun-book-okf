---
type: Web Page
title: Archive - Bun
description: Create and extract tar archives with Bun's fast native implementation
resource: https://bun.sh/docs/runtime/archive
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`Bun.Archive` is Bun’s native API for tar archives. It creates archives from in-memory data, extracts archives to disk, and reads archive contents without extraction.
## Quickstart

**Create an archive from files:**

**Extract an archive:**

**Read archive contents without extracting:**

## Creating Archives

Use`new Bun.Archive()` to create an archive from an object where keys are file paths and values are file contents. By default, archives are uncompressed:
- **Strings**- Text content
- **Blobs**- Binary data
- **ArrayBufferViews**(such as- `Uint8Array`) - Raw bytes
- **ArrayBuffers**- Raw binary data

### Writing Archives to Disk

Use`Bun.write()` to write an archive to disk:
### Getting Archive Bytes

Get the archive data as bytes or a Blob:## Extracting Archives

### From Existing Archive Data

Create an archive from existing tar/tar.gz data:### Extracting to Disk

Use`.extract()` to write all files to a directory:
`extract()` creates the target directory if it doesn’t exist and overwrites existing files. The returned count includes files, directories, and symlinks (on POSIX systems).
**Note**: On Windows, Bun always skips symbolic links during extraction, regardless of privilege level. On Linux and macOS, symlinks are extracted normally.

**Security note**: Bun.Archive validates paths during extraction. It rejects absolute paths (POSIX

`/`, Windows drive letters like `C:\` or `C:/`, and UNC paths like `\\server\share`) and unsafe symlink targets. Path traversal components (`..`) are normalized away to prevent directory escape attacks: `dir/sub/../file` becomes `dir/file`.
### Filtering Extracted Files

Use glob patterns to extract only specific files. Patterns are matched against archive entry paths normalized to use forward slashes (`/`). Positive patterns specify what to include, and negative patterns (prefixed with `!`) specify what to exclude. When only negative patterns are provided, all entries that don’t match them are included:
## Reading Archive Contents

### Get All Files

Use`.files()` to get archive contents as a `Map` of `File` objects without extracting to disk. Unlike `extract()`, which processes all entry types, `files()` returns only regular files (no directories):
`File` object includes:
- `name`- The file path within the archive (always uses forward slashes- `/`as separators)
- `size`- File size in bytes
- `lastModified`- Modification timestamp
- Standard `Blob`methods such as`text()`,`arrayBuffer()`, and`stream()`

**Note**:

`files()` loads file contents into memory. For large archives, use `extract()` to write directly to disk instead.
### Error Handling

Archive operations can fail due to corrupted data, I/O errors, or invalid paths. Use try/catch to handle these cases:- **Corrupted/truncated archives**-- `new Archive()`loads the archive data; errors may be deferred until read/extract operations
- **Permission denied**-- `extract()`throws if the target directory is not writable
- **Disk full**-- `extract()`throws if there’s insufficient space
- **Invalid paths**- Operations throw for malformed file paths

`files()` returns an empty `Map` if no files match:
### Filtering with Glob Patterns

Pass a glob pattern to filter which files are returned:[Bun.Glob](/docs/docs/api/glob)syntax):

- `*`- Match any characters except- `/`
- `**`- Match any characters including- `/`
- `?`- Match single character
- `[abc]`- Match character set
- `{a,b}`- Match alternatives
- `!pattern`- Exclude files matching pattern (negation). When only negative patterns are provided, all files not matching them are included.

[Bun.Glob](/docs/docs/api/glob)for the full glob syntax including escaping and advanced patterns.

## Compression

Bun.Archive creates uncompressed tar archives by default. Use`{ compress: "gzip" }` to enable gzip compression:
`options` argument accepts:
- No options or `undefined`- Uncompressed tar (default)
- `{ compress: "gzip" }`- Enable gzip compression at level 6
- `{ compress: "gzip", level: number }`- Gzip with custom level 1-12 (1 = fastest, 12 = smallest)

## Examples

### Bundle Project Files

### Extract and Process npm Package

### Create Archive from Directory

## Reference

Note: The following type signatures are simplified. See[for the full type definitions.](https://github.com/oven-sh/bun/blob/main/packages/bun-types/bun.d.ts)`packages/bun-types/bun.d.ts`

# Citations

1. Source page: https://bun.sh/docs/runtime/archive
