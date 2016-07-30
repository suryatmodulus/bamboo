# FULL.js

**TL;DR**: Node.js clone in pure JavaScript.

`full.js` is Node.js without C++ parts. The goal is to implement **100%**
of Node.js API without any dependencies on any C++ libraries.

`full.js` will be a drop-in replacement for Node.js and will be able to run
on any JavaScript runtime.

Requirements: it runs on x86_64 Ubuntu 14.04, not tested on other systems.

## API Status

### [`events`](https://nodejs.org/api/events.html)

*Depends on:* Has no dependencies.

Verbatim copy from Node.js

 - [X] `EventEmitter`


### [`process`](https://nodejs.org/api/process.html)

*Depends on:* `process.syscall`, `events`

Extra:

 - [X] `process.syscall(number, ...args): number`
 - [X] `process.syscall64(number, ...args): number`
 - [X] `process.asyscall(number, ...args, callback)`
 - [X] `process.asyscall64(number, ...args, callback)`
 - [X] `process.frame(address: number|[number, number]|[number, number, number]|StaticArrayBuffer, size: number): StaticArrayBuffer`
 - [X] `process.call(addr: any, offset?: number, args?: number[]): number`
 - [X] `process.errno(): number`
 
Standard:

 - [ ] `Event: 'beforeExit'`
 - [ ] `Event: 'disconnect'`
 - [ ] `Event: 'exit'`
 - [ ] `Event: 'message'`
 - [ ] `Event: 'rejectionHandled'`
 - [ ] `Event: 'uncaughtException'`
 - [ ] `Event: 'unhandledRejection'`
 - [ ] `Event: 'warning'`
 - [ ] `Signal Events`
 - [ ] `process.abort()`
 - [ ] `process.arch`
 - [X] `process.argv`
 - [ ] `process.chdir(directory)`
 - [X] `process.config`
 - [ ] `process.connected`
 - [ ] `process.cpuUsage([previousValue])`
 - [X] `process.cwd()` -- check it is 100% compatible with Node.js
 - [ ] `process.disconnect()`
 - [X] `process.env`
 - [ ] `process.emitWarning(warning[, name][, ctor])`
 - [X] `process.execArgv`
 - [X] `process.execPath`
 - [ ] `process.exit([code])`
 - [ ] `process.exitCode`
 - [ ] `process.getegid()`
 - [ ] `process.geteuid()`
 - [X] `process.getgid()`
 - [ ] `process.getgroups()`
 - [ ] `process.getuid()`
 - [ ] `process.hrtime([time])`
 - [ ] `process.initgroups(user, extra_group)`
 - [ ] `process.kill(pid[, signal])`
 - [ ] `process.mainModule`
 - [ ] `process.memoryUsage()`
 - [X] `process.nextTick(callback[, arg][, ...])`
 - [X] `process.pid`
 - [ ] `process.platform`
 - [X] `process.release`
 - [ ] `process.send(message[, sendHandle[, options]][, callback])`
 - [ ] `process.setegid(id)`
 - [ ] `process.seteuid(id)`
 - [ ] `process.setgid(id)`
 - [ ] `process.setgroups(groups)`
 - [ ] `process.setuid(id)`
 - [ ] `process.stderr`
 - [ ] `process.stdin`
 - [ ] `process.stdout`
 - [X] `process.title`
 - [ ] `process.umask([mask])`
 - [ ] `process.uptime()`
 - [X] `process.version`
 - [X] `process.versions`
 - [X] `process.features`
 - [ ] `Exit Codes`


### [`ArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)

full.js requires `ArrayBuffer` constructor, in future we may relax it, so that
full.js would be able to run on runtimes that don't have `ArrayBuffer` implemented.


### `StaticArrayBuffer`

*Depends on:* `ArrayBuffer`, `process.frame`, `process.call`

`StaticArrayBuffer` inherits from `ArrayBuffer` and has one important difference that
garbage collector will not move it in memory.

 - [X] `StaticArrayBuffer`


### [`Buffer`](https://nodejs.org/api/buffer.html)

*Depends on:* `ArrayBuffer`

Use `buffer` *npm* package, which was originally written for Browserify. Internally
it uses `ArrayBuffer`, which is what we need. `buffer` has also a fallback
for older browsers to emulate `ArrayBuffer` with plain JavaScript objects, we may
want to remove that option, as it is no-no for us.

