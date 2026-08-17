---
type: Web Page
title: bun init | Bun Docs
description: Scaffold an empty Bun project with the interactive `bun init` command
resource: https://bun.sh/docs/runtime/templating/init
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# bun init

Scaffold an empty Bun project with the interactive `bun init` command

Scaffold a new Bun project with `bun init`.

`bun init my-app````
? Select a project template - Press return to submit.
❯ Blank
  React
  Library
✓ Select a project template: Blank
 + .gitignore
 + CLAUDE.md
 + .cursor/rules/use-bun-instead-of-node-vite-npm-pnpm.mdc -> CLAUDE.md
 + index.ts
 + tsconfig.json (for editor autocomplete)
 + README.md
```
Press `enter` to accept the default answer for each prompt, or pass the `-y` flag to auto-accept the defaults.

`bun init` infers settings with sane defaults and is non-destructive when run multiple times.

It creates:

- a `package.json` file with a name that defaults to the current directory name
- a `tsconfig.json` or`jsconfig.json` file, depending on whether the entry point is a TypeScript file
- an entry point, which defaults to `index.ts` unless any of`index.{tsx, jsx, js, mts, mjs}` exist or the`package.json` specifies a`module` or`main` field
- a `README.md` file

AI Agent rules (disable with `$BUN_AGENT_RULE_DISABLED=1`):

- a `CLAUDE.md` file when`bun init` detects Claude CLI (disable with`CLAUDE_CODE_AGENT_RULE_DISABLED` env var)
- a `.cursor/rules/*.mdc` file when`bun init` detects Cursor (disable with`CURSOR_AGENT_RULE_DISABLED` env var); the file tells[Cursor AI](https://cursor.sh) to use Bun instead of Node.js and npm

Pass `-y` or `--yes` to accept the defaults without prompting.

At the end, it runs `bun install` to install `@types/bun`.

## CLI Usage

`bun init <folder?>`
### Initialization Options

Accept all default prompts without asking questions. Alias: `-y` 

Only initialize type definitions (skip app scaffolding). Alias: `-m` 

### Project Templates

Scaffold a React project. When used without a value, creates a baseline React app.

 Accepts values for presets: 

- `tailwind` – React app preconfigured with Tailwind CSS
- `shadcn` – React app with`@shadcn/ui` and Tailwind CSS

Examples:

bun init --react
bun init --react=tailwind
bun init --react=shadcn

### Output & Files

Initializes project files and configuration for the chosen options. Exact files vary by template.

### Help

Print this help menu. Alias: `-h` 

### Examples

- Accept all defaults terminal`bun init -y`
- React terminal`bun init --react`
- React + Tailwind CSS terminal`bun init --react=tailwind`
- React + @shadcn/ui terminal`bun init --react=shadcn`

# Citations

1. Source page: https://bun.sh/docs/runtime/templating/init
