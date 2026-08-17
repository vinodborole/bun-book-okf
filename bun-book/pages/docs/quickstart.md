---
type: Web Page
title: Quickstart | Bun Docs
description: Build your first app with Bun
resource: https://bun.sh/docs/quickstart
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Quickstart

Build your first app with Bun

## Overview

Build a minimal HTTP server with `Bun.serve`, run it locally, then evolve it by installing a package.

`PATH`. See [installation](/docs/installation)for setup.

Step 1

Initialize a new project with `bun init`.

`bun init my-app`
`bun init` prompts you to pick a template: `Blank`, `React`, or `Library`. For this guide, pick `Blank`.

`bun init my-app````
✓ Select a project template: Blank
+ .gitignore
+ CLAUDE.md
+ .cursor/rules/use-bun-instead-of-node-vite-npm-pnpm.mdc -> CLAUDE.md
+ index.ts
+ tsconfig.json (for editor autocomplete)
+ README.md
```
The new `my-app` directory contains a basic Bun app.

Step 2

Run `index.ts` with `bun run`.

```
cd my-app
bun run index.ts
```
`Hello via Bun!`
Step 3

Replace the contents of `index.ts` with the following code:

```
const server = Bun.serve({
  port: 3000,
  routes: {
    "/": () => new Response('Bun!'),
  }
});
console.log(`Listening on ${server.url}`);
```
Run `index.ts` again.

`bun run index.ts``Listening on http://localhost:3000/`
Visit [`http://localhost:3000`](http://localhost:3000) to test the server. You should see a page that says `"Bun!"`.

## Seeing TypeScript errors on Bun?

`bun init` installs Bun's TypeScript declarations and configures your `tsconfig.json`. If you're trying out Bun in an existing project, you may see a type error on the `Bun` global.

To fix this, first install `@types/bun` as a dev dependency.

`bun add -d @types/bun`
Then add the following to your `compilerOptions` in `tsconfig.json`:

```
{
  "compilerOptions": {
    "lib": ["ESNext"],
    "target": "ESNext",
    "module": "Preserve",
    "moduleDetection": "force",
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "verbatimModuleSyntax": true,
    "noEmit": true
  }
}
```
Step 4

Install the `figlet` package and its type declarations. Figlet is a utility for converting strings into ASCII art.

```
bun add figlet
bun add -d @types/figlet # TypeScript users only
```
Update `index.ts` to use `figlet` in `routes`.

```
import figlet from 'figlet'; 
const server = Bun.serve({
  port: 3000,
  routes: {
    "/": () => new Response('Bun!'),
    "/figlet": () => { 
      const body = figlet.textSync('Bun!'); 
      return new Response(body); 
    } 
  }
});
console.log(`Listening on ${server.url}`);
```
Run `index.ts` again.

`bun run index.ts``Listening on http://localhost:3000/`
Visit [`http://localhost:3000/figlet`](http://localhost:3000/figlet) to test the server. You should see a page that says `"Bun!"` in ASCII art.

```
____              _
| __ ) _   _ _ __ | |
|  _ \| | | | '_ \| |
| |_) | |_| | | | |_|
|____/ \__,_|_| |_(_)
```
Step 5

Now add some HTML. Create a new file called `index.html`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bun</title>
  </head>
  <body>
    <h1>Bun!</h1>
  </body>
</html>
```
Then, import this file in `index.ts` and serve it from the root `/` route.

```
import figlet from 'figlet';
import index from './index.html'; 
const server = Bun.serve({
  port: 3000,
  routes: {
    "/": index, 
    "/figlet": () => {
      const body = figlet.textSync('Bun!');
      return new Response(body);
    }
  }
});
console.log(`Listening on ${server.url}`);
```
Run `index.ts` again.

`bun run index.ts``Listening on http://localhost:3000/`
Visit [`http://localhost:3000`](http://localhost:3000) to test the server. You should see the static HTML page.

You've built an HTTP server with Bun and installed a package.

## Run a script

Bun can also execute `"scripts"` from your `package.json`. Add the following script:

```
{
  "name": "my-app",
  "module": "index.ts",
  "type": "module",
  "private": true,
  "scripts": { 
    "start": "bun run index.ts" 
  }, 
  "devDependencies": {
    "@types/bun": "latest"
  },
  "peerDependencies": {
    "typescript": "^7"
  }
}
```
Then run it with `bun run start`.

`bun run start``Listening on http://localhost:3000/`
**Performance**—

`bun run` is roughly 28x faster than `npm run` (6ms vs 170ms of overhead).

# Citations

1. Source page: https://bun.sh/docs/quickstart
