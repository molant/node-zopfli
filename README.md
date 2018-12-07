# node-zopfli

[![NPM version][npm-image]][npm-url]
[![Build status][pipelines-image]][pipelines-url]
[![Dependency Status][dep-image]][dep-url]
[![devDependency Status][devDep-image]][devDep-url]

Node.js bindings for [Zopfli](https://en.wikipedia.org/wiki/Zopfli) compressing library.
Compress gzip files 5% better compared to gzip.

It is considerably slower than gzip (~100x) so you may want to use it only for static content and cached resources.

**NOTE:** This is a fork from https://github.com/pierreinglebert/node-zopfli/ which hasn't been update in years and cannot be installed with the latest node versions. Also the name of this package is `zopfli-node` instead of `node-zopfli`.


## Prerequisites for building

* Python 2.7
* GCC and Make (Unix) or Visual Studio Express (Windows), see [Node Building tools](https://github.com/TooTallNate/node-gyp#installation)

## Usage

### Install

```shell
npm install zopfli-node
```

or if you want zopfli binary globally

```shell
npm install -g zopfli-node
```

### Binary (from command line)
To gzip a file

```shell
zopfli file.txt
```

To compress a png file

```shell
zopflipng file.png out.png
```

### Usage examples
#### Stream (async):

```js
var zopfli = require('node-zopfli');
fs.createReadStream('file.js')
  .pipe(zopfli.createGzip(options))
  .pipe(fs.createWriteStream('file.js.gz'));
```

Instead of `zopfli.createGzip`, you can also use

```js
new Zopfli('gzip', options);
```

#### Buffer (async):

```js
var zopfli = require('node-zopfli');
var input = new Buffer('I want to be compressed');
zopfli.deflate(input, options, function(err, deflated) {});
zopfli.zlib(input, options, function(err, zlibed) {});
zopfli.gzip(input, options, function(err, gziped) {});
```

#### Buffer (sync):

```js
var zopfli = require('node-zopfli');
var input = new Buffer('I want to be compressed');
var deflated = zopfli.deflateSync(input, options);
var zlibed = zopfli.zlibSync(input, options);
var gziped = zopfli.gzipSync(input, options);
```

### API

#### compress(input, format, [options, callback])

`input` is the input buffer

`format` can be one of `deflate`, `zlib` and `gzip`

`callback`, if present, gets two arguments `(err, buffer)` where `err` is an error object, if any, and `buffer` is the resultant compressed data.

If no callback is provided, it returns an A+ Promise.

##### aliases

`deflate`, `zlib` and `gzip` methods are aliases on `compress` without `format` argument.

#### Options

Here are the options with defaults values you can pass to zopfli:

```js
{
  verbose: false,
  verbose_more: false,
  numiterations: 15,
  blocksplitting: true,
  blocksplittinglast: false,
  blocksplittingmax: 15
}
```

##### numiterations
Maximum amount of times to rerun forward and backward pass to optimize LZ77 compression cost. Good values: 10, 15 for small files, 5 for files over several MB in size or it will be too slow.

##### blocksplitting
If true, splits the data in multiple deflate blocks with optimal choice for the block boundaries. Block splitting gives better compression.

##### blocksplittinglast
If true, chooses the optimal block split points only after doing the iterative LZ77 compression. If false, chooses the block split points first, then does iterative LZ77 on each individual block. Depending on the file, either first or last gives the best compression.

##### blocksplittingmax
Maximum amount of blocks to split into (0 for unlimited, but this can give extreme results that hurt compression on some files).


## Build from sources

```shell
git clone https://github.com/molant/node-zopfli --recursive
cd node-zopfli
npm install
```

## Tests
mocha is used for tests, you can run them with:

```shell
npm test
```


[npm-image]: https://img.shields.io/npm/v/zopfli-node.svg
[npm-url]: https://www.npmjs.com/package/zopfli-node
[pipelines-image]: https://molant.visualstudio.com/zopfli/_apis/build/status/molant.node-zopfli
[pipelines-url]: https://molant.visualstudio.com/zopfli/_build/latest?definitionId=3
[dep-image]: https://img.shields.io/david/molant/node-zopfli.svg
[dep-url]: https://david-dm.org/molant/node-zopfli
[devDep-image]: https://img.shields.io/david/dev/molant/node-zopfli.svg
[devDep-url]: https://david-dm.org/molant/node-zopfli#info=devDependencies