TODO: check all methods work correctly, including the `.slice()` method.

 - [X] `Buffer`


### `StaticBuffer`

*Depends on:* `StaticArrayBuffer`, `Buffer`

`StaticBuffer` inherits from `Buffer` and uses `StaticArrayBuffer` as its backing store
instead of `ArrayBuffer`.

 - [X] `StaticBuffer`
 

### [`util`](https://nodejs.org/api/util.html)

*Depends on:* `Buffer`

 - [X] `util.debuglog(section)`
 - [X] `util.deprecate(function, string)`
 - [X] `util.format(format[, ...])`
 - [X] `util.inherits(constructor, superConstructor)`
 - [X] `util.inspect(object[, options])` -- slightly broken
 - [X] `util.inspect.colors`
 
Deprecated APIs:

 - [X] `util.debug(string)`
 - [X] `util.error([...])`
 - [X] `util.isArray(object)`
 - [X] `util.isBoolean(object)`
 - [X] `util.isBuffer(object)`
 - [X] `util.isDate(object)`
 - [X] `util.isError(object)`
 - [X] `util.isFunction(object)`
 - [X] `util.isNull(object)`
 - [X] `util.isNullOrUndefined(object)`
 - [X] `util.isNumber(object)`
 - [X] `util.isObject(object)`
 - [X] `util.isPrimitive(object)`
 - [X] `util.isRegExp(object)`
 - [X] `util.isString(object)`
 - [X] `util.isSymbol(object)`
 - [X] `util.isUndefined(object)`
 - [X] `util.log(string)`
 - [X] `util.print([...])`
 - [X] `util.puts([...])`
 - [X] `util._extend(obj)`
 
Extra:

 - [X] `util.extend(...objects)` 


### [`assert`](https://nodejs.org/api/assert.html)

*Depends on:* `util`, `Buffer`

 - [X] `assert(value[, message])`
 - [X] `assert.deepEqual(actual, expected[, message])`
 - [X] `assert.deepStrictEqual(actual, expected[, message])`
 - [X] `assert.doesNotThrow(block[, error][, message])`
 - [X] `assert.equal(actual, expected[, message])`
 - [X] `assert.fail(actual, expected, message, operator)`
 - [X] `assert.ifError(value)`
 - [X] `assert.notDeepEqual(actual, expected[, message])`
 - [X] `assert.notDeepStrictEqual(actual, expected[, message])`
 - [X] `assert.notEqual(actual, expected[, message])`
 - [X] `assert.notStrictEqual(actual, expected[, message])`
 - [X] `assert.ok(value[, message])`
 - [X] `assert.strictEqual(actual, expected[, message])`
 - [X] `assert.throws(block[, error][, message])`


### [`path`](https://nodejs.org/api/path.html)

*Depends on:* `util`

Verbatim copy from Node.js

 - [X] `path`


### [`stream`](https://nodejs.org/api/stream.html)

*Depends on:* `events`, `util`, `Buffer`

Almost verbatim copy from Node.js

 - [X] `stream`
 
 
### [`libjs`](http://www.npmjs.com/package/libjs)

*Depends on:* `process.syscall`, `process.syscall64`, `process.asyscall`, `process.asyscall64`

Wrapper around system calls.


### [`fs`](https://nodejs.org/api/fs.html)

*Depends on:* `events`, `path`, `Buffer`, `stream`, `libjs`

Implements `fs.js` API as in the docs, with few minor differences, here are the known ones:

 1. Some error messages may be different.
 2. `utimes()` sets file time in seconds, instead of milliseconds.
 3. `futimes()` is not implemented.
 4. `watch()` function is implemented using the same `inotify(7)` system calls that Node.js is using, 
    however, I don't know why Node.js emits only change and rename events, when `inotify(7)` provides 
    a much more diverse event set, so the mapping to change and rename might be a bit off. It internally 
    uses Inotify class from `libaio` package, for more control and better performance use the 
    `libaio.Inotify` class directly instead.
 5. `readdir()` results may differ as libfs implements it itself instead of using `libc`'s C implementation.
 6. `realpath()` results may differ as libfs implements it itself instead of using `libc`'s C implementation.
 7. `fs.ReadStream` and `fs.WriteStream` are not implemented yet.
 
