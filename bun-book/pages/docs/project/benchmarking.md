---
type: Web Page
title: Benchmarking - Bun
description: How to benchmark Bun
resource: https://bun.sh/docs/project/benchmarking
timestamp: '2026-08-03T08:59:43.078871+00:00'
---

[directory of the Bun repo.](https://github.com/oven-sh/bun/tree/main/bench)

`/bench`
## Measuring time

To measure time precisely, Bun offers two runtime APIs:
1. The Web-standard [`performance.now()`](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) function
2. `Bun.nanoseconds()` , which is like`performance.now()` except it returns the time since the application started in nanoseconds. Use`performance.timeOrigin` to convert this to a Unix timestamp.

## Benchmarking tools

- For microbenchmarks, we recommend [`mitata`](https://github.com/evanwashere/mitata) .
- For load testing, you *must use* an HTTP benchmarking tool that is at least as fast as`Bun.serve()` , or your results will be skewed. Some popular Node.js-based benchmarking tools like[`autocannon`](https://github.com/mcollina/autocannon) are not fast enough. We recommend one of the following:
- For benchmarking scripts or CLI commands, we recommend [`hyperfine`](https://github.com/sharkdp/hyperfine) .

## Measuring memory usage

Bun has two heaps: one for the JavaScript runtime, and one for everything else.
### JavaScript heap stats

The`bun:jsc` module exposes a few functions for measuring memory usage:
## View example statistics

View example statistics

`Bun.generateHeapSnapshot()` to take a heap snapshot, then view it with Safari or WebKit GTK developer tools. To generate a heap snapshot:
`heap.json` file in Safari’s Developer Tools (or WebKit GTK):
1. Open the Developer Tools
2. Click “Timeline”
3. Click “JavaScript Allocations” in the menu on the left. It might not be visible until you click the pencil icon to show all the timelines
4. Click “Import” and select your heap snapshot JSON

The [web debugger](/docs/runtime/debugger#inspect) timeline also tracks the memory usage of the running debug session.

### Native heap stats

Bun uses mimalloc for the other heap. To print a summary of non-JavaScript memory usage, call`Bun.unsafe.mimallocDump()`.
## CPU profiling

Profile JavaScript execution to identify performance bottlenecks with the`--cpu-prof` flag.
terminal

`--cpu-prof` writes a `.cpuprofile` file you can open in Chrome DevTools (Performance tab → Load profile) or VS Code’s CPU profiler.
### Markdown output

Use`--cpu-prof-md` to generate a markdown CPU profile, which is grep-friendly and designed for LLM analysis:
terminal

`--cpu-prof` and `--cpu-prof-md` to generate both formats at once:
terminal

`BUN_OPTIONS` environment variable:
terminal

### Options

terminal

## Heap profiling

Generate heap snapshots on exit to analyze memory usage and find memory leaks.
terminal

`--heap-prof` writes a V8 `.heapsnapshot` file you can load in Chrome DevTools (Memory tab → Load).
### Markdown output

Use`--heap-prof-md` to generate a markdown heap profile for CLI analysis:
terminal

If both 

`--heap-prof` and `--heap-prof-md` are specified, the markdown format is used.
### Options

terminal

# Citations

1. Source page: https://bun.sh/docs/project/benchmarking
