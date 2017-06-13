# minipass

A _very_ minimal implementation of a [PassThrough
stream](https://nodejs.org/api/stream.html#stream_class_stream_passthrough)

Supports pipe()ing (including multi-pipe() and backpressure
transmission), buffering data until either a `data` event handler or
`pipe()` is added (so you don't lose the first chunk), and most other
cases where PassThrough is a good idea.

There is a `read()` method, but it's much more efficient to consume
data from this stream via `'data'` events or by calling `pipe()` into
some other stream.  Calling `read()` requires the buffer to be
flattened in some cases, which requires copying memory.  Also,
`read()` always returns Buffers, even if an `encoding` option is
specified.

There is also no `unpipe()` method.  Once you start piping, there is
no stopping it!

This is not a `through` or `through2` stream.  It doesn't transform
the data, it just passes it right through.

If you set `objectMode: true` in the options, then whatever is written
will be emitted.  Otherwise, it'll do a minimal amount of Buffer
copying to ensure proper Streams semantics when `read(n)` is called.

For an example of a stream that extends MiniPass to provide transform
capabilities, check out [minizlib](http://npm.im/minizlib).

## USAGE

```js
const MiniPass = require('minipass')
const mp = new MiniPass(options) // optional: { encoding }
mp.write('foo')
mp.pipe(someOtherStream)
mp.end('bar')
```
