---
type: Web Page
title: Cron - Bun
description: Schedule and parse cron jobs with Bun
resource: https://bun.sh/docs/runtime/cron
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

## Quickstart

**Run a callback on a schedule in the current process:**

**Parse a cron expression to find the next matching time:**

**Register an OS-level cron job that runs a script on a schedule:**

`Bun.cron.parse()`

Parse a cron expression and return the next matching `Date` in UTC.
### Parameters

| Parameter | Type | Description | 
|---|---|---|
| `expression` | `string` | A 5-field cron expression or predefined nickname | 
| `relativeDate` | `Date | number` | Starting point for the search (defaults to `Date.now()`) | 

### Returns

`Date | null` — the next matching time, or `null` if no match exists within 8 years (for example, February 30th).
### Chaining calls

Call`parse()` repeatedly to get a sequence of upcoming times:
## Cron expression syntax

Standard 5-field format:`minute hour day-of-month month day-of-week`
| Field | Values | Special characters | 
|---|---|---|
| Minute | `0`–`59` | `*``,``-``/` | 
| Hour | `0`–`23` | `*``,``-``/` | 
| Day of month | `1`–`31` | `*``,``-``/` | 
| Month | `1`–`12`or`JAN`–`DEC` | `*``,``-``/` | 
| Day of week | `0`–`7`or`SUN`–`SAT` | `*``,``-``/` | 

### Special characters

| Character | Description | Example | 
|---|---|---|
| `*` | All values | `* * * * *`— every minute | 
| `,` | List | `1,15 * * * *`— minute 1 and 15 | 
| `-` | Range | `9-17 * * * *`— minutes 9 through 17 | 
| `/` | Step | `*/15 * * * *`— every 15 minutes | 

### Named values

Month and weekday fields accept case-insensitive names:`0` and `7` mean Sunday in the weekday field.
### Predefined nicknames

| Nickname | Equivalent | Description | 
|---|---|---|
| `@yearly`/`@annually` | `0 0 1 1 *` | Once a year (January 1st) | 
| `@monthly` | `0 0 1 * *` | Once a month (1st day) | 
| `@weekly` | `0 0 * * 0` | Once a week (Sunday) | 
| `@daily`/`@midnight` | `0 0 * * *` | Once a day (midnight) | 
| `@hourly` | `0 * * * *` | Once an hour | 

### Time zone

`Bun.cron.parse()` and the in-process `Bun.cron(schedule, handler)` interpret schedules in **UTC**. There is no DST to handle —

`0 9 * * *` always means 9:00 UTC.
The OS-level `Bun.cron(path, schedule, title)` uses the **system’s local time zone**, because that’s how crontab, launchd, and Windows Task Scheduler work. To make the two forms agree, run the process with

`TZ=UTC`.
### Day-of-month and day-of-week interaction

When**both**day-of-month and day-of-week are specified (neither is

`*`), the expression matches when **either**condition is true. This follows the

[POSIX cron](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html)standard.

`*`), only that field is used for matching.
`Bun.cron(schedule, handler)` — in-process

