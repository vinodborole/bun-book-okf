---
type: Web Page
title: Markdown - Bun
description: Parse and render Markdown with Bun's built-in Markdown API, supporting
  GFM extensions and custom rendering callbacks
resource: https://bun.sh/docs/runtime/markdown
timestamp: '2026-07-20T08:37:03.598151+00:00'
---

**Unstable API**— This API is under active development and may change in future versions of Bun.

- `Bun.markdown.html()`— render Markdown to an HTML string
- `Bun.markdown.render()`— render Markdown with custom callbacks for each element
- `Bun.markdown.react()`— render Markdown to React JSX elements

`Bun.markdown.html()`

Convert a Markdown string to HTML.
### Options

Pass an options object as the second argument to configure the parser:#### Autolinks

Pass`true` to enable all autolink types, or an object for granular control:
#### Heading IDs

Pass`true` to enable both heading IDs and autolink headings, or an object for granular control:
`Bun.markdown.render()`

Parse Markdown and render it using custom JavaScript callbacks. This gives you full control over the output format — you can generate HTML with custom classes, React elements, ANSI terminal output, or any other string format.
### Callback signature

Each callback receives:- `children`
- `meta`

`null` or `undefined` to omit the element from the output entirely. If no callback is registered for an element, its children pass through unchanged.
### Block callbacks

#### List item meta

The`listItem` callback receives everything needed to render markers directly:
- `index`— 0-based position within the parent list
- `depth`— the parent list’s nesting level (0 = top-level)
- `ordered`— whether the parent list is ordered
- `start`— the parent list’s start number (only when- `ordered`is true)
- `checked`— task list state (only for- `- [x]`/- `- [ ]`items)

### Inline callbacks

### Examples

#### Custom HTML with classes

#### Stripping all formatting

#### Omitting elements

Return`null` or `undefined` to remove an element from the output:
#### ANSI terminal output

#### Nested list numbering

The`listItem` callback receives everything needed to render markers directly — no post-processing:
#### Code block syntax highlighting

### Parser options

Pass parser options as a separate third argument:`Bun.markdown.react()`

Render Markdown directly to React elements. Returns a `<Fragment>` that you can use as a component return value.
### Server-side rendering

Works with`renderToString()` and React Server Components:
### Component overrides

Replace any HTML element with a custom React component by passing it in the second argument, keyed by tag name:#### Available overrides

Every HTML tag produced by the parser can be overridden:### React 18 and older

By default, elements use`Symbol.for('react.transitional.element')` as the `$$typeof` symbol. For React 18 and older, pass `reactVersion: 18` in the options (third argument):
### Parser options

Pass any of the[parser options](#options)as the third argument:

# Citations

1. Source page: https://bun.sh/docs/runtime/markdown
