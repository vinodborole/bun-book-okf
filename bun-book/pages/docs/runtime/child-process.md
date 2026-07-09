---
type: Web Page
title: Spawn - Bun
description: Spawn child processes with Bun.spawn or Bun.spawnSync
resource: https://bun.sh/docs/runtime/child-process
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

## Spawn a process (`Bun.spawn()`)

Provide a command as an array of strings. The result of `Bun.spawn()` is a `Bun.Subprocess` object.
`Bun.spawn` is a parameters object that configures the subprocess.
## Input stream

By default, the input stream of the subprocess is undefined; configure it with the`stdin` parameter.
| Value | Description | 
|---|---|
| `null` | Default.Provide no input to the subprocess | 
| `"pipe"` | Return a `FileSink`for fast incremental writing | 
| `"inherit"` | Inherit the `stdin`of the parent process | 
| `Bun.file()` | Read from the specified file | 
| `TypedArray | DataView` | Use a binary buffer as input | 
| `Response` | Use the response `body`as input | 
| `Request` | Use the request `body`as input | 
| `ReadableStream` | Use a readable stream as input | 
| `Blob` | Use a blob as input | 
| `number` | Read from the file with a given file descriptor | 

`"pipe"`, the parent process can incrementally write to the subprocess’s input stream.
`ReadableStream` to `stdin` pipes its data directly to the subprocess’s input:
## Output streams

Read the subprocess’s output from the`stdout` and `stderr` properties. By default these are instances of `ReadableStream`.
`stdout/stderr`:
| Value | Description | 
|---|---|
| `"pipe"` | Default for Pipe the output to a`stdout`.`ReadableStream`on the returned`Subprocess`object | 
| `"inherit"` | Default for Inherit from the parent process`stderr`. | 
| `"ignore"` | Discard the output | 
| `Bun.file()` | Write to the specified file | 
| `number` | Write to the file with the given file descriptor | 

## Exit handling

Use the`onExit` callback to listen for the process exiting or being killed.
index.ts

`exited` property is a `Promise` that resolves when the process exits.
index.ts

index.ts

`bun` process does not terminate until all child processes have exited. Use `proc.unref()` to detach the child process from the parent.
index.ts

## Resource usage

After the process exits,`resourceUsage()` reports its resource usage:
index.ts

## Using AbortSignal

You can abort a subprocess using an`AbortSignal`:
index.ts

## Using timeout and killSignal

Set`timeout` to terminate a subprocess after a duration in milliseconds:
index.ts

`SIGTERM`. Specify a different signal with the `killSignal` option:
index.ts

`killSignal` option also controls which signal is sent when an AbortSignal is aborted.
## Using maxBuffer

For`Bun.spawnSync`, `maxBuffer` limits how many bytes of output the process can emit before Bun kills it:
index.ts

`maxBuffer` only by the single read that passed it, never by whatever
the process manages to write before the kill lands. This matches Node.js.
## Inter-process communication (IPC)

Bun supports a direct inter-process communication channel between two`bun` processes. To receive messages from a spawned Bun subprocess, specify an `ipc` handler.
parent.ts

`.send()` method on the returned `Subprocess` instance. The `ipc` handler also receives the sending subprocess as its second argument.
parent.ts

`process.send()` and receives them with `process.on("message")`. This is the same API used for `child_process.fork()` in Node.js.
child.ts

child.ts

`serialization` option controls the underlying communication format between the two processes:
- `advanced`: (default) Messages are serialized using the JSC- `serialize`API, which supports cloning- [everything](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm). This does not support transferring ownership of objects.- `structuredClone`supports
- `json`: Messages are serialized using- `JSON.stringify`and- `JSON.parse`, which does not support as many object types as- `advanced`does.

### IPC between Bun & Node.js

To use IPC between a`bun` process and a Node.js process, set `serialization: "json"` in `Bun.spawn`. This is because Node.js and Bun use different JavaScript engines with different object serialization formats.
bun-node-ipc.js

## Terminal (PTY) support

For interactive terminal applications, use the`terminal` option to spawn a subprocess with a pseudo-terminal (PTY) attached. The subprocess sees a real terminal, which enables colored output, cursor movement, and interactive prompts.
`terminal` option is provided:
- The subprocess sees `process.stdout.isTTY`as`true`
- `stdin`,- `stdout`, and- `stderr`are all connected to the terminal
- `proc.stdin`,- `proc.stdout`, and- `proc.stderr`return- `null`— use the terminal instead
- Access the terminal via `proc.terminal`

