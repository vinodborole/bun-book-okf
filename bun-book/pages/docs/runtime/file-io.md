---
type: Web Page
title: File I/O - Bun
description: Bun provides a set of optimized APIs for reading and writing files.
resource: https://bun.sh/docs/runtime/file-io
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

The 

`Bun.file` and `Bun.write` APIs documented on this page are heavily optimized and are the recommended way to work with files in Bun. For operations they don’t cover, such as `mkdir` or `readdir`, use Bun’s nearly complete implementation of the `node:fs` module.## Reading files (`Bun.file()`)

`Bun.file(path): BunFile`
Create a `BunFile` instance with the `Bun.file(path)` function. A `BunFile` represents a lazily-loaded file; initializing it does not read the file from disk.
`Blob` interface, so the contents can be read in various formats.
`file://` URLs.
`BunFile` can point to a location on disk where a file does not exist.
`text/plain;charset=utf-8`. Override it by passing a second argument to `Bun.file`.
`stdin`, `stdout` and `stderr` as instances of `BunFile`.
### Deleting files (`file.delete()`)

Call `.delete()` to delete a file.
## Writing files (`Bun.write()`)

`Bun.write(destination, data): Promise<number>`
The `Bun.write` function is a multi-tool for writing payloads of all kinds to disk.
The first argument is the `destination`, which can be any of the following:
- `string`: A path to a location on the file system. Use the- `"path"`module to manipulate paths.
- `URL`: A- `file://`descriptor.
- `BunFile`: A file reference.

- `string`
- `Blob`(including- `BunFile`)
- `ArrayBuffer`or- `SharedArrayBuffer`
- `TypedArray`(- `Uint8Array`, et. al.)
- `Response`

See syscalls

See syscalls

| Output | Input | System call | Platform | 
|---|---|---|---|
| file | file | copy_file_range | Linux | 
| file | pipe | sendfile | Linux | 
| pipe | pipe | splice | Linux | 
| terminal | file | sendfile | Linux | 
| terminal | terminal | sendfile | Linux | 
| socket | file or pipe | sendfile (if http, not https) | Linux | 
| file (doesn’t exist) | file (path) | clonefile | macOS | 
| file (exists) | file | fcopyfile | macOS | 
| file | Blob or string | write | macOS | 
| file | Blob or string | write | Linux | 

`stdout`:
## Incremental writing with `FileSink`

Bun provides a native incremental file writing API called `FileSink`. To retrieve a `FileSink` instance from a `BunFile`:
`.write()`.
`.flush()`. This returns the number of flushed bytes.
`FileSink`’s *high water mark*is reached; that is, when its internal buffer is full. You can configure this value.

`bun` process stays alive until this `FileSink` is explicitly closed with `.end()`. To opt out of this behavior, “unref” the instance.
## Directories

Bun’s implementation of`node:fs` is fast. Use `node:fs` for working with directories in Bun.
### Reading directories (readdir)

To read a directory in Bun, use`readdir` from `node:fs`.
#### Reading directories recursively

To recursively read a directory in Bun, use`readdir` with `recursive: true`.
### Creating directories (mkdir)

To recursively create a directory, use`mkdir` in `node:fs`:
## Benchmarks

The following is a 3-line implementation of the Linux`cat` command.
cat.ts

terminal

`cat` for large files on Linux.

# Citations

1. Source page: https://bun.sh/docs/runtime/file-io
