---
type: Web Page
title: REPL - Bun
description: An interactive JavaScript and TypeScript REPL with syntax highlighting,
  history, and tab completion
resource: https://bun.sh/docs/runtime/repl
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`bun repl` starts an interactive Read-Eval-Print Loop (REPL) for evaluating JavaScript and TypeScript expressions. Use it to test code snippets, explore APIs, and debug.
terminal

## Features

- **TypeScript & JSX**— Write TypeScript and JSX directly. Bun transpiles everything on the fly.
- **Top-level**— Await promises directly at the prompt without wrapping in an async function.- `await`
- **Syntax highlighting**— Input is highlighted as you type.
- **Persistent history**— History is saved to- `~/.bun_repl_history`and persists across sessions.
- **Tab completion**— Press- `Tab`to complete property names and REPL commands.
- **Multi-line input**— Unclosed brackets, braces, and parentheses automatically continue on the next line.
- **Node.js globals**—- `require`,- `module`,- `__dirname`, and- `__filename`are available, resolved relative to your current working directory.

## Special variables

The REPL exposes two special variables that update after each evaluation.## Top-level `await`

You can `await` any expression directly at the prompt.
## Importing modules

Just like Bun’s runtime, the REPL accepts both`require` and `import`: mix ES modules and CommonJS freely at the prompt. Module resolution uses the same rules as `bun run`, so you can import from `node_modules`, relative paths, or `node:` builtins.
`const`/`let` can be redeclared across evaluations (unlike in regular scripts), so you can re-run `import` and `require` statements while iterating.
## Multi-line input

When you press`Enter` on a line with unclosed brackets, braces, or parentheses, the REPL automatically continues on the next line. The prompt changes to `...` to indicate continuation.
`.editor` to enter editor mode, which buffers all input until you press `Ctrl+D`.
## REPL commands

Type`.help` at the prompt to see all available REPL commands.
## Keybindings

The REPL supports Emacs-style line editing.## History

REPL history is automatically saved to`~/.bun_repl_history` (up to 1000 entries) and loaded at the start of each session. Use `Up`/`Down` to navigate.
To export your history to a different file, use `.save`:
## Non-interactive mode

Use`-e` / `--eval` to evaluate a script with REPL semantics and exit. Use `-p` / `--print` to additionally print the result.
terminal

`{ a: 1 }` is treated as an object expression instead of a block statement. The process exits after the event loop drains (pending timers and I/O complete first). On error, the process exits with code `1`.

# Citations

1. Source page: https://bun.sh/docs/runtime/repl
