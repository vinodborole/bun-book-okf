---
type: Web Page
title: Node-API | Bun Docs
description: Use Bun's Node-API module to build native add-ons to Node.js
resource: https://bun.sh/docs/runtime/node-api
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Node-API

Use Bun's Node-API module to build native add-ons to Node.js

Node-API is an interface for building native add-ons to Node.js. Bun implements this interface from scratch, so most existing Node-API extensions work with Bun out of the box.

As in Node.js, you can `require()` `.node` files (Node-API modules) directly.

`const napi = require("./my-node-module.node");`
Alternatively, use `process.dlopen`:

```
let mod = { exports: {} };
process.dlopen(mod, "./my-node-module.node");
```

# Citations

1. Source page: https://bun.sh/docs/runtime/node-api
