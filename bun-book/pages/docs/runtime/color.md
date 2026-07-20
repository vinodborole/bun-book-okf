---
type: Web Page
title: Color - Bun
description: Format colors as CSS, ANSI, numbers, hex strings, and more
resource: https://bun.sh/docs/runtime/color
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

`Bun.color(input, outputFormat?)` uses Bun’s CSS parser to parse, normalize, and convert colors from user input to any of these output formats:
Use it to:

- Validate and normalize colors to persist in a database (`number`is the most database-friendly)
- Convert colors to different formats
- Color terminal output beyond the basic 16 colors (use `ansi`to auto-detect terminal color support, or`ansi-16`,`ansi-256`, or`ansi-16m`to target a specific color depth)
- Format colors for use in CSS injected into HTML
- Get the `r`,`g`,`b`, and`a`color components as JavaScript objects or numbers from a CSS color string

[and](https://github.com/Qix-/color)

`color`[, with full support for parsing CSS color strings and zero dependencies.](https://github.com/bgrins/TinyColor)

`tinycolor2`### Flexible input

`Bun.color` accepts any of the following:
- Standard CSS color names like `"red"`
- Numbers like `0xff0000`
- Hex strings like `"#f00"`
- RGB strings like `"rgb(255, 0, 0)"`
- RGBA strings like `"rgba(255, 0, 0, 1)"`
- HSL strings like `"hsl(0, 100%, 50%)"`
- HSLA strings like `"hsla(0, 100%, 50%, 1)"`
- RGB objects like `{ r: 255, g: 0, b: 0 }`
- RGBA objects like `{ r: 255, g: 0, b: 0, a: 1 }`
- RGB arrays like `[255, 0, 0]`
- RGBA arrays like `[255, 0, 0, 255]`
- LAB strings like `"lab(50% 50 50)"`
- … anything else that CSS can parse as a single color value

### Format colors as CSS

The`"css"` format outputs valid CSS for use in stylesheets, inline styles, CSS variables, or CSS-in-JS. It returns the most compact string representation of the color.
`Bun.color` returns `null`.
### Format colors as ANSI (for terminals)

The`"ansi"` format outputs ANSI escape codes that color text in terminals.
`"ansi"` format detects the color depth of stdout from environment variables and picks `"ansi-16m"`, `"ansi-256"`, or `"ansi-16"` accordingly. If stdout doesn’t support any form of ANSI color, it returns an empty string. As with the rest of Bun’s color API, if the input is unknown or fails to parse, it returns `null`.
#### 24-bit ANSI colors (`ansi-16m`)

The `"ansi-16m"` format outputs 24-bit ANSI colors, which can display 16 million colors but require a modern terminal that supports them.
It converts the input color to RGBA, then outputs that as an ANSI color.
#### 256 ANSI colors (`ansi-256`)

The `"ansi-256"` format approximates the input color to the nearest of the 256 ANSI colors supported by some terminals.
[.](https://github.com/tmux/tmux/blob/dae2868d1227b95fd076fb4a5efa6256c7245943/colour.c#L44-L55)

`tmux` uses#### 16 ANSI colors (`ansi-16`)

The `"ansi-16"` format approximates the input color to the nearest of the 16 ANSI colors supported by most terminals.
`ansi-256`, then to the nearest of the 16 ANSI colors.
### Format colors as numbers

The`"number"` format outputs the color as a 24-bit number, a compact representation for databases and configuration.
### Get the red, green, blue, and alpha channels

The`"{rgba}"`, `"{rgb}"`, `"[rgba]"`, and `"[rgb]"` formats return the red, green, blue, and alpha channels as objects or arrays.
`{rgba}` object

The `"{rgba}"` format outputs an object with the red, green, blue, and alpha channels.
`a` channel is a decimal number between `0` and `1`.
The `"{rgb}"` format is similar, but it doesn’t include the alpha channel.
`[rgba]` array

The `"[rgba]"` format outputs an array with the red, green, blue, and alpha channels.
`"{rgba}"` format, the alpha channel is an integer between `0` and `255`. This is useful for typed arrays where each channel must be the same underlying type.
The `"[rgb]"` format is similar, but it doesn’t include the alpha channel.
### Format colors as hex strings

The`"hex"` format outputs a lowercase hex string.
`"HEX"` format is the same, but uppercase.
### Bundle-time client-side color formatting

Like many of Bun’s APIs, you can invoke`Bun.color` at bundle time with a [macro](/docs/bundler/macros)for use in client-side JavaScript builds:

client-side.ts

`bun build` writes the following to `client-side.js`:

# Citations

1. Source page: https://bun.sh/docs/runtime/color