Run a callback on a cron schedule inside the current process.
| In-process | [OS-level](#bun-cron-path-schedule-title-os-level) | |
|---|---|---|
| Survives process exit/reboot | No | Yes | 
| Shared state between runs | Yes | No (fresh process each time) | 
| Platform requirements | None | crontab / launchd / Task Scheduler | 
| Windows expression limits | None | [48-trigger cap](#trigger-limit) | 
| Return type | `CronJob` | `Promise<void>` | 

### Parameters

| Parameter | Type | Description | 
|---|---|---|
| `schedule` | `string` | A [cron expression](#cron-expression-syntax)or nickname like`"@hourly"`. | 
| `handler` | `(this: CronJob) => unknown` | Called on each fire. May return a Promise — the next fire is not scheduled until it settles. Inside a `function`callback,`this`is the`CronJob`(so`this.stop()`works). | 

[synchronously. Throws a](#the-cronjob-handle)

`CronJob``TypeError` if the expression is invalid or has no future occurrences, like `"0 0 30 2 *"` (February 30th).
### No-overlap guarantee

The next fire time is computed only after the handler — including any returned`Promise` — settles. If your handler takes 90 seconds and the schedule is `* * * * *`, the second fire is the first minute boundary *after*the handler finishes, not 60 seconds after the first fire. Invocations never stack.

### Error handling

Errors match`setTimeout` semantics:
- A synchronous `throw`emits`process.on("uncaughtException")`.
- A rejected returned `Promise`emits`process.on("unhandledRejection")`.

`1`. With a listener, the job keeps running — it does not stop on the first failure.
`bun --hot`

Under `bun --hot`, all in-process cron jobs are stopped immediately before the module graph re-evaluates. Every `Bun.cron()` call still in your source then re-registers. Editing the schedule, editing the handler, or deleting the line entirely all take effect on save without leaking timers.
### The `CronJob` handle

`CronJob` is [—](https://github.com/tc39/proposal-explicit-resource-management)

`Disposable``using job = Bun.cron(...)` auto-stops at scope exit. `stop()`, `ref()`, and `unref()` all return the job for chaining.
### Fake timers

In-process cron honors`jest.useFakeTimers()`. `setSystemTime()`, `advanceTimersByTime()`, and `runAllTimers()` control when it fires, so you can test scheduled callbacks without waiting on the real clock.
`Bun.cron(path, schedule, title)` — OS-level

Register an OS-level cron job that runs a JavaScript/TypeScript module on a schedule.
### Parameters

| Parameter | Type | Description | 
|---|---|---|
| `path` | `string` | Path to the script (resolved relative to caller) | 
| `schedule` | `string` | Cron expression or nickname | 
| `title` | `string` | Unique job identifier (alphanumeric, hyphens, underscores) | 

`title` overwrites the existing job in-place — the old schedule is replaced, not duplicated.
### The `scheduled()` handler

The registered script must export a default object with a `scheduled()` method, following the [Cloudflare Workers Cron Triggers API](https://developers.cloudflare.com/workers/runtime-apis/handlers/scheduled/):

worker.ts

`async`. Bun waits for the returned promise to settle before exiting.
## How it works per platform

### Linux

Bun uses[crontab](https://man7.org/linux/man-pages/man5/crontab.5.html)to register jobs. Each job is stored as a line in your user’s crontab with a

`# bun-cron: <title>` marker comment above it.
The crontab entry looks like:
`scheduled()` handler.
**Viewing registered jobs:**

**Logs:**On Linux, cron output goes to the system log. Check with:

`scheduled()` handler.
**Manually uninstalling without code:**

### macOS

Bun uses[launchd](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLaunchdJobs.html)to register jobs. Each job is installed as a plist file at:

`StartCalendarInterval` to define the schedule. Complex patterns with ranges, lists, or steps are supported — Bun expands them into multiple `StartCalendarInterval` dicts as a Cartesian product.
**Viewing registered jobs:**

**Logs:**stdout and stderr are written to:

`weekly-report`:
**Manually uninstalling without code:**

### Windows

Bun uses[Windows Task Scheduler](https://learn.microsoft.com/en-us/windows/win32/taskschd/task-scheduler-start-page)with XML-based task definitions. Each job is registered as a scheduled task named

`bun-cron-<title>` using [elements and](https://learn.microsoft.com/en-us/windows/win32/taskschd/taskschedulerschema-calendartrigger-triggergroup-element)

`CalendarTrigger`[patterns. Most cron expressions are fully supported, including](https://learn.microsoft.com/en-us/windows/win32/taskschd/taskschedulerschema-repetition-triggerbasetype-element)

`Repetition``@daily`, `@weekly`, `@monthly`, `@yearly`, ranges (`1-5`), lists (`1,15`), named days/months, and day-of-month patterns.
#### User context

Bun registers tasks with the[logon type, which runs jobs as the registering user even when not logged in — matching Linux](https://learn.microsoft.com/en-us/windows/win32/taskschd/taskschedulerschema-logontype-simpletype)

`S4U` (Service-for-User)`crontab` behavior. No password is stored.
TCP/IP networking (`fetch()`, HTTP, WebSocket, database connections) works normally. The only restriction is that S4U tasks cannot access [Windows-authenticated network resources](https://learn.microsoft.com/en-us/windows/win32/taskschd/security-contexts-for-running-tasks)(SMB file shares, mapped drives, Kerberos/NTLM services). On headless servers and CI environments where the current user’s

[Security Identifier (SID)](https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers)cannot be resolved — such as service accounts created by

[NSSM](https://nssm.cc/)or similar tools —

`Bun.cron()` fails with an error explaining the issue. To work around this, either run Bun as a regular user account, or create the scheduled task manually with `schtasks /create /xml <file> /tn <name> /ru SYSTEM /f`.
#### Trigger limit

**Expressions that work on all platforms:**

| Pattern | Trigger strategy | Count | 
|---|---|---|
| `*/5 * * * *` | Single trigger with (PT5M)`Repetition` | 1 | 
| `*/15 * * * *` | Single trigger with Repetition (PT15M) | 1 | 
| `0 9 * * MON-FRI` | One `CalendarTrigger`per weekday | 5 | 
| `0,30 9-17 * * *` | 2 minutes × 9 hours | 18 | 
| `@daily`,`@weekly`,`@monthly`,`@yearly` | Single trigger | 1 | 

**Expressions that fail on Windows**(but work on Linux and macOS):

| Pattern | Why | Trigger count | 
|---|---|---|
| `*/7 * * * *` | 9 minute values × 24 hours | 216 | 
| `*/8 * * * *` | 8 minute values × 24 hours | 192 | 
| `*/9 * * * *` | 7 minute values × 24 hours | 168 | 
| `*/11 * * * *` | 6 minute values × 24 hours | 144 | 
| `*/13 * * * *` | 5 minute values × 24 hours | 120 | 
| `*/15 * * 6 *` | Month restriction prevents Repetition: 4 × 24 | 96 | 
| `0,30 * 15 * FRI` | OR-split doubles triggers: 2 × 24 × 2 | 96 | 

[interval (single trigger) or must expand to individual](https://learn.microsoft.com/en-us/windows/win32/taskschd/taskschedulerschema-repetition-triggerbasetype-element)

`Repetition``CalendarTrigger` elements. Minute steps that **evenly divide 60**(

`*/1`, `*/2`, `*/3`, `*/4`, `*/5`, `*/6`, `*/10`, `*/12`, `*/15`, `*/20`, `*/30`) use Repetition and work regardless of other fields. Steps that don’t divide 60 (`*/7`, `*/8`, `*/9`, `*/11`, `*/13`, etc.) must be expanded, and with 24 hours active, the count quickly exceeds 48.
To work around it, simplify the expression or restrict the hour range:
#### Windows containers

**Viewing registered jobs:**

**Manually uninstalling without code:**

**Task Scheduler**(taskschd.msc), find the task named

`bun-cron-<title>`, right-click, and delete it.
`Bun.cron.remove()`

Remove a previously registered cron job by its title. Works on all platforms.
`Bun.cron()` did:
| Platform | What `remove()`does | 
|---|---|
| Linux | Edits crontab to remove the entry and its marker comment | 
| macOS | Runs `launchctl bootout`and deletes the plist file | 
| Windows | Runs `schtasks /delete`to remove the scheduled task |

# Citations

1. Source page: https://bun.sh/docs/runtime/cron
