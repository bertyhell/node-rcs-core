# rcs-core

[![Build Status](https://travis-ci.org/JPeer264/node-json-extra.svg)](https://travis-ci.org/JPeer264/node-json-extra)
[![Coverage Status](https://coveralls.io/repos/github/JPeer264/node-rcs-core/badge.svg)](https://coveralls.io/github/JPeer264/node-rcs-core)

> This module is the base for minifying CSS selectors for all files

## Plugins
- Node Plugin: [rename-css-selectors](https://www.npmjs.com/package/rename-css-selectors)
- Grunt Plugin: [grunt-rcs](https://www.npmjs.com/package/grunt-rcs)
- Gulp Plugin: [gulp-rcs](https://www.npmjs.com/package/gulp-rcs)

## Methods

- [rcs.helper](#rcshelper)
- [rcs.nameGenerator](#rcsnamegenerator)
- [rcs.replace](#rcsreplace)
- [rcs.selectorLibrary](#rcsselectorlibrary)

### rcs.helper

#### Methods
- [save](#save)
- [objectToJson](#objecttojson)

##### save

> Saves a file into any path

**rcs.helper.save(destinationPath, data[, options] cb)**

Options:

- overwrite (Boolean): If it should overwrite an existing file. Default `false`.

Example:

```js
const rcs = require('rcs-core');

rcs.helper.save('/my/path/file.txt', 'My data', err => {
    if (err) return console.log(err);

    console.log('The file has been successfully created!');
});
```

##### objectToJson

> This will beautify a json object and returns a string

**rcs.helper.objectToJson(object[, indentation])**

Example:

```js
const rcs = require('rcs-core');
const beautifiedString = rcs.helper.objectToJson({ key: 'value' }, '\t');
```

### rcs.nameGenerator

> This is mainly to generate a new name based on the module `decimal-to-any`

#### Methods
- [generate](#generate)
- [resetCountForTests](#resetcountfortests)

##### generate

> This will generate a new name

**rcs.nameGenerator.generate(prefix)**

Example:

```js
const rcs = require('rcs-core');
const newName = rcs.nameGenerator.generate();
```

##### resetCountForTests

> This is just for tests. This will set the Alphabet to `abcdefghijklmnopqrstuvwxyz`

Usage:

```js
const rcs = require('rcs-core');

describe('a test', () => {
    beforeEach(() => {
        rcs.nameGenerator.resetCountForTests();
    });
};
```

### rcs.replace

#### Methods

- [file](#file)
- [fileCss](#filecss)
- [buffer](#buffer)
- [bufferCss](#buffercss)
- [string](#string)

##### file

> Reads a file and replaces all selectors which are stored in `rcs.selectorLibrary`

> Note: runs internally `buffer`

**rcs.replace.file(filepath[, options], cb)**

Example:

```js
const rcs = require('rcs-core');

rcs.replace.file('./my/file/script.js', (err, data) => {
    if (err) return console.log(err);

    // the minified datastring
    console.log(data.data);
    // the filepath
    console.log(data.filepath);
});
```

##### fileCss

> Reads a CSS file, stores all selectors in `rcs.selectorLibrary` and replaces them

> Note: runs internally `buffer`

**rcs.replace.fileCss(filepath[, options], cb)**

Example:

```js
const rcs = require('rcs-core');

rcs.replace.fileCss('./my/file/style.css', (err, data) => {
    if (err) return console.log(err);

    // the minified datastring
    console.log(data.data);
    // the filepath
    console.log(data.filepath);
});
```

##### buffer

> Replaces all selectors of a buffer which are stored in `rcs.selectorLibrary`

**rcs.replace.buffer(Buffer[, options])**

Example:

```js
const rcs = require('rcs-core');
const replacedBuffer = rcs.replace.buffer(new Buffer('document.getElementById("my-id")');
```

##### bufferCss

> Stores all selectors in `rcs.selectorLibrary` and replaces them

**rcs.replace.bufferCss(Buffer[, options])**

Example:

```js
const rcs = require('rcs-core');
const replacedBuffer = rcs.replace.buffer(new Buffer('#my-id: {}');
```

##### string

> Replaces a given string with the stored value in `rcs.selectorLibrary`

Example:

```js
const rcs = require('rcs-core');
const replacedStringDoubleQuote = rcs.replace.string('"my-id"'); // returns "a"
const replacedStringSingleQuote = rcs.replace.string("'my-id'"); // returns 'a'
```

### rcs.selectorLibrary

#### Methods
- [get](#get)
- [getAll](#getall)
- [set](#set)
- [setExclude](#setexclude)
- [setValue](#setvalue)

##### get

> This will get a specific minified selector

**rcs.selectorLibrary.get(selector[, options])**

Options:

- origValues (boolean): If true the input is the original value. Default: `true`
- isSelectors (boolean): If true it will also add the ID or CLASS prefix (# or .). Default: `false`
- isCompressed (boolean): To see the minified string and not the object itself. Default: `true`

Example:

```js
const rcs = require('rcs-core');

rcs.selectorLibrary.set('#my-id'); // sets to 'a'

rcs.selectorLibrary.get('#my-id'); // a
rcs.selectorLibrary.get('#my-id', { isSelectors: true }); // #a
```

##### getAll

> Returns all setted values as array plus metadata

**rcs.selectorLibrary.getAll([options])**

Options:

- origValues (boolean): If true the input is the original value. Default: `true`
- regex (boolean): This will return a regex of all setted selectors in the selectorLibrary. Default: `false`
- regexCss (boolean): This will return a regex of all setted selectors in the selectorLibrary, optimized for CSS files. Default: `false`
- isSelectors (boolean): If true it will also add the ID or CLASS prefix (# or .). Default: `false`
- extended (boolean): If true it will all selectors with stored metadata. Has NO EFFECT in combination with the option REGEX. Default: `false`

Example:

```js
const rcs = require('rcs-core');

rcs.selectorLibrary.set('#my-id');
rcs.selectorLibrary.set('.a-class');

const allValues = rcs.selectorLibrary.getAll();
```

##### set

> Sets a specific selector into the selectorLibrary

**rcs.selectorLibrary.set(selector[, options])**

Example:

```js
const rcs = require('rcs-core');

rcs.selectorLibrary.set('#my-id'); // sets to 'a'

rcs.selectorLibrary.get('#my-id'); // a
```

##### setExclude

> To exclude some CLASSES or IDs. Good if you use tools such as Modernizr

**rcs.selectorLibrary.setExclude(selector)**

Example:

```js
const rcs = require('rcs-core');

rcs.selectorLibrary.setExclude('js');
rcs.selectorLibrary.setExclude('no-js');

// CLASSES and IDs called `js` or `no-js` will now be ignored
rcs.selectorLibrary.set('no-js'); // will not set the minified one
rcs.selectorLibrary.get('.no-js'); // .no-js
rcs.selectorLibrary.get('no-js'); // no-js
```

##### setValue

> Returns the metainformation of the selector and generates a new name for the selector

**rcs.selectorLibrary.setValue(selector[, options])**

Example:

```js
const rcs = require('rcs-core');

const myClassMeta = rcs.selectorLibrary.setValue('.my-class');

/*
myClassMeta returns:
    {
        type: 'class',
        typeChar: '.',
        selector: '.my-class',
        modifiedSelector: 'my-class',
        compressedSelector: 'a'
    }
 */

```
