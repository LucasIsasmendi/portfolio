# Research NodeJS

## Various

### Event Loop
#### All you need to know to really understand the Node.js Event Loop and its Metrics
[link](https://www.dynatrace.com/blog/all-you-need-to-know-to-really-understand-the-node-js-event-loop-and-its-metrics/)

### Versus

#### process.nextTick() vs. setImmediate()
`process.nextTick()` queues its callbacks before I/O and timer callbacks.  It is used when we need to postpone emitting an event until after the caller has had the chance to register an event listener for this event.

`setImmediate()` queues its callbacks in the event queue after I/O and timer callbacks.

**NOTE**: the names are counter-intuitive since in reality the setImmediate callback is actually executed after the process.nextTick callback.

[visual explanation](http://stackoverflow.com/questions/17502948/nexttick-vs-setimmediate-visual-explanation)
```js
setImmediate(function A() {
  setImmediate(function B() {
    log(1);
    setImmediate(function D() { log(2); });
    setImmediate(function E() { log(3); });
  });
  setImmediate(function C() {
    log(4);
    setImmediate(function F() { log(5); });
    setImmediate(function G() { log(6); });
  });
});
setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 'TIMEOUT FIRED' 1 4 2 3 5 6
// OR
// 1 'TIMEOUT FIRED' 4 2 3 5 6
```
```js
process.nextTick(function A() {
  process.nextTick(function B() {
    log(1);
    process.nextTick(function D() { log(2); });
    process.nextTick(function E() { log(3); });
  });
  process.nextTick(function C() {
    log(4);
    process.nextTick(function F() { log(5); });
    process.nextTick(function G() { log(6); });
  });
});

setTimeout(function timeout() {
  console.log('TIMEOUT FIRED');
}, 0)
// 1 4 2 3 5 6 'TIMEOUT FIRED'
```
[setImmediate vs. nextTick](http://stackoverflow.com/questions/15349733/setimmediate-vs-nexttick)
```js
import fs from 'fs';
import http from 'http';

const options = {
  host: 'www.stackoverflow.com',
  port: 80,
  path: '/index.html'
};

describe('deferredExecution', () => {
  it('deferredExecution', (done) => {
    console.log('Start');
    setTimeout(() => console.log('TO1'), 0);
    setImmediate(() => console.log('IM1'));
    process.nextTick(() => console.log('NT1'));
    setImmediate(() => console.log('IM2'));
    process.nextTick(() => console.log('NT2'));
    http.get(options, () => console.log('IO1'));
    fs.readdir(process.cwd(), () => console.log('IO2'));
    setImmediate(() => console.log('IM3'));
    process.nextTick(() => console.log('NT3'));
    setImmediate(() => console.log('IM4'));
    fs.readdir(process.cwd(), () => console.log('IO3'));
    console.log('Done');
    setTimeout(done, 1500);
  });
});
```
output
```bash
Start
Done
NT1
NT2
NT3
TO1
IO2
IO3
IM1
IM2
IM3
IM4
IO1
```

#### ECMAScript Modules vs. CommonJS: Or… What is a Module?
[medium - Sep 29, 2016](https://hackernoon.com/node-js-tc-39-and-modules-a1118aecf95e)
It turns out, Node.js and TC-39 have very different ideas of what a “module” is, how those are defined, and how they are loaded into memory and used.
From nearly the beginning, Node.js has had a module system that is derived from a fairly loosely defined specification called “CommonJS”.

The short version is that symbols exported by one JavaScript file (things like functions and variables) are made available for use by another JavaScript file. In Node.js, this is accomplished using the require() function. When a call like require(“foo”) is called within Node.js, there is a very specific sequence of steps performed:
Resolution -> Loading -> Wrapping -> Evaluation -> Caching



#### ES6 modules vs require
[medium - Feb 10, 2017](https://medium.com/the-node-js-collection/an-update-on-es6-modules-in-node-js-42c958b890c)
ES6 modules are loaded, resolved and evaluated asynchronously, it will not be possible to require() an ES6 module. The reason is because require() is a fully synchronous function.


## NPM Tools

### asyc
* [auto](http://caolan.github.io/async/docs.html#auto)
`auto(tasks, concurrency, callback)`
Determines the best order for running the AsyncFunctions in tasks, based on their requirements. Each function can optionally depend on other functions being completed first, and each function is run as soon as its requirements are satisfied.