### Terminal options

| Option | Description | Default | 
|---|---|---|
| `cols` | Number of columns | `80` | 
| `rows` | Number of rows | `24` | 
| `name` | Terminal type for PTY configuration (set the `TERM`env var separately with the`env`option) | `"xterm-256color"` | 
| `data` | Callback when data is received `(terminal, data) => void` | — | 
| `exit` | Callback when PTY stream closes (EOF or error). `exitCode`is PTY lifecycle status (0=EOF, 1=error), not subprocess exit code. Use`proc.exited`for process exit. | — | 
| `drain` | Callback when ready for more data `(terminal) => void` | — | 

### Terminal methods

The`Terminal` object returned by `proc.terminal` has the following methods:
### Reusable Terminal

To run multiple commands in sequence through the same terminal session, create a terminal independently and reuse it across subprocesses:`Terminal` object:
- The terminal can be reused across multiple spawns
- You control when to close the terminal
- The `exit`callback fires when you call`terminal.close()`, not when each subprocess exits
- Use `proc.exited`to detect individual subprocess exits

### Platform differences

`Bun.Terminal` uses `openpty()` on Linux and macOS, and ConPTY (`CreatePseudoConsole`) on Windows. The core behavior — child sees a TTY, `write()` reaches the child’s stdin, child output reaches the `data` callback, `resize()` updates the child’s view — is the same on every platform. A few details differ:
- **No termios on Windows.**- `inputFlags`,- `outputFlags`,- `localFlags`, and- `controlFlags`always read as- `0`and setting them is a no-op.- `setRawMode()`records the flag but has no effect on the child; the child controls its own console mode.
- **No echo without a child process on Windows.**On POSIX, the kernel line discipline echoes- `write()`input back to the- `data`callback even with no process attached. ConPTY has no line discipline; input is buffered for the next reader. If you need echo, spawn a process that echoes.
- **ConPTY re-encodes output.**ConPTY renders the child’s output to a virtual screen and emits whatever VT sequences describe the result, so the- `data`callback receives semantically equivalent — but not byte-identical — escape sequences. Colors and text are preserved; cursor-positioning and reset sequences may be reordered or coalesced. ConPTY also emits a short VT init sequence (- `\x1b[?9001h\x1b[?1004h…`) before any child output.
- **Input**POSIX- `\r`is not translated to- `\n`on Windows.- `ICRNL`maps carriage return to newline on input; ConPTY passes- `\r`through unchanged.
- `process.on('SIGWINCH')`in the child does not fire under ConPTY- `process.stdout.columns`/- `rows`do update after- `resize()`. This is a libuv limitation that affects any libuv-based child (Node.js included).
- On Windows before 11 24H2 (build 26100), `terminal.close()`may not terminate a still-running child promptly because`ClosePseudoConsole`

## Blocking API (`Bun.spawnSync()`)

`Bun.spawnSync` is the blocking equivalent of `Bun.spawn`. It supports the same inputs and parameters and returns a `SyncSubprocess` object, which differs from `Subprocess` in a few ways.
- It contains a `success`property that indicates whether the process exited with a zero exit code.
- The `stdout`and`stderr`properties are instances of`Buffer`instead of`ReadableStream`.
- There is no `stdin`property. Use`Bun.spawn`to incrementally write to the subprocess’s input stream.

`Bun.spawn` API is better for HTTP servers and apps, and `Bun.spawnSync` is better for building command-line tools.
## Benchmarks

⚡️ 

`Bun.spawn` and `Bun.spawnSync` use [.](https://man7.org/linux/man-pages/man3/posix_spawn.3.html)`posix_spawn(3)``spawnSync` spawns processes 60% faster than the Node.js `child_process` module.
terminal

terminal

## Reference

The following is a reference of the Spawn API and types. The real types have complex generics to strongly type the`Subprocess` streams with the options passed to `Bun.spawn` and `Bun.spawnSync`. For full details, see [bun.d.ts](https://github.com/oven-sh/bun/blob/main/packages/bun-types/bun.d.ts).

See Typescript Definitions

# Citations

1. Source page: https://bun.sh/docs/runtime/child-process
