# refractor [![Build Status][travis-badge]][travis] [![Coverage Status][codecov-badge]][codecov]

Lightweight, robust, elegant virtual syntax highlighting using [Prism][].
Useful for virtual DOMs and non-HTML things.  Perfect for [React][], [VDOM][],
and others.

`refractor` is built to work with all syntaxes supported by [Prism][],
that’s [120 languages][names] (as of [prism@1.6.0][prismjs]) (and all
7 [themes][themes]).

Want to use [`highlight.js`][hljs] instead?  Try [`lowlight`][lowlight]!

## Table of Contents

*   [Installation](#installation)
*   [Usage](#usage)
*   [API](#api)
    *   [refractor.register(syntax)](#refractorregistersyntax)
    *   [refractor.highlight(value, language)](#refractorhighlightvalue-language)
*   [Browser](#browser)
*   [Plugins](#plugins)
*   [Syntaxes](#syntaxes)
*   [Related](#related)
*   [License](#license)

## Installation

[npm][]:

```bash
npm install
```

[Usage in the browser »][browser]

## Usage

```javascript
var refractor = require('refractor');

var result = refractor.highlight('"use strict";', 'js');
console.dir(result, {depth: null});
```

Yields:

```js
[ { type: 'element',
    tagName: 'span',
    properties: { className: [ 'token', 'string' ] },
    children: [ { type: 'text', value: '"use strict"' } ] },
  { type: 'element',
    tagName: 'span',
    properties: { className: [ 'token', 'punctuation' ] },
    children: [ { type: 'text', value: ';' } ] } ]
```

Or, stringified with [rehype][]:

```js
var rehype = require('rehype');
var html = rehype().stringify({type: 'root', children: result}).toString();

console.log(html);
```

Yields:

```html
<span class="token string">"use strict"</span><span class="token punctuation">;</span>
```

> **Tip**: Use [`hast-to-hyperscript`][to-hyperscript] to transform
> to other virtual DOMs, or DIY.

## API

### `refractor.register(syntax)`

Register a [syntax][].  Needed in the browser.

### `refractor.highlight(value, language)`

Parse `value` (`string`) according to the [`language` (name or alias)][syntax]
grammar.

###### Returns

Virtual nodes representing the highlighted value ([`Array.<Node>`][node]).

## Browser

I do not suggest using the pre-built files or requiring `refractor` in
the browser as that would include a 250kb (90kb GZipped) file.

Instead, require `refractor/core.js`, and include only the used
highlighters.  For example:

```js
var refractor = require('refractor/core.js');
var jsx = require('refractor/lang/jsx.js');

refractor.register(jsx);

var result = refractor.highlight('<Dropdown primary />', 'jsx');
console.dir(result, {depth: null});
```

Yields:

```js
[ { type: 'element',
    tagName: 'span',
    properties: { className: [ 'token', 'tag' ] },
    children:
     [ { type: 'element',
         tagName: 'span',
         properties: { className: [ 'token', 'tag' ] },
         children:
          [ { type: 'element',
              tagName: 'span',
              properties: { className: [ 'token', 'punctuation' ] },
              children: [ { type: 'text', value: '<' } ] },
            { type: 'text', value: 'Dropdown' } ] },
       { type: 'text', value: ' ' },
       { type: 'element',
         tagName: 'span',
         properties: { className: [ 'token', 'attr-name' ] },
         children: [ { type: 'text', value: 'primary' } ] },
       { type: 'text', value: ' ' },
       { type: 'element',
         tagName: 'span',
         properties: { className: [ 'token', 'punctuation' ] },
         children: [ { type: 'text', value: '/>' } ] } ] } ]
```

…When using browserify and minifying the result, this results in just 19kb of
code (7.62kb with GZip).

## Plugins

`refractor` does not support Prism plugins.  Prism is built using global
variables instead of a module format.  All the syntaxes below are custom made
to work with CommonJS so you can `require` just what you want to make things
work.  Additionally, Prism plugins often deal with the real DOM instead of the
tokens created by Prism, making them hard to get to work in virtual DOMs.

## Syntaxes

All syntaxes are included if you `require('refractor')`.  Checked items are
always included in the core (`core.js`), but unchecked items are not and must
be `require`d and [`register`][register]ed if `core.js` is used.

Unlike in Prism, `cssExtras` and `phpExtras` are camel-cased instead of
dash-cased.

Prism syntaxes are built to work with global variables and without module
format.  All below syntaxes are custom made to work with CommonJS.  Only
these custom built syntaxes will work with `refractor`.

<!--support start-->

*   [x] [`clike`](https://github.com/wooorm/refractor/blob/master/lang/clike.js)
*   [x] [`css`](https://github.com/wooorm/refractor/blob/master/lang/css.js)
*   [x] [`javascript`](https://github.com/wooorm/refractor/blob/master/lang/javascript.js) — alias: `js`
*   [x] [`markup`](https://github.com/wooorm/refractor/blob/master/lang/markup.js) — alias: `xml`, `html`, `mathml`, `svg`
*   [ ] [`abap`](https://github.com/wooorm/refractor/blob/master/lang/abap.js)
*   [ ] [`actionscript`](https://github.com/wooorm/refractor/blob/master/lang/actionscript.js)
*   [ ] [`ada`](https://github.com/wooorm/refractor/blob/master/lang/ada.js)
*   [ ] [`apacheconf`](https://github.com/wooorm/refractor/blob/master/lang/apacheconf.js)
*   [ ] [`apl`](https://github.com/wooorm/refractor/blob/master/lang/apl.js)
*   [ ] [`applescript`](https://github.com/wooorm/refractor/blob/master/lang/applescript.js)
*   [ ] [`asciidoc`](https://github.com/wooorm/refractor/blob/master/lang/asciidoc.js)
*   [ ] [`aspnet`](https://github.com/wooorm/refractor/blob/master/lang/aspnet.js)
*   [ ] [`autohotkey`](https://github.com/wooorm/refractor/blob/master/lang/autohotkey.js)
*   [ ] [`autoit`](https://github.com/wooorm/refractor/blob/master/lang/autoit.js)
*   [ ] [`bash`](https://github.com/wooorm/refractor/blob/master/lang/bash.js)
*   [ ] [`basic`](https://github.com/wooorm/refractor/blob/master/lang/basic.js)
*   [ ] [`batch`](https://github.com/wooorm/refractor/blob/master/lang/batch.js)
*   [ ] [`bison`](https://github.com/wooorm/refractor/blob/master/lang/bison.js)
*   [ ] [`brainfuck`](https://github.com/wooorm/refractor/blob/master/lang/brainfuck.js)
*   [ ] [`bro`](https://github.com/wooorm/refractor/blob/master/lang/bro.js)
*   [ ] [`c`](https://github.com/wooorm/refractor/blob/master/lang/c.js)
*   [ ] [`coffeescript`](https://github.com/wooorm/refractor/blob/master/lang/coffeescript.js)
*   [ ] [`cpp`](https://github.com/wooorm/refractor/blob/master/lang/cpp.js)
*   [ ] [`crystal`](https://github.com/wooorm/refractor/blob/master/lang/crystal.js)
*   [ ] [`csharp`](https://github.com/wooorm/refractor/blob/master/lang/csharp.js)
*   [ ] [`cssExtras`](https://github.com/wooorm/refractor/blob/master/lang/css-extras.js)
*   [ ] [`d`](https://github.com/wooorm/refractor/blob/master/lang/d.js)
*   [ ] [`dart`](https://github.com/wooorm/refractor/blob/master/lang/dart.js)
*   [ ] [`diff`](https://github.com/wooorm/refractor/blob/master/lang/diff.js)
*   [ ] [`docker`](https://github.com/wooorm/refractor/blob/master/lang/docker.js)
*   [ ] [`eiffel`](https://github.com/wooorm/refractor/blob/master/lang/eiffel.js)
*   [ ] [`elixir`](https://github.com/wooorm/refractor/blob/master/lang/elixir.js)
*   [ ] [`erlang`](https://github.com/wooorm/refractor/blob/master/lang/erlang.js)
*   [ ] [`fortran`](https://github.com/wooorm/refractor/blob/master/lang/fortran.js)
*   [ ] [`fsharp`](https://github.com/wooorm/refractor/blob/master/lang/fsharp.js)
*   [ ] [`gherkin`](https://github.com/wooorm/refractor/blob/master/lang/gherkin.js)
*   [ ] [`git`](https://github.com/wooorm/refractor/blob/master/lang/git.js)
*   [ ] [`glsl`](https://github.com/wooorm/refractor/blob/master/lang/glsl.js)
*   [ ] [`go`](https://github.com/wooorm/refractor/blob/master/lang/go.js)
*   [ ] [`graphql`](https://github.com/wooorm/refractor/blob/master/lang/graphql.js)
*   [ ] [`groovy`](https://github.com/wooorm/refractor/blob/master/lang/groovy.js)
*   [ ] [`haml`](https://github.com/wooorm/refractor/blob/master/lang/haml.js)
*   [ ] [`handlebars`](https://github.com/wooorm/refractor/blob/master/lang/handlebars.js)
*   [ ] [`haskell`](https://github.com/wooorm/refractor/blob/master/lang/haskell.js)
*   [ ] [`haxe`](https://github.com/wooorm/refractor/blob/master/lang/haxe.js)
*   [ ] [`http`](https://github.com/wooorm/refractor/blob/master/lang/http.js)
*   [ ] [`icon`](https://github.com/wooorm/refractor/blob/master/lang/icon.js)
*   [ ] [`inform7`](https://github.com/wooorm/refractor/blob/master/lang/inform7.js)
*   [ ] [`ini`](https://github.com/wooorm/refractor/blob/master/lang/ini.js)
*   [ ] [`j`](https://github.com/wooorm/refractor/blob/master/lang/j.js)
*   [ ] [`jade`](https://github.com/wooorm/refractor/blob/master/lang/jade.js)
*   [ ] [`java`](https://github.com/wooorm/refractor/blob/master/lang/java.js)
*   [ ] [`jolie`](https://github.com/wooorm/refractor/blob/master/lang/jolie.js)
*   [ ] [`json`](https://github.com/wooorm/refractor/blob/master/lang/json.js) — alias: `jsonp`
*   [ ] [`jsx`](https://github.com/wooorm/refractor/blob/master/lang/jsx.js)
*   [ ] [`julia`](https://github.com/wooorm/refractor/blob/master/lang/julia.js)
*   [ ] [`keyman`](https://github.com/wooorm/refractor/blob/master/lang/keyman.js)
*   [ ] [`kotlin`](https://github.com/wooorm/refractor/blob/master/lang/kotlin.js)
*   [ ] [`latex`](https://github.com/wooorm/refractor/blob/master/lang/latex.js)
*   [ ] [`less`](https://github.com/wooorm/refractor/blob/master/lang/less.js)
*   [ ] [`livescript`](https://github.com/wooorm/refractor/blob/master/lang/livescript.js)
*   [ ] [`lolcode`](https://github.com/wooorm/refractor/blob/master/lang/lolcode.js)
*   [ ] [`lua`](https://github.com/wooorm/refractor/blob/master/lang/lua.js)
*   [ ] [`makefile`](https://github.com/wooorm/refractor/blob/master/lang/makefile.js)
*   [ ] [`markdown`](https://github.com/wooorm/refractor/blob/master/lang/markdown.js)
*   [ ] [`matlab`](https://github.com/wooorm/refractor/blob/master/lang/matlab.js)
*   [ ] [`mel`](https://github.com/wooorm/refractor/blob/master/lang/mel.js)
*   [ ] [`mizar`](https://github.com/wooorm/refractor/blob/master/lang/mizar.js)
*   [ ] [`monkey`](https://github.com/wooorm/refractor/blob/master/lang/monkey.js)
*   [ ] [`nasm`](https://github.com/wooorm/refractor/blob/master/lang/nasm.js)
*   [ ] [`nginx`](https://github.com/wooorm/refractor/blob/master/lang/nginx.js)
*   [ ] [`nim`](https://github.com/wooorm/refractor/blob/master/lang/nim.js)
*   [ ] [`nix`](https://github.com/wooorm/refractor/blob/master/lang/nix.js)
*   [ ] [`nsis`](https://github.com/wooorm/refractor/blob/master/lang/nsis.js)
*   [ ] [`objectivec`](https://github.com/wooorm/refractor/blob/master/lang/objectivec.js)
*   [ ] [`ocaml`](https://github.com/wooorm/refractor/blob/master/lang/ocaml.js)
*   [ ] [`oz`](https://github.com/wooorm/refractor/blob/master/lang/oz.js)
*   [ ] [`parigp`](https://github.com/wooorm/refractor/blob/master/lang/parigp.js)
*   [ ] [`parser`](https://github.com/wooorm/refractor/blob/master/lang/parser.js)
*   [ ] [`pascal`](https://github.com/wooorm/refractor/blob/master/lang/pascal.js)
*   [ ] [`perl`](https://github.com/wooorm/refractor/blob/master/lang/perl.js)
*   [ ] [`phpExtras`](https://github.com/wooorm/refractor/blob/master/lang/php-extras.js)
*   [ ] [`php`](https://github.com/wooorm/refractor/blob/master/lang/php.js)
*   [ ] [`powershell`](https://github.com/wooorm/refractor/blob/master/lang/powershell.js)
*   [ ] [`processing`](https://github.com/wooorm/refractor/blob/master/lang/processing.js)
*   [ ] [`prolog`](https://github.com/wooorm/refractor/blob/master/lang/prolog.js)
*   [ ] [`properties`](https://github.com/wooorm/refractor/blob/master/lang/properties.js)
*   [ ] [`protobuf`](https://github.com/wooorm/refractor/blob/master/lang/protobuf.js)
*   [ ] [`puppet`](https://github.com/wooorm/refractor/blob/master/lang/puppet.js)
*   [ ] [`pure`](https://github.com/wooorm/refractor/blob/master/lang/pure.js)
*   [ ] [`python`](https://github.com/wooorm/refractor/blob/master/lang/python.js)
*   [ ] [`q`](https://github.com/wooorm/refractor/blob/master/lang/q.js)
*   [ ] [`qore`](https://github.com/wooorm/refractor/blob/master/lang/qore.js)
*   [ ] [`r`](https://github.com/wooorm/refractor/blob/master/lang/r.js)
*   [ ] [`reason`](https://github.com/wooorm/refractor/blob/master/lang/reason.js)
*   [ ] [`rest`](https://github.com/wooorm/refractor/blob/master/lang/rest.js)
*   [ ] [`rip`](https://github.com/wooorm/refractor/blob/master/lang/rip.js)
*   [ ] [`roboconf`](https://github.com/wooorm/refractor/blob/master/lang/roboconf.js)
*   [ ] [`ruby`](https://github.com/wooorm/refractor/blob/master/lang/ruby.js)
*   [ ] [`rust`](https://github.com/wooorm/refractor/blob/master/lang/rust.js)
*   [ ] [`sas`](https://github.com/wooorm/refractor/blob/master/lang/sas.js)
*   [ ] [`sass`](https://github.com/wooorm/refractor/blob/master/lang/sass.js)
*   [ ] [`scala`](https://github.com/wooorm/refractor/blob/master/lang/scala.js)
*   [ ] [`scheme`](https://github.com/wooorm/refractor/blob/master/lang/scheme.js)
*   [ ] [`scss`](https://github.com/wooorm/refractor/blob/master/lang/scss.js)
*   [ ] [`smalltalk`](https://github.com/wooorm/refractor/blob/master/lang/smalltalk.js)
*   [ ] [`smarty`](https://github.com/wooorm/refractor/blob/master/lang/smarty.js)
*   [ ] [`sql`](https://github.com/wooorm/refractor/blob/master/lang/sql.js)
*   [ ] [`stylus`](https://github.com/wooorm/refractor/blob/master/lang/stylus.js)
*   [ ] [`swift`](https://github.com/wooorm/refractor/blob/master/lang/swift.js)
*   [ ] [`tcl`](https://github.com/wooorm/refractor/blob/master/lang/tcl.js)
*   [ ] [`textile`](https://github.com/wooorm/refractor/blob/master/lang/textile.js)
*   [ ] [`twig`](https://github.com/wooorm/refractor/blob/master/lang/twig.js)
*   [ ] [`typescript`](https://github.com/wooorm/refractor/blob/master/lang/typescript.js) — alias: `ts`
*   [ ] [`verilog`](https://github.com/wooorm/refractor/blob/master/lang/verilog.js)
*   [ ] [`vhdl`](https://github.com/wooorm/refractor/blob/master/lang/vhdl.js)
*   [ ] [`vim`](https://github.com/wooorm/refractor/blob/master/lang/vim.js)
*   [ ] [`wiki`](https://github.com/wooorm/refractor/blob/master/lang/wiki.js)
*   [ ] [`xojo`](https://github.com/wooorm/refractor/blob/master/lang/xojo.js)
*   [ ] [`yaml`](https://github.com/wooorm/refractor/blob/master/lang/yaml.js)

<!--support end-->

## Related

*   [`lowlight`][lowlight] — Same, but based on [`highlight.js`][hljs]

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions -->

[travis-badge]: https://img.shields.io/travis/wooorm/refractor.svg

[travis]: https://travis-ci.org/wooorm/refractor

[codecov-badge]: https://img.shields.io/codecov/c/github/wooorm/refractor.svg

[codecov]: https://www.npmjs.com/package/refractor/tutorial

[npm]: https://docs.npmjs.com/cli/install

[license]: LICENSE

[author]: http://wooorm.com

[rehype]: https://github.com/wooorm/rehype

[names]: http://prismjs.com/#languages-list

[themes]: http://prismjs.com/#theme

[react]: https://facebook.github.io/react/

[vdom]: https://github.com/Matt-Esch/virtual-dom

[to-hyperscript]: https://github.com/wooorm/hast-to-hyperscript

[browser]: #browser

[prism]: https://github.com/PrismJS/prism

[prismjs]: https://www.npmjs.com/package/prismjs

[lowlight]: https://github.com/wooorm/lowlight

[hljs]: https://github.com/isagalaev/highlight.js

[node]: https://github.com/syntax-tree/hast#nodes

[syntax]: #syntaxes

[register]: #refractorregistersyntax