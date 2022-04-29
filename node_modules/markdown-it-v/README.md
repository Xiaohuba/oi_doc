# markdown⁠-⁠it⁠-⁠v
A custom markdown⁠-⁠it renderer that outputs virtual DOM.

![build](https://img.shields.io/travis/TitanSnow/markdown-it-v.svg?style=for-the-badge)
![coverage](https://img.shields.io/codecov/c/github/TitanSnow/markdown-it-v.svg?style=for-the-badge)
![version](https://img.shields.io/npm/v/markdown-it-v.svg?style=for-the-badge)
![license](https://img.shields.io/npm/l/markdown-it-v.svg?style=for-the-badge)

## Motivation

### Why prefer virtual DOM to `innerHTML`?
- Better integration with modern JavaScript frameworks like [Vue](https://vuejs.org) and [React](https://reactjs.org).
- Better performance for real-time preview of large Markdown document. Thanks to the diff algorithm of virtual DOM, the real DOM modification can be minimized.

### Why markdown⁠-⁠it⁠-⁠v
- markdown⁠-⁠it itself has great performance.
- markdown⁠-⁠it⁠-⁠v is a markdown⁠-⁠it plugin and can be integrated seamlessly.
- markdown⁠-⁠it⁠-⁠v supports four schemes of output:

  - Vue virtual DOM
  - React virtual DOM
  - Browser’s real DOM
  - HTML string

## Installation
```console
$ npm install markdown-it markdown-it-v --save
```
Or yarn:
```console
$ yarn add markdown-it markdown-it-v
```

## Usage

### Setup
markdown⁠-⁠it⁠-⁠v is a plugin of markdown⁠-⁠it:
```javascript
const MarkdownIt = require('markdown-it')
const MarkdownItV = require('markdown-it-v')

const md = MarkdownIt()
md.use(MarkdownItV)
```

### Render
After setup, the `render()` method will return a `StreamDom` object — a kind of virtual DOM implemented by markdown⁠-⁠it⁠-⁠v itself:
```javascript
let sdom = md.render('The *quick* brown fox _jumps_ over the **lazy** dog.')
```

### Convert
Unfortunately you cannot use `StreamDom` in other places and it doesn’t implement a diff algorithm. You must convert it to final output:
```javascript
let vueVDom   = sdom.toVue(vueVm.$createElement)    // `vueVm` is a Vue instance
let reactVDom = sdom.toReact(React.createElement)
let realDom   = sdom.toNative(document)             // not `document.createElement`!
let htmlStr   = sdom.toHTML()
```

### Integrate with JS Frameworks
Vue component (without JSX):
```javascript
{   // in a Vue component
    render(h) {
        return h('div', null, sdom.toVue(h))
    }
}
```

React component (with JSX):
```jsx
class Markdown extends React.Component {
    // in a React component
    render() {
        const h = React.createElement
        return <div>{sdom.toReact(h)}</div>
    }
}
```

## Changelog

- 1.2.0
  - Drop css-tree

- 1.1.1
  - Add ES6 module output

- 1.1.0
  - Adds highlightNoWrappingEls (#1) by @laosb

- 1.0.0-beta.1
  - No change

- 1.0.0-alpha.2
  - Use `_.fromPairs` in lodash

- 1.0.0-alpha.1
  - Initial release
