---
type: Web Page
title: Shell - Bun
description: Use Bun's shell scripting API to run shell commands from JavaScript
resource: https://bun.sh/docs/runtime/shell
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

index.ts

## Features

- **Cross-platform**: works on Windows, Linux & macOS. Instead of installing- `rimraf`or- `cross-env`, you can use Bun Shell. Common shell commands like- `ls`,- `cd`, and- `rm`are implemented natively.
- **Familiar**: Bun Shell is a bash-like shell that supports redirection, pipes, and environment variables.
- **Globs**: Glob patterns are supported natively, including- `**`,- `*`, and- `{expansion}`.
- **Template literals**: Template literals execute shell commands and interpolate variables and expressions.
- **Safety**: Bun Shell escapes all strings by default, preventing shell injection attacks.
- **JavaScript interop**: Use- `Response`,- `ArrayBuffer`,- `Blob`,- `Bun.file(path)`and other JavaScript objects as stdin, stdout, and stderr.
- **Shell scripting**: Bun Shell runs shell scripts (- `.bun.sh`files).
- **Custom interpreter**: Bun Shell is a small programming language with its own lexer, parser, and interpreter, written in Rust.

## Getting started

The simplest shell command is`echo`. To run it, use the `$` template literal tag:
`.quiet()`:
`.text()`:
`await`ing returns stdout and stderr as `Buffer`s.
## Error handling

By default, a non-zero exit code throws an error. The`ShellError` contains information about the command that ran.
`.nothrow()` disables throwing. Check the result’s `exitCode` yourself.
`.nothrow()` or `.throws(boolean)` on the `$` function itself.
## Redirection

Redirect a command’s*input*or

*output*with the typical Bash operators:

- `<`redirect stdin
- `>`or- `1>`redirect stdout
- `2>`redirect stderr
- `&>`redirect both stdout and stderr
- `>>`or- `1>>`redirect stdout,- *appending*to the destination, instead of overwriting
- `2>>`redirect stderr,- *appending*to the destination, instead of overwriting
- `&>>`redirect both stdout and stderr,- *appending*to the destination, instead of overwriting
- `1>&2`redirect stdout to stderr (writes to stdout go to stderr instead)
- `2>&1`redirect stderr to stdout (writes to stderr go to stdout instead)

### Example: Redirect output to JavaScript objects (`>`)

To redirect stdout to a JavaScript object, use the `>` operator:
- `Buffer`,- `Uint8Array`,- `Uint16Array`,- `Uint32Array`,- `Int8Array`,- `Int16Array`,- `Int32Array`,- `Float32Array`,- `Float64Array`,- `ArrayBuffer`,- `SharedArrayBuffer`(writes to the underlying buffer)
- `Bun.file(path)`,- `Bun.file(fd)`(writes to the file)

### Example: Redirect input from JavaScript objects (`<`)

To use a JavaScript object as stdin, use the `<` operator:
- `Buffer`,- `Uint8Array`,- `Uint16Array`,- `Uint32Array`,- `Int8Array`,- `Int16Array`,- `Int32Array`,- `Float32Array`,- `Float64Array`,- `ArrayBuffer`,- `SharedArrayBuffer`(reads from the underlying buffer)
- `Bun.file(path)`,- `Bun.file(fd)`(reads from the file)
- `Response`(reads from the body)

### Example: Redirect stdin -> file

### Example: Redirect stdout -> file

### Example: Redirect stderr -> file

### Example: Redirect stderr -> stdout

### Example: Redirect stdout -> stderr

## Piping (`|`)

Like in bash, you can pipe the output of one command to another:
## Command substitution (`$(...)`)

Command substitution inserts the output of another command into the current script:
Because Bun internally uses the special Instead of printing:It prints:Use the 

[property on the input template literal, using the backtick syntax for command substitution won’t work:](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#raw_strings)`raw``$(...)` syntax instead.## Environment variables

Set environment variables like in bash:### Changing the environment variables

By default, all commands use`process.env` as their environment variables.
To change the environment variables for a single command, call `.env()`:
`$.env`:
`$.env()` with no arguments:
### Changing the working directory

To change the working directory of a command, pass a string to`.cwd()`:
`$.cwd`:
## Reading output

To read the output of a command as a string, use`.text()`:
### Reading output as JSON

To read the output of a command as JSON, use`.json()`:
### Reading output line-by-line

To read the output of a command line-by-line, use`.lines()`:
`.lines()` on a completed command:
### Reading output as a Blob

To read the output of a command as a Blob, use`.blob()`:
## Builtin Commands

For cross-platform compatibility, Bun Shell implements a set of builtin commands, in addition to reading commands from the`PATH` environment variable.
- `cd`: change the working directory
- `ls`: list files in a directory (supports- `-l`for long listing format)
- `rm`: remove files and directories
- `echo`: print text
- `pwd`: print the working directory
- `bun`: run bun in bun
- `cat`
- `touch`
- `mkdir`
- `which`
- `mv`
- `exit`
- `true`
- `false`
- `yes`
- `seq`
- `dirname`
- `basename`

**Partially**implemented:

- `mv`: move files and directories (missing cross-device support)

**Not**implemented yet, but planned:

- See [Issue #9716](https://github.com/oven-sh/bun/issues/9716)for the full list.

## Utilities

Bun Shell also implements a set of utilities for working with shells.`$.braces` (brace expansion)

`$.braces` implements [brace expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html)for shell commands:

`$.escape` (escape strings)

Exposes Bun Shell’s escaping logic as a function:
`{ raw: 'str' }` object:
`.sh` file loader

For simple shell scripts, you can use Bun Shell instead of `/bin/sh`. Pass a file with the `.sh` extension to `bun`:
script.sh

terminal

powershell

## Implementation notes

Bun Shell is a small programming language implemented in Rust, with a handwritten lexer, parser, and interpreter. Unlike bash, zsh, and other shells, Bun Shell runs operations concurrently.## Security in the Bun shell

By design, Bun Shell*does not invoke a system shell*like

`/bin/sh`. It’s a
re-implementation of bash that runs in the same Bun process.
When parsing command arguments, it treats all *interpolated variables*as single, literal strings. This protects against

**command injection**:

`userInput` is treated as a single string, so `ls` tries to read the
contents of a single directory named `my-file.txt; rm -rf /`.
### Security considerations

While command injection is prevented by default, you are still responsible for security in certain scenarios. Similar to the`Bun.spawn` or `node:child_process.exec()` APIs, you can intentionally
execute a command which spawns a new shell (for example, `bash -c`) with arguments.
When you do this, you hand off control, and Bun’s built-in protections no
longer apply to the string interpreted by that new shell.
### Argument injection

Bun Shell cannot know how an external command interprets its own command-line arguments. An attacker can supply input that the target program recognizes as one of its own options or flags, leading to unintended behavior.**Recommendation**: Always sanitize user-provided input before passing it as an argument to an external command. Validating arguments is your application’s responsibility.

## Credits

Large parts of this API were inspired by[zx](https://github.com/google/zx),

[dax](https://github.com/dsherret/dax), and

[bnx](https://github.com/wobsoriano/bnx). Thank you to the authors of those projects.

# Citations

1. Source page: https://bun.sh/docs/runtime/shell