- [ ] `persistent` currently option in watchFile() and watch() methods is always true, even if you set it to false.

Below synchronous methods are implements using `process.syscall`:

 - [X] `fs.accessSync(path[, mode])`
 - [X] `fs.appendFileSync(file, data[, options])`
 - [X] `fs.chmodSync(path, mode)`
 - [X] `fs.chownSync(path, uid, gid)`
 - [X] `fs.closeSync(fd)`
 - [X] `fs.existsSync(path)`
 - [X] `fs.fchmodSync(fd, mode)`
 - [X] `fs.fchownSync(fd, uid, gid)`
 - [X] `fs.fdatasyncSync(fd)`
 - [X] `fs.fstatSync(fd)`
 - [X] `fs.fsyncSync(fd)`
 - [X] `fs.ftruncateSync(fd, len)`
 - [ ] `fs.futimesSync(fd, atime, mtime)`
 - [ ] `fs.lchmodSync(path, mode)`
 - [X] `fs.lchownSync(path, uid, gid)`
 - [X] `fs.linkSync(srcpath, dstpath)`
 - [X] `fs.lstatSync(path)`
 - [X] `fs.mkdtempSync(prefix)`
 - [X] `fs.mkdirSync(path[, mode])`
 - [X] `fs.openSync(path, flags[, mode])`
 - [ ] `fs.realpathSync(path[, options])`
 - [X] `fs.readFileSync(file[, options])`
 - [ ] `fs.readlinkSync(path[, options])`
 - [ ] `fs.symlinkSync(target, path[, type])`
 - [X] `fs.statSync(path)`
 - [X] `fs.truncateSync(path, len)`
 - [ ] `fs.renameSync(oldPath, newPath)`
 - [ ] `fs.readSync(fd, buffer, offset, length, position)`
 - [ ] `fs.writeSync(fd, buffer, offset, length[, position])`
 - [ ] `fs.writeSync(fd, data[, position[, encoding]])`
 - [ ] `fs.writeFileSync(file, data[, options])`
 - [ ] `fs.unlinkSync(path)`
 - [ ] `fs.rmdirSync(path)`
 
Below asynchronous methods are implemented using `process.asyscall`:
 
 - [X] `fs.access(path[, mode], callback)`
 - [X] `fs.appendFile(file, data[, options], callback)`
 - [X] `fs.chmod(path, mode, callback)`
 - [X] `fs.chown(path, uid, gid, callback)`
 - [X] `fs.close(fd, callback)`
 - [X] `fs.exists(path, callback)`
 - [X] `fs.fchmod(fd, mode, callback)`
 - [X] `fs.fchown(fd, uid, gid, callback)`
 - [X] `fs.fdatasync(fd, callback)`
 - [X] `fs.fstat(fd, callback)`
 - [X] `fs.fsync(fd, callback)`
 - [X] `fs.ftruncate(fd, len, callback)`
 - [ ] `fs.futimes(fd, atime, mtime, callback)`
 - [ ] `fs.lchmod(path, mode, callback)`
 - [X] `fs.lchown(path, uid, gid, callback)`
 - [X] `fs.link(srcpath, dstpath, callback)`
 - [X] `fs.lstat(path, callback)`
 - [X] `fs.mkdir(path[, mode], callback)`
 - [X] `fs.mkdtemp(prefix, callback)`
 - [X] `fs.open(path, flags[, mode], callback)`
 - [ ] `fs.read(fd, buffer, offset, length, position, callback)`
 - [X] `fs.readFile(file[, options], callback)`
 - [ ] `fs.readlink(path[, options], callback)`
 - [ ] `fs.realpath(path[, options], callback)`
 - [ ] `fs.rename(oldPath, newPath, callback)`
 - [ ] `fs.rmdir(path, callback)`
 - [X] `fs.stat(path, callback)`
 - [ ] `fs.symlink(target, path[, type], callback)`
 - [X] `fs.truncate(path, len, callback)`
 - [ ] `fs.unlink(path, callback)`
 - [ ] `fs.write(fd, buffer, offset, length[, position], callback)`
 - [ ] `fs.write(fd, data[, position[, encoding]], callback)`
 - [ ] `fs.writeFile(file, data[, options], callback)`
 
