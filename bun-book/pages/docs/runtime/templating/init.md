---
type: Web Page
title: bun init - Bun
description: Scaffold an empty Bun project with the interactive bun init command
resource: https://bun.sh/docs/runtime/templating/init
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`bun init`.
terminal

`enter` to accept the default answer for each prompt, or pass the `-y` flag to auto-accept the defaults.
`bun init` infers settings with sane defaults and is non-destructive when run multiple times.
- a `package.json`file with a name that defaults to the current directory name
- a `tsconfig.json`or`jsconfig.json`file, depending on whether the entry point is a TypeScript file
- an entry point, which defaults to `index.ts`unless any of`index.{tsx, jsx, js, mts, mjs}`exist or the`package.json`specifies a`module`or`main`field
- a `README.md`file

`$BUN_AGENT_RULE_DISABLED=1`):
- a `CLAUDE.md`file when Claude CLI is detected (disable with`CLAUDE_CODE_AGENT_RULE_DISABLED`env var)
- a `.cursor/rules/*.mdc`file when Cursor is detected, which tells[Cursor AI](https://cursor.sh)to use Bun instead of Node.js and npm

`-y` or `--yes` to accept the defaults without prompting.
At the end, it runs `bun install` to install `@types/bun`.
## CLI Usage

terminal

### Initialization Options

Accept all default prompts without asking questions. Alias: 

`-y`Only initialize type definitions (skip app scaffolding). Alias: 

`-m`### Project Templates

Scaffold a React project. When used without a value, creates a baseline React app.

Accepts values for presets:

Accepts values for presets:

- `tailwind`– React app preconfigured with Tailwind CSS
- `shadcn`– React app with- `@shadcn/ui`and Tailwind CSS

`bun init —react bun init —react=tailwind bun init —react=shadcn`### Output & Files

Initializes project files and configuration for the chosen options. Exact files vary by template.

### Help

Print this help menu. Alias: 

`-h`### Examples

- 
Accept all defaults
terminal
- 
React
terminal
- 
React + Tailwind CSS
terminal
- 
React + @shadcn/ui
terminal

# Citations

1. Source page: https://bun.sh/docs/runtime/templating/init
