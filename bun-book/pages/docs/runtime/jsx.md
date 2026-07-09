---
type: Web Page
title: JSX - Bun
description: Built-in JSX and TSX support in Bun with configurable transpilation options
resource: https://bun.sh/docs/runtime/jsx
timestamp: '2026-07-09T12:17:04.216670+00:00'
---

`.jsx` and `.tsx` files. Bun’s internal transpiler converts JSX syntax into vanilla JavaScript before execution.
react.tsx

## Configuration

Bun reads your`tsconfig.json` or `jsconfig.json` to determine how to perform the JSX transform internally. If you’d rather not use either, you can set the same options in [. Bun respects the following compiler options.](/docs/runtime/bunfig)

`bunfig.toml``jsx`

How JSX constructs are transformed into vanilla JavaScript internally. The following table lists the possible values of `jsx``jsx`, along with how each transpiles this JSX component:
| Compiler options | Transpiled output | 
|---|---|
| `json<br/>{<br/>  "jsx": "react"<br/>}<br/>` | `tsx<br/>React.createElement(Box, { width: 5 }, "Hello");<br/>` | 
| `json<br/>{<br/>  "jsx": "react-jsx"<br/>}<br/>` | `tsx<br/>import { jsx } from "react/jsx-runtime";<br/>jsx("Box", { width: 5 }, "Hello");<br/>` | 
| `json<br/>{<br/>  "jsx": "react-jsxdev"<br/>}<br/>` | `tsx<br/>import { jsxDEV } from "react/jsx-dev-runtime";<br/>jsxDEV(<br/>  "Box",<br/>  { width: 5, children: "Hello" },<br/>  undefined,<br/>  false,<br/>  undefined,<br/>  this,<br/>);<br/>`The `jsxDEV`variable name is a React convention. The`DEV`suffix marks code intended for development. The development version of React is slower and includes additional validity checks & debugging tools. | 
| `json<br/>{<br/>  "jsx": "preserve"<br/>}<br/>` | `tsx<br/>// JSX is not transpiled<br/>// "preserve" is not supported by Bun currently<br/><Box width={5}>Hello</Box><br/>` | 

`jsxFactory`

`jsxFactory`Only applicable when 

`jsx` is `react`.`"React.createElement"`. Set this for libraries like [Preact](https://preactjs.com/)that use a different function name (

`"h"`).
| Compiler options | Transpiled output | 
|---|---|
| `json<br/>{<br/>  "jsx": "react",<br/>  "jsxFactory": "h"<br/>}<br/>` | `tsx<br/>h(Box, { width: 5 }, "Hello");<br/>` | 

`jsxFragmentFactory`

`jsxFragmentFactory`Only applicable when 

`jsx` is `react`.[JSX fragments](https://react.dev/reference/react/Fragment)such as

`<>Hello</>`. Default value is `"React.Fragment"`.
| Compiler options | Transpiled output | 
|---|---|
| `json<br/>{<br/>  "jsx": "react",<br/>  "jsxFactory": "myjsx",<br/>  "jsxFragmentFactory": "MyFragment"<br/>}<br/>` | `tsx<br/>// input<br/><>Hello</>;<br/><br/>// output<br/>myjsx(MyFragment, null, "Hello");<br/>` | 

`jsxImportSource`

`jsxImportSource`Only applicable when 

`jsx` is `react-jsx` or `react-jsxdev`.`createElement`, `jsx`, or `jsxDEV`) is imported from. Default value is `"react"`. You’ll typically need this when using a component library like Preact.
| Compiler options | Transpiled output | 
|---|---|
| `jsonc<br/>{<br/>  "jsx": "react-jsx",<br/>  // jsxImportSource is not defined<br/>  // default to "react"<br/>}<br/>` | `tsx<br/>import { jsx } from "react/jsx-runtime";<br/>jsx("Box", { width: 5, children: "Hello" });<br/>` | 
| `jsonc<br/>{<br/>  "jsx": "react-jsx",<br/>  "jsxImportSource": "preact",<br/>}<br/>` | `tsx<br/>import { jsx } from "preact/jsx-runtime";<br/>jsx("Box", { width: 5, children: "Hello" });<br/>` | 
| `jsonc<br/>{<br/>  "jsx": "react-jsxdev",<br/>  "jsxImportSource": "preact",<br/>}<br/>` | `tsx<br/>// /jsx-runtime is automatically appended<br/>import { jsxDEV } from "preact/jsx-dev-runtime";<br/>jsxDEV(<br/>  "Box",<br/>  { width: 5, children: "Hello" },<br/>  undefined,<br/>  false,<br/>  undefined,<br/>  this,<br/>);<br/>` | 

### JSX pragma

You can set any of these values per file with a*pragma*, a comment that sets a compiler option in a particular file.

| Pragma | Equivalent config | 
|---|---|
| `ts<br/>// @jsx h<br/>` | `jsonc<br/>{<br/>  "jsxFactory": "h",<br/>}<br/>` | 
| `ts<br/>// @jsxFrag MyFragment<br/>` | `jsonc<br/>{<br/>  "jsxFragmentFactory": "MyFragment",<br/>}<br/>` | 
| `ts<br/>// @jsxImportSource preact<br/>` | `jsonc<br/>{<br/>  "jsxImportSource": "preact",<br/>}<br/>` | 

## Logging

Bun implements special logging for JSX to make debugging easier. Given the following file:index.tsx

## Prop punning

The Bun runtime also supports “prop punning” for JSX: a shorthand for assigning a variable to a prop with the same name.react.tsx

# Citations

1. Source page: https://bun.sh/docs/runtime/jsx
