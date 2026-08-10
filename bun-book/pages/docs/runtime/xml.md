---
type: Web Page
title: XML - Bun
description: Use Bun's built-in support for XML through both runtime APIs and bundler
  integration
resource: https://bun.sh/docs/runtime/xml
timestamp: '2026-08-10T07:07:25.236908+00:00'
---

New in Bun v1.4

- Parse and stringify XML with `Bun.XML.parse` and`Bun.XML.stringify`
- `import` &`require` XML files as modules at runtime (including hot reloading & watch mode support)
- `import` &`require` XML files in frontend apps with Bun’s bundler

## Runtime API

### `Bun.XML.parse()`

Parse an XML document into a plain JavaScript object.
**compact object**keyed by element name — the shape most XML-to-object libraries use:

- The result has one key, the root element’s name.
- An element with no attributes and no child elements becomes its text content, trimmed of surrounding whitespace (`""` when empty).
- Any other element becomes an object with a `"@name"` key per attribute, one key per distinct child element name — an**array** when that name repeats, in document order — and`"#text"` for its trimmed character data, if any.
- CDATA sections and entity references are already expanded into the text. Comments and processing instructions are dropped.
- All values are strings. Nothing is coerced to numbers, booleans, or `null` .

`{ compact: false }` to get the root element as a **node tree**that keeps everything in document order:

`{ name, attributes, children }`; `children` holds child elements and strings, and text is passed through exactly (including whitespace-only runs between elements).
#### Input types and encodings

`XML.parse` accepts a string, or bytes as a `Buffer`, `TypedArray`, `ArrayBuffer`, or `Blob`.
A string is already-decoded text, so its `encoding` declaration is checked for syntax but otherwise ignored. Bytes are decoded per the XML rules: a byte-order mark or the `encoding` in `<?xml version="1.0" encoding="..."?>` selects **UTF-8**(the default),

**UTF-16**(either byte order), or

**ISO-8859-1**. Other encodings throw.

#### Error handling

`Bun.XML.parse()` throws a `SyntaxError` when the document is not well-formed:
### `Bun.XML.stringify()`

Serialize either shape back to XML. The output has no XML declaration and is always well-formed: `&`, `<`, `>` (and, in attributes, quotes, tabs and newlines) are escaped, and element or attribute names that are not XML names throw.
`name` and a `children` or `attributes` property is written as a node; anything else is a compact object and must have exactly one key naming the root element. Strings, numbers, booleans, bigints and `Date`s (as ISO strings) become text, `null` becomes an empty element, and `undefined`, functions and symbols are skipped like `JSON.stringify` skips them (unlike `JSON.stringify`, a bigint is written as its decimal digits rather than rejected).
#### Pretty printing

Pass a`space` argument (a number of spaces or an indent string, as with `JSON.stringify`) to indent element-only content. Elements that contain text are written inline so character data is unchanged:
`XML.parse(XML.stringify(value))` gives back `value` for anything `XML.parse` produced, in either shape.
## Module Import

### ES Modules

You can import XML files directly. Files are decoded like bytes passed to`XML.parse` (UTF-8, UTF-16, or ISO-8859-1 per the byte-order mark or declaration), and the module’s value is the compact object described above:
config.xml

#### Default Import

app.ts

#### Named Import

The root element is also available as a named import:
app.ts

### CommonJS

app.ts

### Import Attributes

Use`with { type: "xml" }` to parse a file with another extension as XML:
## Hot Reloading with XML

When you run your application with`bun --hot`, Bun reloads XML files when they change:
server.ts

terminal

## Bundler Integration

When you bundle with Bun, imported XML files are parsed at build time and inlined as JavaScript objects:
terminal

- Zero runtime XML parsing overhead in production
- Smaller bundle sizes
- Tree shaking of unused properties

### Dynamic Imports

XML files can be dynamically imported:
## Conformance

Bun’s XML parser is written in Rust and implements
[XML 1.0 (Fifth Edition)](https://www.w3.org/TR/2008/REC-xml-20081126/)as a

**non-validating processor that does not read external entities**:

- The whole document, including the internal DTD subset, must be well-formed — anything else throws a `SyntaxError` .
- Internal entities declared in the document are expanded (with expansion limits, so “billion laughs” payloads fail instead of exhausting memory), attribute values are normalized, and attribute defaults declared in the internal subset are applied.
- External DTDs and external entities are never fetched or read, so there is no XXE surface. In a document with no DTD, a reference to an undeclared entity is an error; when the DOCTYPE points at an external subset (or uses parameter entities) that could have declared it, the reference is kept as written (` ` stays` ` ), unless the document says`standalone="yes"` .
- Nothing is validated against the DTD, namespaces are not resolved (prefixed names are kept verbatim), and comments and processing instructions are skipped.

[W3C XML Conformance Test Suite](https://www.w3.org/XML/Test/): all 1,679 cases that have a required outcome for this class of processor pass — not-well-formed documents are rejected, well-formed ones are accepted and, where the suite gives one, their element tree matches its canonical output byte for byte. The

[translated test suite](https://github.com/oven-sh/bun/blob/main/test/js/bun/xml/xml-test-suite.test.ts)lists every case, including the ones whose outcome legitimately depends on not reading external entities.

## Performance

The parser works in two stages, like Bun’s JSON parser: a SIMD pass (runtime-dispatched AVX2/AVX-512/NEON/SVE kernels) finds the bytes that can change the parse, so character data, attribute values, comments and CDATA sections are never scanned a byte at a time, and element and attribute names reuse JavaScriptCore’s atom-string cache the same way`JSON.parse` does.
[compares](https://github.com/oven-sh/bun/blob/main/bench/xml/xml.mjs)

`bench/xml/xml.mjs``Bun.XML.parse` with popular npm parsers on the same documents (lower is better; Linux x64, one core):

# Citations

1. Source page: https://bun.sh/docs/runtime/xml