There is no such `readdir` Linux system call, instead `libc` implements it
itself, here we too implement the `readdir` function in JavaScript, so it
might not be 100% compatible with Node.js:
 
 - [ ] `fs.readdirSync(path[, options])`
 - [ ] `fs.readdir(path[, options], callback)`
 
Currently `utime` system call is used instead of `utimes`, so timestamps resolved 
to millisecond accuracy, you can still use `utimes` but if you specify time with 
microsecond accuracy it will be rounded to milliseconds:
  
 - [X] `fs.utimes(path, atime, mtime, callback)`
 - [X] `fs.utimesSync(path, atime, mtime)`
 
We use `inotify` syscall interface (just like `libuv` in Node.js) from 
[`libaio`](http://www.npmjs.com/package/libaio) package:   

 - [ ] `fs.watch(filename[, options][, listener])`
 
Below file watching just polls file system (as does Node.js) using `setTimeout()`. There
are some incompatibilities with Node.js, see [`fslib`](http://www.npmjs.com/package/fslib):
 
 - [ ] `fs.unwatchFile(filename[, listener])`
 - [ ] `fs.watchFile(filename[, options], listener)` 
 - [ ] `Class: fs.FSWatcher`
     - [ ] `Event: 'change'`
     - [ ] `Event: 'error'`
     - [ ] `watcher.close()`

Other:
    
 - [ ] `Class: fs.ReadStream`
     - [ ] `Event: 'open'`
     - [ ] `Event: 'close'`
     - [ ] `readStream.path`
     - [ ] `fs.createReadStream(path[, options])`
 - [ ] `Class: fs.Stats`
 - [ ] `Class: fs.WriteStream`
     - [ ] `Event: 'open'`
     - [ ] `Event: 'close'`
     - [ ] `writeStream.bytesWritten`
     - [ ] `writeStream.path`
     - [ ] `fs.createWriteStream(path[, options])`
 - [ ] `fs.constants`
     - [ ] FS Constants
     - [ ] File Access Constants
     - [ ] File Open Constants
     - [ ] File Type Constants
     - [ ] File Mode Constants


### [`Console`](https://nodejs.org/api/console.html)

*Depends on:* `util`, `stream`, `fs`, `assert`

 - [ ] `new Console(stdout[, stderr])`
 - [ ] `console.assert(value[, message][, ...])`
 - [ ] `console.dir(obj[, options])`
 - [ ] `console.error([data][, ...])`
 - [ ] `console.info([data][, ...])`
 - [ ] `console.log([data][, ...])`
 - [ ] `console.time(label)`
 - [ ] `console.timeEnd(label)`
 - [ ] `console.trace(message[, ...])`
 - [ ] `console.warn([data][, ...])`


### [`dgram`](https://nodejs.org/api/dgram.html)

*Depends on:* 

 - [ ] `Class: dgram.Socket`
     - [ ] `Event: 'close'`
     - [ ] `Event: 'error'`
     - [ ] `Event: 'listening'`
     - [ ] `Event: 'message'`
     - [ ] `socket.addMembership(multicastAddress[, multicastInterface])`
     - [ ] `socket.address()`
     - [ ] `socket.bind([port][, address][, callback])`
     - [ ] `socket.bind(options[, callback])`
     - [ ] `socket.close([callback])`
     - [ ] `socket.dropMembership(multicastAddress[, multicastInterface])`
     - [ ] `socket.send(msg, [offset, length,] port, address[, callback])`
     - [ ] `socket.setBroadcast(flag)`
     - [ ] `socket.setMulticastLoopback(flag)`
     - [ ] `socket.setMulticastTTL(ttl)`
     - [ ] `socket.setTTL(ttl)`
     - [ ] `socket.ref()`
     - [ ] `socket.unref()`
 - [ ] `dgram.createSocket(options[, callback])`
 - [ ] `dgram.createSocket(type[, callback])`


### [`dns`](https://nodejs.org/api/dns.html)

*Depends on:* `util`, `dgram`

 - [ ] `dns.getServers()`
 - [ ] `dns.lookup(hostname[, options], callback)`
     - [ ] `Supported getaddrinfo flags`
 - [ ] `dns.lookupService(address, port, callback)`
 - [ ] `dns.resolve(hostname[, rrtype], callback)`
 - [ ] `dns.resolve4(hostname, callback)`
 - [ ] `dns.resolve6(hostname, callback)`
 - [ ] `dns.resolveCname(hostname, callback)`
 - [ ] `dns.resolveMx(hostname, callback)`
 - [ ] `dns.resolveNaptr(hostname, callback)`
 - [ ] `dns.resolveNs(hostname, callback)`
 - [ ] `dns.resolveSoa(hostname, callback)`
 - [ ] `dns.resolveSrv(hostname, callback)`
 - [ ] `dns.resolvePtr(hostname, callback)`
 - [ ] `dns.resolveTxt(hostname, callback)`
 - [ ] `dns.reverse(ip, callback)`
 - [ ] `dns.setServers(servers)`
 - [ ] Error codes


### [`net`](https://nodejs.org/api/net.html)

*Depends on:* `dns`, `dgram`

 - [ ] `Class: net.Server`
     - [ ] `Event: 'close'`
     - [ ] `Event: 'connection'`
     - [ ] `Event: 'error'`
     - [ ] `Event: 'listening'`
     - [ ] `server.address()`
     - [ ] `server.close([callback])`
     - [ ] `server.connections`
     - [ ] `server.getConnections(callback)`
     - [ ] `server.listen(handle[, backlog][, callback])`
     - [ ] `server.listen(options[, callback])`
     - [ ] `server.listen(path[, backlog][, callback])`
     - [ ] `server.listen(port[, hostname][, backlog][, callback])`
     - [ ] `server.listening`
     - [ ] `server.maxConnections`
     - [ ] `server.ref()`
     - [ ] `server.unref()`
 - [ ] `Class: net.Socket`
     - [ ] `new net.Socket([options])`
     - [ ] `Event: 'close'`
     - [ ] `Event: 'connect'`
     - [ ] `Event: 'data'`
     - [ ] `Event: 'drain'`
     - [ ] `Event: 'end'`
     - [ ] `Event: 'error'`
     - [ ] `Event: 'lookup'`
     - [ ] `Event: 'timeout'`
     - [ ] `socket.address()`
     - [ ] `socket.bufferSize`
     - [ ] `socket.bytesRead`
     - [ ] `socket.bytesWritten`
     - [ ] `socket.connect(options[, connectListener])`
     - [ ] `socket.connect(path[, connectListener])`
     - [ ] `socket.connect(port[, host][, connectListener])`
     - [ ] `socket.connecting`
     - [ ] `socket.destroy([exception])`
     - [ ] `socket.destroyed`
     - [ ] `socket.end([data][, encoding])`
     - [ ] `socket.localAddress`
     - [ ] `socket.localPort`
     - [ ] `socket.pause()`
     - [ ] `socket.ref()`
     - [ ] `socket.remoteAddress`
     - [ ] `socket.remoteFamily`
     - [ ] `socket.remotePort`
     - [ ] `socket.resume()`
     - [ ] `socket.setEncoding([encoding])`
     - [ ] `socket.setKeepAlive([enable][, initialDelay])`
     - [ ] `socket.setNoDelay([noDelay])`
     - [ ] `socket.setTimeout(timeout[, callback])`
     - [ ] `socket.unref()`
     - [ ] `socket.write(data[, encoding][, callback])`
 - [ ] `net.connect(options[, connectListener])`
 - [ ] `net.connect(path[, connectListener])`
 - [ ] `net.connect(port[, host][, connectListener])`
 - [ ] `net.createConnection(options[, connectListener])`
 - [ ] `net.createConnection(path[, connectListener])`
 - [ ] `net.createConnection(port[, host][, connectListener])`
 - [ ] `net.createServer([options][, connectionListener])`
 - [ ] `net.isIP(input)`
 - [ ] `net.isIPv4(input)`
 - [ ] `net.isIPv6(input)`


### [`punycode`](https://nodejs.org/api/punycode.html)

*Depends on:* no dependencies.

 - [X] `punycode.decode(string)`
 - [X] `punycode.encode(string)`
 - [X] `punycode.toASCII(domain)`
 - [X] `punycode.toUnicode(domain)`
 - [X] `punycode.ucs2`
     - [X] `punycode.ucs2.decode(string)`
     - [X] `punycode.ucs2.encode(codePoints)`
 - [X] `punycode.version`

### [Globals](https://nodejs.org/api/globals.html)

 - [X] `Class: Buffer`
 - [X] `__dirname`
 - [X] `__filename`
 - [X] `clearImmediate(immediateObject)`
 - [X] `clearInterval(intervalObject)`
 - [X] `clearTimeout(timeoutObject)`
 - [ ] `console`
 - [X] `exports`
 - [X] `global`
 - [X] `module`
 - [X] `process`
 - [X] `require()`
 - [X] `require.cache`
 - [X] `require.extensions`
 - [X] `require.resolve()`
 - [X] `setImmediate(callback[, arg][, ...])`
 - [X] `setInterval(callback, delay[, arg][, ...])`
 - [X] `setTimeout(callback, delay[, arg][, ...])`



Below is list of already implemented API and roadmap on how the rest of the API will be implemented.

 - [`dgram.js`](https://nodejs.org/api/dgram.html), [`net.js`](https://nodejs.org/api/net.html) and [`http.js`](https://nodejs.org/api/http.html) networking stack will be implemented using `epoll` system calls.
 - [`fs.js`](https://nodejs.org/api/fs.html) -- *synchronous* functions are implemented in [`libfs`](http://www.npmjs.com/package/libfs)
 using system calls, also `libfs` will implement *asynchronous* `fs` calls in four ways:
    1. fake *async* wrappers around *synchronous/blocking* `fs` calls;
    2. thread pool where *blocking* function will be executed in parallel using `treads-a-go-go`, analogous to what `libuv` does;
    3. `Worker` pool, similar to *2.*, but less efficient;
    4. using *asynchronous* Linux system calls: `io_submit`.
 - [`dns.js`](https://nodejs.org/api/dns.html) will be replaced by [`node-dns`](https://github.com/tjfontaine/node-dns).
 - [`tls.js`](https://nodejs.org/api/tls.html) will be replaced by [`forge`](https://github.com/digitalbazaar/forge). 
 - [`https.js`](https://nodejs.org/api/https.html) will have to be put together using `http.js` and `tls.js`. 
 - [`zlib.js`](https://nodejs.org/api/tls.html) will be replaced by [`pako`](https://github.com/nodeca/pako).
 - For [`crypto.js`](https://nodejs.org/api/crypto.html) there is `browserify-crypto` and some other libraries we can use.
 - Pure JavaScript modules will be adopted *as-is*:
    - `assert.js`
    - `console.js`
    - `events.js`
    - `path.js`
    - `process.js`
    - `punycode.js`
    - `querystring.js`
    - `readline.js`
    - `repl.js`
    - `module.js`
    - `url.js`
    - `util.js`
    - `stream.js`
    - `string_decoder.js`
 - *Misc* V8 and Node.js specific modules:
    - `vm.js`
    - `v8.js`
    - `os.js`
    - `tty.js`
    - `timers.js`
 - These we need to think about:
    - `child_process.js`
    - `cluster.js`

## JavaScript Runtimes

Currently `full.js` runs on Node.js and Duktape, but we should be able to
port it to all runtimes that run on Linux

 - [X] Node.js
 
JIT compiling runtimes:

 - [ ] Carakan
 - [ ] Chakra -- currently Chakra runs only on Windows
 - [ ] SpiderMonkey
 - [ ] JavaScriptCore
 - [ ] V8
 - [ ] Nashorn

Interpreter runtimes:

 - [ ] Continuum
 - [ ] Futhark
 - [ ] InScript
 - [ ] JScript
 - [ ] KJS
 - [ ] Linear B
 - [ ] Narcissus
 - [ ] JS-Interpreter
 - [ ] Rhino
 - [ ] YAJI
 - [X] Duktape
 - [ ] Jsish
 - [ ] Websocket.js
 - [ ] Espruino
 - [ ] MuJS
 - [ ] V7
 - [ ] Tiny-JS
 - [ ] JerryScript
 
 
 ## TODOs
 
  - [ ] Use asynchronous Linux I/O for file systems on file systems that support that.
  - [ ] Integrate `process.asyscall` with `epoll`.
  