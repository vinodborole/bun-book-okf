---
type: Web Page
title: Console - Bun
description: The console object in Bun
resource: https://bun.sh/docs/runtime/console
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

Bun provides a browser- and Node.js-compatible console
global. This page only documents Bun-native APIs.

## Object inspection depth

You can configure how deeply`console.log()` prints nested objects:
- **CLI flag**: Use- `--console-depth <number>`to set the depth for a single run
- **Configuration**: Set- `console.depth`in your- `bunfig.toml`to persist it across runs
- **Default**: Objects are inspected to a depth of- `2`levels

## Reading from stdin

In Bun, the`console` object is also an `AsyncIterable` that reads `process.stdin` line by line.
adder.ts

adder.ts

terminal

# Citations

1. Source page: https://bun.sh/docs/runtime/console
