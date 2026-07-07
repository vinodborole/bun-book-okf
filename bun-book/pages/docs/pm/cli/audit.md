---
type: Web Page
title: bun audit - Bun
description: Check your installed packages for known security vulnerabilities
resource: https://bun.sh/docs/pm/cli/audit
timestamp: '2026-07-07T10:59:41.879776+00:00'
---

`bun.lock` file:
terminal

### Filtering options

**- Only show vulnerabilities at this severity level or higher:**

`--audit-level=<low|moderate|high|critical>`terminal

**- Audit only production dependencies (excludes devDependencies):**

`--prod`terminal

**- Ignore specific CVEs (repeat the flag to ignore several):**

`--ignore <CVE>`terminal

`--json`

Use the `--json` flag to print the raw JSON response from the registry instead of the formatted report:
terminal

### Exit code

`bun audit` exits with code `0` if no vulnerabilities are found and `1` if the report lists any, including when `--json` is passed.

# Citations

1. Source page: https://bun.sh/docs/pm/cli/audit
