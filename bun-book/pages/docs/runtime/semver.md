---
type: Web Page
title: Semver | Bun Docs
description: Use Bun's semantic versioning API
resource: https://bun.sh/docs/runtime/semver
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Semver

Use Bun's semantic versioning API

`Bun.semver` compares semantic versions and checks whether a version is compatible with a range of versions. Versions and ranges are designed to be compatible with `node-semver`, which npm clients use.

`Bun.semver` is about 20x faster than `node-semver`.

The API provides two functions:

## `Bun.semver.satisfies(version: string, range: string): boolean`

Returns `true` if `version` satisfies `range`, otherwise `false`.

Example:

```
import { semver } from "bun";
semver.satisfies("1.0.0", "^1.0.0"); // true
semver.satisfies("1.0.0", "^1.0.1"); // false
semver.satisfies("1.0.0", "~1.0.0"); // true
semver.satisfies("1.0.0", "~1.0.1"); // false
semver.satisfies("1.0.0", "1.0.0"); // true
semver.satisfies("1.0.0", "1.0.1"); // false
semver.satisfies("1.0.1", "1.0.0"); // false
semver.satisfies("1.0.0", "1.0.x"); // true
semver.satisfies("1.0.0", "1.x.x"); // true
semver.satisfies("1.0.0", "x.x.x"); // true
semver.satisfies("1.0.0", "1.0.0 - 2.0.0"); // true
semver.satisfies("1.0.0", "1.0.0 - 1.0.1"); // true
```
If `range` or `version` is invalid, it returns `false`.

## `Bun.semver.order(versionA: string, versionB: string): 0 | 1 | -1`

Returns `0` if `versionA` and `versionB` are equal, `1` if `versionA` is greater than `versionB`, and `-1` if `versionA` is less than `versionB`.

Example:

```
import { semver } from "bun";
semver.order("1.0.0", "1.0.0"); // 0
semver.order("1.0.0", "1.0.1"); // -1
semver.order("1.0.1", "1.0.0"); // 1
const unsorted = ["1.0.0", "1.0.1", "1.0.0-alpha", "1.0.0-beta", "1.0.0-rc"];
unsorted.sort(semver.order); // ["1.0.0-alpha", "1.0.0-beta", "1.0.0-rc", "1.0.0", "1.0.1"]
console.log(unsorted);
```
If you need other semver functions, open an issue or pull request.

# Citations

1. Source page: https://bun.sh/docs/runtime/semver
