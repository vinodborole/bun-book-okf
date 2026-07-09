---
type: Web Page
title: Markdown - Bun
description: Parse and render Markdown with Bun's built-in Markdown API, supporting
  GFM extensions and custom rendering callbacks
resource: https://bun.sh/docs/runtime/markdown
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

**Unstable API**— This API is under active development and may change in future versions of Bun.

- `Bun.markdown.html()`— render Markdown to an HTML string
- `Bun.markdown.render()`— render Markdown with custom callbacks for each element
- `Bun.markdown.react()`— render Markdown to React JSX elements

`Bun.markdown.html()`

Convert a Markdown string to HTML.
### Options

Pass an options object as the second argument to configure the parser:| Option | Default | Description | 
|---|---|---|
| `tables` | `true` | GFM tables | 
| `strikethrough` | `true` | GFM strikethrough ( `~~text~~`) | 
| `tasklists` | `true` | GFM task lists ( `- [x] item`) | 
| `autolinks` | `false` | Enable autolinks — see [Autolinks](#autolinks) | 
| `headings` | `false` | Heading IDs and autolinks — see [Heading IDs](#heading-ids) | 
| `hardSoftBreaks` | `false` | Treat soft line breaks as hard breaks | 
| `wikiLinks` | `false` | Enable `[[wiki links]]` | 
| `underline` | `false` | `__text__`renders as`<u>`instead of`<strong>` | 
| `latexMath` | `false` | Enable `$inline$`and`$$display$$`math | 
| `collapseWhitespace` | `false` | Collapse whitespace in text | 
| `permissiveAtxHeaders` | `false` | ATX headers without space after `#` | 
| `noIndentedCodeBlocks` | `false` | Disable indented code blocks | 
| `noHtmlBlocks` | `false` | Disable HTML blocks | 
| `noHtmlSpans` | `false` | Disable inline HTML | 
| `tagFilter` | `false` | GFM tag filter for disallowed HTML tags | 

#### Autolinks

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

| Callback | Meta | Description | 
|---|---|---|
| `heading` | `{ level, id? }` | Heading level 1–6. `id`is set when`headings: { ids: true }`is enabled | 
| `paragraph` | — | Paragraph block | 
| `blockquote` | — | Blockquote block | 
| `code` | `{ language? }` | Fenced or indented code block. `language`is the info-string when specified on the fence | 
| `list` | `{ ordered, start?, depth }` | `depth`is nesting level (0 = top-level).`start`is set for ordered lists | 
| `listItem` | `{ index, depth, ordered, start?, checked? }` | See [List item meta](#list-item-meta)below | 
| `hr` | — | Horizontal rule | 
| `table` | — | Table block | 
| `thead` | — | Table head | 
| `tbody` | — | Table body | 
| `tr` | — | Table row | 
| `th` | `{ align? }` | Table header cell. `align`is`"left"`,`"center"`,`"right"`, or absent | 
| `td` | `{ align? }` | Table data cell | 
| `html` | — | Raw HTML content | 

#### List item meta

The`listItem` callback receives everything needed to render markers directly:
- `index`— 0-based position within the parent list
- `depth`— the parent list’s nesting level (0 = top-level)
- `ordered`— whether the parent list is ordered
- `start`— the parent list’s start number (only when- `ordered`is true)
- `checked`— task list state (only for- `- [x]`/- `- [ ]`items)

### Inline callbacks

| Callback | Meta | Description | 
|---|---|---|
| `strong` | — | Strong emphasis ( `**text**`) | 
| `emphasis` | — | Emphasis ( `*text*`) | 
| `link` | `{ href: string, title?: string }` | Link | 
| `image` | `{ src: string, title?: string }` | Image | 
| `codespan` | — | Inline code ( ``code``) | 
| `strikethrough` | — | Strikethrough ( `~~text~~`) | 
| `text` | — | Plain text content | 

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

Every HTML tag produced by the parser can be overridden:| Option | Props | Description | 
|---|---|---|
| `h1`–`h6` | `{ id?, children }` | Headings. `id`is set when`headings: { ids: true }`is enabled | 
| `p` | `{ children }` | Paragraph | 
| `blockquote` | `{ children }` | Blockquote | 
| `pre` | `{ language?, children }` | Code block. `language`is the info string (e.g.`"js"`) | 
| `hr` | `{}` | Horizontal rule (no children) | 
| `ul` | `{ children }` | Unordered list | 
| `ol` | `{ start, children }` | Ordered list. `start`is the first item number | 
| `li` | `{ checked?, children }` | List item. `checked`is set for task list items | 
| `table` | `{ children }` | Table | 
| `thead` | `{ children }` | Table head | 
| `tbody` | `{ children }` | Table body | 
| `tr` | `{ children }` | Table row | 
| `th` | `{ align?, children }` | Table header cell | 
| `td` | `{ align?, children }` | Table data cell | 
| `em` | `{ children }` | Emphasis ( `*text*`) | 
| `strong` | `{ children }` | Strong ( `**text**`) | 
| `a` | `{ href, title?, children }` | Link | 
| `img` | `{ src, alt?, title? }` | Image (no children) | 
| `code` | `{ children }` | Inline code | 
| `del` | `{ children }` | Strikethrough ( `~~text~~`) | 
| `br` | `{}` | Hard line break (no children) | 

### React 18 and older

By default, elements use`Symbol.for('react.transitional.element')` as the `$$typeof` symbol. For React 18 and older, pass `reactVersion: 18` in the options (third argument):
### Parser options

Pass any of the[parser options](#options)as the third argument:

# Citations

1. Source page: https://bun.sh/docs/runtime/markdown
