---
type: Web Page
title: Watch Mode | Bun Docs
description: Automatic reloading in Bun with --watch and --hot modes
resource: https://bun.sh/docs/runtime/watch-mode
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Watch Mode

Automatic reloading in Bun with --watch and --hot modes

Bun supports two kinds of automatic reloading:

- `--watch` mode, which hard restarts Bun's process when imported files change.
- `--hot` mode, which soft reloads the code (without restarting the process) when imported files change.

## `--watch` mode

Watch mode works with `bun test` and `bun build`, and when running TypeScript, JSX, and JavaScript files.

To run a file in `--watch` mode:

`bun --watch index.tsx`
To run your tests in `--watch` mode:

`bun --watch test`
In `--watch` mode, Bun keeps track of all imported files and watches them for changes. When a file changes, Bun restarts the process with the same CLI arguments and environment variables as the initial run. If Bun crashes, `--watch` attempts to restart the process.

**⚡️ Reloads are fast.** The filesystem watchers you're probably used to have several layers of libraries wrapping the native APIs or, worse, rely on polling.

Instead, Bun uses the operating system's native filesystem watcher APIs, like kqueue or inotify, to detect file changes. Bun also applies several optimizations to scale to larger projects, such as setting a high rlimit for file descriptors, statically allocating file path buffers, and reusing file descriptors when possible.

The following examples show Bun live-reloading a file as you edit it, with VSCode configured to save the file [on each keystroke](https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save).

`bun run --watch watchy.tsx````
import { serve } from "bun";
console.log("I restarted at:", Date.now());
serve({
  port: 4003,
  fetch(request) {
    return new Response("Sup");
  },
});
```
Running `bun test` in watch mode with `save-on-keypress` enabled:

`bun --watch test`
The **`--no-clear-screen`** flag, like TypeScript's `--preserveWatchOutput`, keeps Bun from clearing the terminal in
watch mode. Use it when running multiple `bun build --watch` commands at the same time with a tool like
`concurrently`, where one instance clearing the screen could hide another's errors: ```
bun build --watch
  --no-clear-screen
```
.

Before each restart, `bun run --watch` runs the handlers your script registered for the kill signal. The default is
`SIGTERM`, matching the signal Node.js sends its watched process. Use **`--watch-kill-signal`** to pick a different
signal, e.g. `bun --watch --watch-kill-signal SIGINT index.ts`.

## `--hot` mode

Use `bun --hot` to enable hot reloading when executing code with Bun. Unlike `--watch` mode, Bun doesn't hard-restart the entire process. It detects code changes and updates its internal module cache with the new code.

Bun's `--hot` is not the same as hot reloading in the browser. Many frameworks provide a "hot reloading" experience,
where you can edit & save your frontend code (say, a React component) and see the changes reflected in the browser
without refreshing the page. Bun's `--hot` is the server-side equivalent of this experience. To get hot reloading in
the browser, use a framework like [Vite](https://vite.dev).

`bun --hot server.ts`
Starting from the entrypoint (`server.ts` in this example), Bun builds a registry of all imported source files (excluding those in `node_modules`) and watches them for changes. When a file changes, Bun performs a "soft reload". Bun re-evaluates all files, but global state (notably, the `globalThis` object) persists.

```
// make TypeScript happy
declare global {
  var count: number;
}
globalThis.count ??= 0;
console.log(`Reloaded ${globalThis.count} times`);
globalThis.count++;
// prevent `bun run` from exiting
setInterval(function () {}, 1000000);
```
If you run this file with `bun --hot server.ts`, you'll see the reload count increment every time you save the file.

`bun --hot server.ts````
Reloaded 1 times
Reloaded 2 times
Reloaded 3 times
```
Traditional file watchers like `nodemon` restart the entire process, so HTTP servers and other stateful objects are lost. By contrast, `bun --hot` reflects the updated code without restarting the process.

### HTTP servers

You can update your HTTP request handler without shutting down the server: when you save the file, Bun reloads the server with the updated code without restarting the process. This results in seriously fast refresh speeds.

```
globalThis.count ??= 0;
globalThis.count++;
Bun.serve({
  fetch(req: Request) {
    return new Response(`Reloaded ${globalThis.count} times`);
  },
  port: 3000,
});
```
Support for Vite's `import.meta.hot` is planned, to enable better lifecycle management for hot reloading and to align with the ecosystem.

## Implementation details

On hot reload, Bun:

- Resets the internal `require` cache and ES module registry (`Loader.registry` )
- Runs the garbage collector synchronously (to minimize memory leaks, at the cost of runtime performance)
- Re-transpiles all of your code from scratch (including sourcemaps)
- Re-evaluates the code with JavaScriptCore

This implementation isn't particularly optimized. It re-transpiles files that haven't changed. It makes no attempt at incremental compilation. It's a starting point.

# Citations

1. Source page: https://bun.sh/docs/runtime/watch-mode
