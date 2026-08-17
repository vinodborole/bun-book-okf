---
type: Web Page
title: Feedback | Bun Docs
description: Share feedback, bug reports, and feature requests
resource: https://bun.sh/docs/feedback
timestamp: '2026-08-17T06:30:47.177846+00:00'
---

# Feedback

Share feedback, bug reports, and feature requests

Here's how to open a helpful issue for a bug, a performance problem, or a feature request:

[Discord](https://bun.com/discord).

## Reporting Issues

Upgrade Bun

Upgrade Bun to the latest version with `bun upgrade`. This might fix your problem without opening an issue.

`bun upgrade`
You can also try the latest canary release, which includes changes and bug fixes that haven't reached a stable release yet.

```
bun upgrade --canary
# To revert to the stable release
bun upgrade --stable
```
If the issue persists after upgrading, continue to the next step.

Review Existing Issues

Check whether the issue has already been reported before opening a new one. Checking first saves time for everyone and helps us focus on fixing things.

If you find a related issue, add a 👍 reaction or comment with extra details instead of opening a new one.

Report the Issue

If no one has reported the issue, open a new one or suggest an improvement.

Provide as much detail as possible, including:

- A clear and concise title
- A code example or steps to reproduce the issue
- The version of Bun you are using (run `bun --version` )
- A description of the issue (what you expected to happen and what actually happened)
- The operating system and version you are using
  - For macOS and Linux: copy the output of `uname -mprs`
  - For Windows: copy the output of this command in the PowerShell console:
`"$([Environment]::OSVersion | ForEach-Object VersionString) $(if ([Environment]::Is64BitOperatingSystem) { "x64" } else { "x86" })"`
- For macOS and Linux: copy the output of 

The Bun team will review the issue and get back to you as soon as possible.

# Citations

1. Source page: https://bun.sh/docs/feedback
