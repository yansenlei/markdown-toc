# markdown-toc [![NPM version](https://badge.fury.io/js/markdown-toc.svg)](http://badge.fury.io/js/markdown-toc)  [![Build Status](https://travis-ci.org/jonschlinkert/markdown-toc.svg)](https://travis-ci.org/jonschlinkert/markdown-toc) 

> Generate a markdown TOC (table of contents) with Remarkable.

- Won't mangle markdown in code examples (like headings in gfm fenced code blocks that other TOC generators mistake as being real headings)
- Uses sane defaults, so no customization is necessary, but you can if you need to.
- Get JSON to generate your own TOC from whatever templates you want to use
- [filter](#filter-headings) out headings you don't want
- [Improve](#titleize) the headings you do want

## Install with [npm](npmjs.org)

```bash
npm i markdown-toc --save
```

## Related projects
* [remarkable](https://github.com/jonschlinkert/remarkable): Markdown parser, done right. 100% Commonmark support, extensions, syntax plugins, high speed - all in one.
* [markdown-utils](https://github.com/jonschlinkert/markdown-utils): Micro-utils for creating markdown snippets.
* [markdown-link](https://github.com/jonschlinkert/markdown-link): Micro util for generating a single markdown link.
* [gfm-code-blocks](https://github.com/jonschlinkert/gfm-code-blocks): Extract gfm (GitHub Flavored Markdown) fenced code blocks from a string.

## Usage

```js
var toc = require('markdown-toc');

toc('# One\n\n# Two').content;
// Results in:
// - [One](#one)
// - [Two](#two)
```

To allow customization of the output, an object is returned with the following properties:

 - `content` **{String}**: The generated table of contents. Unless you want to customize rendering, this is all you need.
 - `highest` **{Number}**: The highest level heading found. This is used to adjust indentation. 
 - `tokens` **{Array}**: Headings tokens that can be used for custom rendering


## API

### toc.json

Object for creating a custom TOC. 

```js
toc('# AAA\n## BBB\n### CCC\nfoo').json;

// results in
[ { content: 'AAA', lvl: 1 },
  { content: 'BBB', lvl: 2 },
  { content: 'CCC', lvl: 3 } ]
```


### toc.insert

Insert a table of contents immediately after an _opening_ `

` code comment, or replace an existing TOC if both an _opening_ comment and a _closing_ comment (`

`) are found. 

_(This strategy works well since code comments in markdown are hidden when viewed as HTML, like when viewing a README on GitHub README for example)._

**Example**

```markdown

- old toc 1
- old toc 2
- old toc 3

## abc
This is a b c.

## xyz
This is x y z.
```

Would result in something like:

```markdown

- [abc](#abc)
- [xyz](#xyz)

## abc
This is a b c.

## xyz
This is x y z.
```

### Utility functions

As a convenience to folks who wants to create a custom TOC, markdown-toc's internal utility methods are exposed:

```js
var toc = require('markdown-toc');
```
- `toc.bullets()`: render a bullet list from an array of tokens
- `toc.linkify()`: linking a heading `content` string
- `toc.slugify()`: slugify a heading `content` string 
- `toc.strip()`: strip words or characters from a heading `content` string 

**Example**

```js
var result = toc('# AAA\n## BBB\n### CCC\nfoo');
var str = '';

result.json.forEach(function(heading) {
  str += toc.linkify(heading.content);
});
```

## Options

### options.filter

Type: `Function`

Default: `undefined`

Params: 

 - `str` **{String}** the actual heading string
 - `ele` **{Objecct}** object of heading tokens
 - `arr` **{Array}** all of the headings objects

**Example**

From time to time, we might get junk like this in our TOC. 

```markdown
[.aaa([foo], ...) another bad heading](#-aaa--foo--------another-bad-heading)
```

Unless you like that kind of thing, you might want to filter these bad headings out.

```js
function removeYunk(str, ele, arr) {
  return str.indexOf('...') === -1;
}

var result = toc(str, {filter: removeYunk});
//=> beautiful TOC, sans "Yunk"? wtf is "Yunk" 
// anyway? Look, j and y are kind of sort of 
// close on the keyboard. Regardless, it gets the 
// yunk out of your headings.
```


### options.bullet

Type: `String|Array`

Default: `*`

The bullet to use for each item in the generated TOC. If passed as an array (`['*', '-', '+']`), the bullet point strings will be used based on the header depth.


### options.maxDepth

Type: `Number`

Default: `3`

Use headings whose depth is at most maxDepth.


### options.firsth1

Type: `Boolean`

Default: `true`

Exclude the first h1-level heading in a file. For example, this prevents the first heading in a README from showing up in the TOC.


## Running tests
Install dev dependencies.

```bash
npm i -d && npm test
```

## Contributing
Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/markdown-toc/issues)

## Author

**Jon Schlinkert**
 
+ [github/jonschlinkert](https://github.com/jonschlinkert)
+ [twitter/jonschlinkert](http://twitter.com/jonschlinkert) 

## License
Copyright (c) 2014-2015 Jon Schlinkert  
Released under the MIT license

***

_This file was generated by [verb-cli](https://github.com/assemble/verb-cli) on March 21, 2015._