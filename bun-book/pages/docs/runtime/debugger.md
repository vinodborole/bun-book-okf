---
type: Web Page
title: Debugging - Bun
description: Debug your Bun code with an interactive debugger using WebKit Inspector
  Protocol
resource: https://bun.sh/docs/runtime/debugger
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

## Debugging JavaScript and TypeScript

server.ts

`--inspect`

To enable debugging when running code with Bun, use the `--inspect` flag. This starts a WebSocket server on an available port for introspecting the running Bun process.
terminal

`--inspect-brk`

The `--inspect-brk` flag behaves identically to `--inspect`, except it injects a breakpoint at the first line of the executed script. Use it to debug scripts that run quickly and exit immediately.
`--inspect-wait`

The `--inspect-wait` flag behaves identically to `--inspect`, except the code does not execute until a debugger attaches to the running process.
### Setting a port or URL for the debugger

Regardless of which flag you use, you can specify a port number, URL prefix, or both.terminal

## Debuggers

Several debugging tools can connect to this server.`debug.bun.sh`

Bun hosts a web-based debugger at debug.bun.sh. It is a modified version of WebKit’s Web Inspector Interface, which looks familiar to Safari users.
Open the provided `debug.bun.sh` URL in your browser to start a debugging session. From this interface, you can view the source code of the running file, view and set breakpoints, and execute code with the built-in console.
`3` to set a breakpoint on the `console.log(req.url)` statement.
`http://localhost:3000` in your web browser. The page never finishes loading because the program has paused at the breakpoint.
Note how the UI has changed.
`req` here, to see its properties and methods.
- *Continue script execution*— run until the next breakpoint or exception.
- *Step over*— advance to the next line.
- *Step into*— if the current statement contains a function call, enter the called function.
- *Step out*— if the current statement is a function call, finish executing it, then return to the location where it was called.

### Visual Studio Code Debugger

Visual Studio Code support for debugging Bun scripts is experimental. To use it, install the Bun VSCode extension.## Debugging Network Requests

Set the`BUN_CONFIG_VERBOSE_FETCH` environment variable to log network requests made with `fetch()` or `node:http`.
| Value | Description | 
|---|---|
| `curl` | Print requests as `curl`commands | 
| `true` | Print request & response info | 
| `false` | Don’t print anything (default) | 

### Print fetch & node:http requests as curl commands

Set`BUN_CONFIG_VERBOSE_FETCH` to `curl` to print each `fetch()` and `node:http` request as a single-line `curl` command you can copy-paste into your terminal to replicate the request.
index.ts

`[fetch] >` are the request from your local code, and the lines with `[fetch] <` are the response from the remote server.
To print without the `curl` command, set `BUN_CONFIG_VERBOSE_FETCH` to `true`.
index.ts

## Stacktraces & sourcemaps

Bun transpiles every file, which could leave stack traces pointing at the transpiled output. To avoid this, Bun generates and serves sourcemapped files for every file it transpiles. When you see a stack trace in the console, you can click on the file path and land in the original source code, even though it was written in TypeScript or JSX, or has some other transformation applied. Bun loads sourcemaps both at runtime when transpiling files on-demand, and when using`bun build` to precompile files ahead of time.
### Syntax-highlighted source code preview

Bun prints a small source-code preview when an unhandled exception or rejection occurs. To produce the same output yourself, call`Bun.inspect(error)`:
### V8 Stack Traces

Bun uses JavaScriptCore as its engine, but much of the Node.js ecosystem and npm expects V8, and JavaScript engines differ in how they format`error.stack`. Because Bun aims to be a drop-in replacement for Node.js, it formats `error.stack` the same way V8 does. This matters most when you use libraries that expect V8 stack traces.
#### V8 Stack Trace API

Bun implements the V8 Stack Trace API, a set of functions for manipulating stack traces.`Error.prepareStackTrace`

Define a global `Error.prepareStackTrace` function to customize the stack trace output. It receives the error object and an array of `CallSite` objects, and its return value becomes `error.stack`.
`CallSite` object has the following methods:
| Method | Returns | 
|---|---|
| `getThis` | `this`value of the function call | 
| `getTypeName` | typeof `this` | 
| `getFunction` | function object | 
| `getFunctionName` | function name as a string | 
| `getMethodName` | method name as a string | 
| `getFileName` | file name or URL | 
| `getLineNumber` | line number | 
| `getColumnNumber` | column number | 
| `getEvalOrigin` | `undefined` | 
| `getScriptNameOrSourceURL` | source URL | 
| `isToplevel` | returns `true`if the function is in the global scope | 
| `isEval` | returns `true`if the function is an`eval`call | 
| `isNative` | returns `true`if the function is native | 
| `isConstructor` | returns `true`if the function is a constructor | 
| `isAsync` | returns `true`if the function is`async` | 
| `isPromiseAll` | Not implemented yet. | 
| `getPromiseIndex` | Not implemented yet. | 
| `toString` | returns a string representation of the call site | 

`Function` object has already been garbage collected, some of these methods return `undefined`.
`Error.captureStackTrace(error, startFn)`

`Error.captureStackTrace` captures a stack trace at a specific point in your code, rather than at the point where the error was thrown.
This helps when callbacks or asynchronous code make it hard to tell where an error originated. The 2nd argument to `Error.captureStackTrace` is the function where you want the stack trace to start.
In the following example, `err.stack` points to the code calling `fn()`, even though the error was thrown at `myInner`.
index.ts

# Citations

1. Source page: https://bun.sh/docs/runtime/debugger
