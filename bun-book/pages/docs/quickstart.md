---
type: Web Page
title: Quickstart - Bun
description: Build your first app with Bun
resource: https://bun.sh/docs/quickstart
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

## Overview

Build a minimal HTTP server with`Bun.serve`, run it locally, then evolve it by installing a package.
Prerequisites: Bun installed and available on your 

`PATH`. See [installation](/docs/installation)for setup.Step 1

Initialize a new project with The new 

`bun init`.terminal

`bun init` prompts you to pick a template: `Blank`, `React`, or `Library`. For this guide, pick `Blank`.terminal

`my-app` directory contains a basic Bun app.Step 3

Replace the contents of Run Visit 

Then add the following to your 

`index.ts` with the following code:index.ts

`index.ts` again.terminal

[to test the server. You should see a page that says](http://localhost:3000)`http://localhost:3000``"Bun!"`.Seeing TypeScript errors on Bun?

Seeing TypeScript errors on Bun?

`bun init` installs Bun’s TypeScript declarations and configures your `tsconfig.json`. If you’re trying out Bun in an existing project, you may see a type error on the `Bun` global.To fix this, first install `@types/bun` as a dev dependency.terminal

`compilerOptions` in `tsconfig.json`:tsconfig.json

Step 4

Install the Update Run Visit 

`figlet` package and its type declarations. Figlet is a utility for converting strings into ASCII art.terminal

`index.ts` to use `figlet` in `routes`.index.ts

`index.ts` again.terminal

[to test the server. You should see a page that says](http://localhost:3000/figlet)`http://localhost:3000/figlet``"Bun!"` in ASCII art.Step 5

Now add some HTML. Create a new file called Then, import this file in Run Visit 

`index.html`:index.html

`index.ts` and serve it from the root `/` route.index.ts

`index.ts` again.terminal

[to test the server. You should see the static HTML page.](http://localhost:3000)`http://localhost:3000`## Run a script

Bun can also execute`"scripts"` from your `package.json`. Add the following script:
package.json

`bun run start`.
terminal

⚡️ 

**Performance**—`bun run` is roughly 28x faster than `npm run` (6ms vs 170ms of overhead).

# Citations

1. Source page: https://bun.sh/docs/quickstart
