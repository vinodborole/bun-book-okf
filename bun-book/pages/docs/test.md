---
type: Web Page
title: Test runner - Bun
description: Bun's fast, built-in, Jest-compatible test runner with TypeScript support,
  lifecycle hooks, mocking, and watch mode
resource: https://bun.sh/docs/test
timestamp: '2026-08-10T07:07:25.236908+00:00'
---

- TypeScript and JSX
- Lifecycle hooks
- Snapshot testing
- UI & DOM testing
- Watch mode with `--watch`
- Script pre-loading with `--preload`

Bun aims for compatibility with Jest, but not everything is implemented. To track compatibility, see 

[this tracking issue](https://github.com/oven-sh/bun/issues/1825).
## Run tests

terminal

[Writing tests](/docs/test/writing-tests).

math.test.ts

- `*.test.{js|jsx|ts|tsx|mjs|cjs|mts|cts}`
- `*_test.{js|jsx|ts|tsx|mjs|cjs|mts|cts}`
- `*.spec.{js|jsx|ts|tsx|mjs|cjs|mts|cts}`
- `*_spec.{js|jsx|ts|tsx|mjs|cjs|mts|cts}`

*test files*to run, pass additional positional arguments to

`bun test`. Any test file with a path that matches one of the filters runs. Filters are commonly file or directory names; glob patterns are not yet supported.
terminal

*test name*, use the

`-t`/`--test-name-pattern` flag.
terminal

`./` or `/` to distinguish it from a filter name.
terminal

`--preload` scripts (see [Lifecycle](/docs/test/lifecycle)), then runs every file in one shared global. Pass

[to spread files across CPU cores instead. If a test fails, the test runner exits with a non-zero exit code.](/docs/test/parallel)

`--parallel`
## CI/CD integration

`bun test` supports a variety of CI/CD integrations.
### GitHub Actions

`bun test` automatically detects when it’s running inside GitHub Actions and emits GitHub Actions annotations to the console directly.
No configuration is needed, other than installing `bun` in the workflow and running `bun test`.
#### How to install `bun` in a GitHub Actions workflow

To use `bun test` in a GitHub Actions workflow, add the following step:
.github/workflows/test.yml

### JUnit XML reports (GitLab, etc.)

To write a JUnit XML report, pass`--reporter=junit` together with `--reporter-outfile`.
terminal

`bun test` still writes to stdout/stderr as usual, and writes the JUnit XML report to the given path at the end of the run.
JUnit XML is a popular format for reporting test results in CI/CD pipelines.
## Timeouts

Use the`--timeout` flag to specify a *per-test*timeout in milliseconds. If a test times out, it is marked as failed. The default value is

`5000`.
terminal

## Concurrent test execution

To run test
**files**across CPU cores, see

[. The flags below control concurrency of tests](/docs/test/parallel)

`--parallel`
*within*a file. By default, Bun runs all tests sequentially within each test file. Concurrent execution runs async tests in parallel, which speeds up test suites with independent tests.

### `--concurrent` flag

Use the `--concurrent` flag to run all tests concurrently within their respective files:
terminal

`test.serial`.
### `--max-concurrency` flag

Control the maximum number of tests running simultaneously with the `--max-concurrency` flag:
terminal

### `test.concurrent`

Mark individual tests to run concurrently, even when the `--concurrent` flag is not used:
math.test.ts

### `test.serial`

Force tests to run sequentially, even when the `--concurrent` flag is enabled:
math.test.ts

## Retry failed tests

Use the`--retry` flag to automatically retry failed tests up to a given number of times. If a test fails and then passes on a subsequent attempt, it is reported as passing.
terminal

`{ retry: N }` overrides the global `--retry` value:
`bunfig.toml`:
bunfig.toml

## Rerun tests

Use the`--rerun-each` flag to run each test multiple times. This surfaces flaky or non-deterministic test failures.
terminal

## Randomize test execution order

Use the`--randomize` flag to run tests in a random order. This helps detect tests that depend on shared state or execution order.
terminal

`--randomize`, the seed used for randomization is displayed in the test summary:
terminal

### Reproducible random order with `--seed`

Use the `--seed` flag to specify the randomization seed and reproduce the same test order when debugging order-dependent failures.
terminal

`--seed` flag implies `--randomize`, so you don’t need to specify both. The same seed always produces the same test execution order.
## Bail out with `--bail`

Use the `--bail` flag to abort the test run after a given number of test failures. By default, Bun runs all tests and reports all failures, but in CI it can be preferable to stop early and reduce CPU usage.
terminal

## Watch mode

Like`bun run`, `bun test` accepts the `--watch` flag to watch for changes and re-run tests.
terminal

## Lifecycle hooks

Bun supports the following lifecycle hooks:
Define hooks inside test files, or in a separate file preloaded with the 

`--preload` flag.
terminal

[Lifecycle](/docs/test/lifecycle).

## Mocks

Create mock functions with the`mock` function.
math.test.ts

`jest.fn()`; it behaves identically.
math.test.ts

[Mocks](/docs/test/mocks).

## Snapshot testing

`bun test` supports snapshot testing.
math.test.ts

`--update-snapshots` flag.
terminal

[Snapshots](/docs/test/snapshots).

## UI & DOM testing

Bun is compatible with popular UI testing libraries: See
[DOM testing](/docs/test/dom).

## Large codebases

For a suite with thousands of test files,`bun test` has several knobs that stack — worker processes, isolation level, sharding across machines, and duration-aware scheduling. Each is covered in depth on [Parallel & isolated test runs](/docs/test/parallel); here is how they fit together, roughly in order of payoff:

**1. Use every core:**One worker per core, files handed out one at a time.

[.](/docs/test/parallel#--parallel)`--parallel`
**2. Decide how much isolation you need.**

`--parallel` gives every file a fresh global, which is the safe default and what Jest/Vitest do. If your files don’t leak state into each other (they already pass under plain `bun test`, which shares one global), [lets each worker evaluate your imports and preloads once instead of once per file. On suites made of many small files that is the single biggest win — see](/docs/test/parallel#every-file-is-isolated-unless-you-opt-out)

`--parallel --no-isolate`
[how it compares](/docs/test/parallel#how-it-compares).

**3. Split across machines:**Deterministic, no coordinator; each CI job runs one slice, and each slice still uses

[.](/docs/test/parallel#splitting-a-suite-across-ci-machines-with---shard)`--shard=i/n``--parallel` locally.
**4. Balance by time, not count:**With recorded durations, shards are cut so each gets about the same total time (longest-processing-time style, but keeping path-neighbours together so a worker’s module cache stays warm), each worker starts its slowest file first, and idle workers steal the slowest remaining file — so the run isn’t held up by one long file that happened to start last.

[.](/docs/test/parallel#balancing-with---timings)`--timings`
**5. Keep the timings fresh automatically:**Each shard writes the durations of the files it ran; the next run reads all of them. In GitHub Actions that looks like:

`--update-timings`.
.github/workflows/test.yml

*same set*of timings files for the shards to add up to the whole suite, which is why a run reads the previous run’s files (restored from the cache) and writes its own where sibling shards still in flight won’t pick them up (

`next/` above). Add `--no-isolate` to the `bun test` line if step 2 applies to you.
**6. Within a file:**for I/O-bound tests that spend their time awaiting.

`test.concurrent`
## Performance

Bun’s test runner is fast.
## AI Agent Integration

When you use Bun’s test runner with an AI coding assistant, you can enable quieter output that keeps failure details but drops the rest of the noise.
### Environment Variables

Set any of the following environment variables to enable AI-friendly output:
- `CLAUDECODE=1` - For Claude Code
- `REPL_ID=1` - For Replit
- `AGENT=1` - Generic AI agent flag

### Behavior

When an AI agent environment is detected:
- Only test failures are displayed in detail
- Passing, skipped, and todo test indicators are hidden
- Summary statistics remain intact

terminal

# CLI Usage

### Execution Control

number

default:"5000"

Set the per-test timeout in milliseconds (default 5000)

number

Re-run each test file 

`NUMBER` times to help catch certain bugs
number

Retry failed tests up to 

`NUMBER` times. Overridden by per-test boolean

Treat all tests as 

`test.concurrent()` tests
boolean

Run tests in random order

number

Set the random seed for test randomization

number

default:"1"

Exit the test suite after 

`NUMBER` failures. If you do not specify a number, it defaults to 1.
number

default:"20"

Maximum number of concurrent tests to execute at once (default 20)

### Test Filtering

boolean

Include tests that are marked with 

`test.todo()`
string

Run only tests with a name that matches the given regex. Alias: 

`-t`
### Reporting

string

Test output reporter format. Available: 

`junit` (requires —reporter-outfile), `dots`. Default:
console output.
string

Output file path for the reporter format (required with —reporter)

boolean

Enable dots reporter. Shorthand for —reporter=dots

### Coverage

boolean

Generate a coverage profile

string

default:"text"

Report coverage in 

`text` and/or `lcov`. Defaults to `text`
string

default:"coverage"

Directory for coverage files. Defaults to 

`coverage`
### Snapshots

boolean

Update snapshot files. Alias: 

`-u`
## Examples

Run all test files:
terminal

terminal

terminal

# Citations

1. Source page: https://bun.sh/docs/test
