---
type: Web Page
title: Semver - Bun
description: Use Bun's semantic versioning API
resource: https://bun.sh/docs/runtime/semver
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`Bun.semver` compares semantic versions and checks whether a version is compatible with a range of versions. Versions and ranges are designed to be compatible with `node-semver`, which npm clients use.
It’s about 20x faster than `node-semver`.
`Bun.semver.satisfies(version: string, range: string): boolean`

Returns `true` if `version` satisfies `range`, otherwise `false`.
Example:
`range` or `version` is invalid, it returns `false`.
`Bun.semver.order(versionA: string, versionB: string): 0 | 1 | -1`

Returns `0` if `versionA` and `versionB` are equal, `1` if `versionA` is greater than `versionB`, and `-1` if `versionA` is less than `versionB`.
Example:

# Citations

1. Source page: https://bun.sh/docs/runtime/semver
