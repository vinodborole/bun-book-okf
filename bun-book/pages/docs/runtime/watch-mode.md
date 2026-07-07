---
type: Web Page
title: Watch Mode - Bun
description: Automatic reloading in Bun with --watch and --hot modes
resource: https://bun.sh/docs/runtime/watch-mode
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

- `--watch`mode, which hard restarts Bun’s process when imported files change.
- `--hot`mode, which soft reloads the code (without restarting the process) when imported files change.

`--watch` mode

Watch mode works with `bun test` and when running TypeScript, JSX, and JavaScript files.
To run a file in `--watch` mode:
terminal

`--watch` mode:
terminal

`--watch` mode, Bun keeps track of all imported files and watches them for changes. When a file changes, Bun restarts the process with the same CLI arguments and environment variables as the initial run. If Bun crashes, `--watch` attempts to restart the process.
**⚡️ Reloads are fast.**The filesystem watchers you’re probably used to have several layers of libraries wrapping the native APIs or, worse, rely on polling.Instead, Bun uses the operating system’s native filesystem watcher APIs, like kqueue or inotify, to detect file changes. Bun also applies several optimizations to scale to larger projects, such as setting a high rlimit for file descriptors, statically allocating file path buffers, and reusing file descriptors when possible.

terminal

watchy.tsx

`bun test` in watch mode with `save-on-keypress` enabled:
terminal

The 

**flag, like TypeScript’s**`--no-clear-screen``--preserveWatchOutput`, keeps Bun from clearing the terminal in
watch mode. Use it when running multiple `bun build --watch` commands at the same time with a tool like
`concurrently`, where one instance clearing the screen could hide another’s errors: `bun build --watch   --no-clear-screen`.`--hot` mode

Use `bun --hot` to enable hot reloading when executing code with Bun. Unlike `--watch` mode, Bun doesn’t hard-restart the entire process. It detects code changes and updates its internal module cache with the new code.
This is not the same as hot reloading in the browser. Many frameworks provide a “hot reloading” experience, where you
can edit & save your frontend code (say, a React component) and see the changes reflected in the browser without
refreshing the page. Bun’s 

`--hot` is the server-side equivalent of this experience. To get hot reloading in the
browser, use a framework like Vite.terminal

`server.ts` in this example), Bun builds a registry of all imported source files (excluding those in `node_modules`) and watches them for changes. When a file changes, Bun performs a “soft reload”. All files are re-evaluated, but global state (notably, the `globalThis` object) persists.
server.ts

`bun --hot server.ts`, you’ll see the reload count increment every time you save the file.
terminal

`nodemon` restart the entire process, so HTTP servers and other stateful objects are lost. By contrast, `bun --hot` reflects the updated code without restarting the process.
### HTTP servers

You can update your HTTP request handler without shutting down the server: when you save the file, Bun reloads the server with the updated code without restarting the process. This results in seriously fast refresh speeds.server.ts

Support for Vite’s 

`import.meta.hot` is planned, to enable better lifecycle management for hot reloading and to align with the ecosystem.Implementation details

Implementation details

On hot reload, Bun:

- Resets the internal `require`cache and ES module registry (`Loader.registry`)
- Runs the garbage collector synchronously (to minimize memory leaks, at the cost of runtime performance)
- Re-transpiles all of your code from scratch (including sourcemaps)
- Re-evaluates the code with JavaScriptCore

# Citations

1. Source page: https://bun.sh/docs/runtime/watch-mode
