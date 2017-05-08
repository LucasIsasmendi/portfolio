# Node.js Dessign Patterns - second edition
[code repository](https://github.com/PacktPublishing/Node.js_Design_Patterns_Second_Edition_Code)

## Index
[Chapter 1: Welcome to the Node.js Platform](#ch1-welcome-to-the-nodejs-platform)  
[Chapter 2: Node.js Essential Patterns](#ch2-nodejs-essential-patterns)  
[Chapter 3: Asynchronous Control Flow Patterns with Callbacks](#ch3-asynchronous-control-flow-patterns-with-callbacks)

## Ch1 Welcome to the Node.js Platform
In this chapter, we will learn the following topics:
* The Node.js philosophy, the "Node way"
* Node.js version 6 and ES2015
* The reactor pattern—the mechanism at the heart of the Node.js asynchronous architecture

### The Node.js philosophy
* Small core
* Small modules: not only in terms of code size, but most importantly in terms of scope.
* Small surface area: expose minimal set of functionalities. Created to be used rather than extended.
* Simplicity and pragmatism

### Introduction to Node.js 6 and ES2015
#### The let and const keywords
Access variables that belong to another scope: `var` returns undefined, `let` returns ReferenceError (safer code).

It is becoming best practice to use `const` when requiring a module in a script, so that the variable holding the module cannot be accidentally reassigned

**Note**: If you want to create an immutable object, `const` is not enough, so you should use ES5's method [Object.freeze()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)

#### The arrow function
Arrow functions are bound to their lexical scope. This means that inside an arrow function the value of `this` is the same as in the parent block. In ES5 use `bind` to create same behavior.
```js
function DelayedGreeter(name) {
  this.name = name;
}
DelayedGreeter.prototype.greetold = function() {
  setTimeout( (function cb() {
    console.log('Old ' + this.name);
  }).bind(this), 500);
};
DelayedGreeter.prototype.greetnew = function() {
  setTimeout( () => console.log('New' + this.name), 500);
};
const greeter = new DelayedGreeter('Way');
greeter.greetold(); // will print "Old Way"
greeter.greetnew(); // will print "New Way"
```
#### Class syntax
leverage prototypical inheritance similar to OOP like Java or C#. It is just syntactic sugar: JS internally handles inherit properties and functions through prototypes and not through classes.

##### classic prototype-based approach
```js
function Person(name, surname, age) {
  this.name = name;
  this.surname = surname;
  this.age = age;
}
Person.prototype.getFullName = function() {
  return this.name + '' + this.surname;
};
Person.older = function(person1, person2) {
  return (person1.age >= person2.age) ? person1 : person2;
};
```
##### new handy ES2015 class syntax
```js
class Person {
  constructor (name, surname, age) {
    this.name = name;
    this.surname = surname;
    this.age = age;
  }
  getFullName () {
    return this.name + ' ' + this.surname;
  }
  static older (person1, person2) {
    return (person1.age >= person2.age) ? person1 : person2;
  }
}
```
##### killer feature with extend and super keywords
```js
class PersonWithMiddlename extends Person {
  constructor (name, middlename, surname, age) {
    super(name, surname, age);
    this.middlename = middlename;
  }
  getFullName () {
    return this.name + '' + this.middlename + '' + this.surname;
  }
}
```

#### Enhanced object literals
shorthand to assign variables and functions as members of the object
``` js
const x = 22;
const y = 17;
const obj = { x, y };

```
We can do the same with functions:
``` js
module.exports = {
  square (x) {
    return x * x;
  },
  cube (x) {
    return x * x * x;
  }
};
```
> Notice that we don't need to specify the keyword `function`.

new setter and getter syntax
``` js
const person = {
  name : 'George',
  surname : 'Boole',

  get fullname () {
    return this.name + '' + this.surname;
  },

  set fullname (fullname) {
    let parts = fullname.split('');
    this.name = parts[0];
    this.surname = parts[1];
  }
};

console.log(person.fullname); // "George Boole"
console.log(person.fullname = 'Alan Turing'); // "Alan Turing"
console.log(person.name); // "Alan"
```

#### Map and Set collections
 the Map prototype offers several handy methods, such as `set`, `get`, has, and `delete`, and the `size` attribute. We can also iterate through all the entries using the `for...of` syntax
``` js
const profiles = new Map();
profiles.set('twitter', '@adalovelace');
profiles.set('facebook', 'adalovelace');
profiles.set('googleplus', 'ada');

profiles.size; // 3
profiles.has('twitter'); // true
profiles.get('twitter'); // "@adalovelace"
profiles.has('youtube'); // false
profiles.delete('facebook');
profiles.has('facebook'); // false
profiles.get('facebook'); // undefined
for (const entry of profiles) {
  console.log(entry);
}
```
**feature**: using functions and objects as keys of the map, and this is something that is not entirely possible using plain objects, because with objects all the keys are automatically cast to strings.
``` js
const tests = new Map();
tests.set(() => 2+2, 4);
tests.set(() => 2*2, 4);
tests.set(() => 2/2, 1);
for (const entry of tests) {
  console.log((entry[0]() === entry[1]) ? 'PASS' : 'FAIL');
}
```
> all the entries respect the order in which they have been inserted

Along with `Map`, ES2015 also introduces the `Set` prototype, which means lists with unique values:
``` js
const s = new Set([0, 1, 2, 3]);
s.add(3); // will not be added
s.size; // 4
s.delete(0);
s.has(0); // false
for (const entry of s) {
  console.log(entry);
}
```
> sets can also contain objects and functions as values.

#### WeakMap and WeakSet collections
`WeakMap` is quite similar to `Map` in terms of interface; however, there are two main differences you should be aware of: there is no way to iterate all over the entries, and it only allows having objects as keys. Allows objects used as keys to be garbage collected when the only reference left is inside `WeakMap`.
``` js
let obj = {};
const map = new WeakMap();
map.set(obj, {key: "some_value"});
console.log(map.get(obj)); // {key: "some_value"}
obj = undefined;
// now obj and the associated data in the map  
// will be cleaned up in the next gc cycle
```

Similar to `WeakMap`, `WeakSet` is the weak version of `Set`
``` js
let obj1= {key: "val1"};
let obj2= {key: "val2"};
const set= new WeakSet([obj1, obj2]);
console.log(set.has(obj1)); // true
obj1= undefined; // now obj1 will be removed from the set
console.log(set.has(obj1)); // false
```
> It's important to understand that WeakMap and WeakSet are not better or worse than Map and Set, they are simply more suitable for different use cases.

#### Template literals
powerful syntax to define strings, can interpolate variables or expressions using `${expression}` inside the string

#### Other ES2015 features
Another extremely interesting feature added in ES2015 and available since Node.js version 4 is Promise (ch4)

Other interesting ES2015 features introduced in Node.js version 6 are as follows:
* Default function parameters
* Rest parameters
* Spread operator
* Destructuring
* `new.target` (we will talk about this in Chapter 2, Node.js Essential Patterns)
* Proxy (we will talk about this in Chapter 6, Design Patterns)
* Reflect
* Symbols

**Note**: more about ES6 in the [official Node.js documentation](https://nodejs.org/en/docs/es6/)

### The reactor pattern
 The heart of the asynchronous nature of Node.js: single-threaded architecture and non-blocking I/O.

#### I/O is slow
RAM access is in nanoseconds (10E-9 seconds), disk/network access in in milliseconds (10E-3 seconds)

#### Blocking I/O
In traditional blocking I/O programming, the function call corresponding to an I/O request will block the execution of the thread until the operation completes. A thread is not cheap in terms of system resources; it consumes memory and causes context switches.

#### Non-blocking I/O
the system call always returns immediately without waiting for the data to be read or written. If no results are available at the moment of the call, the function will simply return a predefined constant, indicating that there is no data available to return at that moment.

#### Event demultiplexing

most modern operating systems provide a native mechanism to handle concurrent, non-blocking resources in an efficient way; this mechanism is called **synchronous event demultiplexer** or **event notification interface**. This component collects and queues I/O events that come from a set of watched resources, and block until new events are available to process.

#### Introducing the reactor pattern
The main idea is to have a handler (which in Node.js is represented by a callback function) associated with each I/O operation, which will be invoked as soon as an event is produced and processed by the event loop.

<p align="center">
  <img src="/images/books/nodejs-patterns-2ed/ch1-reactor pattern.jpg" width="700">
</p>

**Pattern (reactor)** handles I/O by blocking until new events are available from a set of observed resources, and then reacts by dispatching each event to an associated handler.

**Note**: A Node.js application will exit automatically when there are no more pending operations in the `Event Demultiplexer`, and no more events to be processed inside the `Event Queue`.

#### The non-blocking I/O engine of Node.js-libuv
Each operating system has its own interface for the Event Demultiplexer: epoll on Linux, kqueue on Mac OS X, and the I/O Completion Port (IOCP) API on Windows. All these inconsistencies across and within the different operating systems required a higher-level abstraction to be built for the Event Demultiplexer. This is exactly why the Node.js core team created a C library called **libuv**, with the objective to make Node.js compatible with all the major platforms and normalize the non-blocking behavior of the different types of resource; libuv today represents the low-level I/O engine of Node.js.

**Note**: [An intoduction to libuv](http://nikhilm.github.io/uvbook/), a free book to learn more about livub

#### The recipe for Node.js
The reactor pattern and libuv are the basic building blocks of Node.js, but we need the following three other components to build the full platform:
* A set of **bindings** responsible for wrapping and exposing libuv and other low-level functionality to JavaScript.
* **V8**, the JavaScript engine originally developed by Google for the Chrome browser. This is one of the reasons why Node.js is so fast and efficient. V8 is acclaimed for its revolutionary design, its speed, and for its efficient memory management.
* A core JavaScript library (called **node-core**) that implements the high-level Node.js API.

Finally, this is the recipe of Node.js, and the following image represents its final architecture:
<p align="center">
  <img src="/images/books/nodejs-patterns-2ed/ch1-node architecture.jpg" width="500">
</p>


## Ch2 Node.js Essential Patterns
Node.js offers a series of tools and design patterns to deal optimally with asynchronous code. In this chapter, we will see two of the most important asynchronous patterns: callback and event emitter.

### The Callback pattern
Callbacks are the materialization of the handlers of the reactor pattern, are functions that are invoked to propagate the result of an operation and this is exactly what we need when dealing with asynchronous operations. They do replace the use of the `return` instruction that always executes synchronously.
Another ideal construct for implementing callbacks is [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures). With closures, we can in fact reference the environment in which a function was created; we can always maintain the context in which the asynchronous operation was requested, no matter when or where its callback is invoked.

#### The continuation-passing style
In JavaScript, a callback is a function that is passed as an argument to another function and is invoked with the result when the operation completes. In functional programming, this way of propagating the result is called **continuation-passing style (CPS)**. It is a general concept, and it is not always associated with asynchronous operations. In fact, it simply indicates that a result is propagated by passing it to another function (the callback), instead of directly returning it to the caller.

##### Synchronous continuation-passing style
* direct style
```js
function add(a, b) {
  return a + b;
}
```
* continuation-passing style
```js
function add(a, b, callback) {
  callback(a + b);
}
console.log('before');
add(1, 2, result => console.log('Result: ' + result));
console.log('after');
// before
// Result: 3
// after
```

##### Asynchronous continuation-passing style
```js
function additionAsync(a, b, callback) {
  setTimeout(() => callback(a + b), 100);
}
console.log('before');
additionAsync(1, 2, result => console.log('Result: ' + result));
console.log('after');
//before
//after
//Result: 3
```
This property in Node.js is crucial, as it gives control back to the event loop as soon as an asynchronous request is sent, thus allowing a new event from the queue to be processed.

<p align="center">
  <img src="/images/books/nodejs-patterns-2ed/ch2-event loop.jpg" width="500">
</p>

Thanks to closures, it is trivial to maintain the context of the caller of the asynchronous function, even if the callback is invoked at a different point in time and from a different location.

##### Non-continuation-passing style callbacks
There are several circumstances in which the presence of a callback argument might make us think that a function is asynchronous or is using a continuation-passing style; that's not always true.
```js
const result = [1, 5, 7].map(element => element - 1);
console.log(result); // [0, 4, 6]
```
> The result is returned synchronously using a direct style.

#### Synchronous or asynchronous?
 In general, what must be avoided is creating inconsistency and confusion around the nature of an API. Example the case of an inconsistently asynchronous function.

##### An unpredictable function
One of the most dangerous situations is to have an API that behaves synchronously under certain conditions and asynchronously under others.
```js
const fs = require('fs');
const cache = {};
function inconsistentRead(filename, callback) {
  if(cache[filename]) {
    //invoked synchronously
    callback(cache[filename]);   
  } else {
    //asynchronous function
    fs.readFile(filename, 'utf8', (err, data) => {
      cache[filename] = data;
      callback(data);
    });
  }
}
```
> uses the cache variable to store the results of different file read operations.

##### Unleashing Zalgo
```js
function createFileReader(filename) {
  const listeners = [];
  inconsistentRead(filename, value => {
    listeners.forEach(listener => listener(value));
  });

  return {
    onDataReady: listener => listeners.push(listener)
  };
}
```
> creates a new object that acts as a notifier, allowing us to set multiple listeners for a file read operation.

```js
const reader1 = createFileReader('data.txt'); //(1)
reader1.onDataReady(data => {
  console.log('First call data: ' + data);
  //...sometime later we try to read again from the same file
  const reader2 = createFileReader('data.txt'); //(2)
  reader2.onDataReady( data => {
    console.log('Second call data: ' + data); //(3)
  });
});
// First call data: some data
```
(1) Behaves asynchronously, because there is no cached result available.
(2) cache for the requested file already exists, the inner call to inconsistentRead() will be synchronous.
(3) will never be invoked.

The callback behavior of our inconsistentRead() function is really unpredictable, as it depends on many factors, such as the frequency of its invocation, the filename passed as argument, and the amount of time taken to load the file.
> Isaac Z. Schlueter, creator of npm and former Node.js project lead, in one of his blog posts compared the use of this type of unpredictable functions to [unleashing Zalgo]( http://blog.izs.me/post/59142742143/designing-apis-for-asynchrony .).

##### Using synchronous APIs
It is imperative for an API to clearly define its nature: either synchronous or asynchronous.
One suitable fix for our `inconsistentRead()` function is to make it totally synchronous.
```js
const fs = require('fs');
const cache = {};
function consistentReadSync(filename) {
  if(cache[filename]) {
    return cache[filename];   
  } else {
    cache[filename] = fs.readFileSync(filename, 'utf8');
    return cache[filename];
  }
}
```
The entire function was also converted to a **direct style**. There is no reason for a function to have a **continuation-passing style** if it is synchronous

**Note Pattern**: Prefer the direct style for purely synchronous functions.
Changing an API from CPS(async) to a direct style(sync) might also require a change to the style of all the code using it (`createFileReader()`).

Synchronous API:
- might not always be available.
- will block the event loop and put the concurrent requests on hold.

Using synchronous I/O in Node.js is strongly discouraged in many circumstances(to serve concurrent requests); however, it makes perfect sense to use a synchronous blocking API to load a configuration file while bootstrapping an application.

##### Deferred execution
Another alternative for fixing our `inconsistentRead()` function is to make it purely asynchronous.
The trick here is to schedule the synchronous callback invocation to be executed "in the future" instead of being run immediately in the same event loop cycle. In Node.js, this is possible using `process.nextTick()`: it takes a callback as an argument and pushes it to the top of the event queue, in front of any pending I/O event, and returns immediately. The callback will then be invoked as soon as the event loop runs again.

```js
const fs = require('fs');
const cache = {};
function consistentReadAsync(filename, callback) {
  if(cache[filename]) {
    process.nextTick(() => callback(cache[filename]));
  } else {
    //asynchronous function
    fs.readFile(filename, 'utf8', (err, data) => {
      cache[filename] = data;
      callback(data);
    });
  }
}
```
Another API for deferring the execution of code is `setImmediate()`. Callbacks deferred with `process.nextTick()` run before any other I/O event is fired, while with `setImmediate()`, the execution is queued behind any I/O event that is already in the queue. Since `process.nextTick()` runs before any already scheduled I/O, it might cause I/O starvation under certain circumstances, for example, a recursive invocation; this can never happen with `setImmediate()`.

**Note Pattern**: We guarantee that a callback is invoked asynchronously by deferring its execution using `process.nextTick()`.

#### Node.js callback conventions
In Node.js, continuation-passing style APIs and callbacks follow a set of specific conventions.
##### Callbacks come last
##### Error comes first: It is best practice to always check for the presence of an error. Type `Error`
##### Propagating errors
In synchronous: `throw` statement, which causes the error to jump up in the call stack until it is caught.

In asynchronous: pass the error to the next callback in the chain. The typical pattern looks as follows:
```js
const fs = require('fs');
function readJSON(filename, callback) {
  fs.readFile(filename, 'utf8', (err, data) => {
    let parsed;
    if(err)
      //propagate the error and exit the current function
      return callback(err);
    try {
      //parse the file contents
      parsed = JSON.parse(data);
    } catch(err) {
      //catch parsing errors
      return callback(err);
    }
    //no errors, propagate just the data
    callback(null, parsed);
  });
};
```
> callback is invoked when we want to pass a valid result and when we want to propagate an error.

When we are propagating an error we use the `return` statement to exit from the function as soon as the callback function is invoked and to avoid executing the next lines in `readJSON`.

#### Uncaught exceptions
`try...catch` block, throwing inside an asynchronous callback will cause the exception to jump up to the event loop and never be propagated to the next callback. In Node.js, this is an unrecoverable state and the application will simply shut down printing the error to the `stderr` interface.
Node.js emits a special event called `uncaughtException` just before exiting the process, it leaves the application in a state that is not guaranteed to be consistent, which can lead to unforeseeable problems. That's why it is always advised, especially in production, to exit from the application after an uncaught exception is received anyway: `process.exit(1);`.


### The module system and its patterns
Modules are the bricks for structuring non-trivial applications, but also the main mechanism to enforce information hiding by keeping private all the functions and variables that are not explicitly marked to be exported.

#### The revealing module pattern
This pattern leverages a self-invoking function to create a private scope, exporting only the parts that are meant to be public, the idea behind this pattern is used as a base for the Node.js module system.
```js
const module = (() => {
  const privateFoo = () => {...};
  const privateBar = [];
  const exported = {
    publicFoo: () => {...},
    publicBar: () => {...}
  };
  return exported;
})();
console.log(module);
```

#### Node.js modules explained
CommonJS is a group with the aim to standardize the JavaScript ecosystem, and one of their most popular proposals is called CommonJS modules. Node.js built its module system on top of this specification, with the addition of some custom extensions

##### A homemade module loader
The source code of a module is essentially wrapped into a function, as it was for the revealing module pattern.

##### Defining a module
Everything inside a module is private unless it's assigned to the module.exports variable. The content of this variable is then cached and returned when the module is loaded using require().

##### Defining globals
Everything that is assigned to `global` variable will end up automatically in the global scope.
**Note**: Polluting the global scope is considered bad practice and nullifies the advantage of having a module system.

##### module.exports versus exports
The variable `exports` is just a reference to the initial value of `module.exports`.
If we want to export something other than an object literal, such as a function, an instance, or even a string, we have to reassign` module.exports`

##### The require function is synchronous
Require returns the module contents using a simple direct style, and no callback is required. As a consequence, any assignment to `module.exports` must be synchronous as well.
This is actually one of the most important reasons why the core Node.js libraries offer synchronous APIs as an alternative to most of the asynchronous ones. In its early days, Node.js used to have an asynchronous version of require(), but it was soon removed because it was overcomplicated.

##### The resolving algorithm
The term **dependency hell** describes a situation whereby the dependencies of software in turn depend on a shared dependency, but require different incompatible versions. Node.js solves this problem elegantly by loading a different version of a module depending on where the module is loaded from. All the merits of this feature go to npm,

The `resolve()` function takes a module name (which we will call here, `moduleName`) as input and it returns the full path of the module.
For file and package modules, both the individual files and directories can match `moduleName`. In particular, the algorithm will try to match the following:
* `<moduleName>.js`
* `<moduleName>/index.js`
* The directory/file specified in the `main` property of `<moduleName>/package.json`

Formal documentation [nodejs org](https://nodejs.org/api/modules.html#modules_all_together)
Each package can have its own private dependencies.
The resolving algorithm is the core part behind the robustness of the Node.js dependency management, and is what makes it possible to have hundreds or even thousands of packages in an application without having collisions or problems of version compatibility.

##### The module cache
Each module is only loaded and evaluated the first time it is required, since any subsequent call of `require()` will simply return the cached version `require.cache`.

##### Circular dependencies
Many consider circular dependencies an intrinsic design issue, but it is something which might actually happen in a real project.

#### Module definition patterns
The module system, besides being a mechanism for loading dependencies, is also a tool for defining APIs. As for any other problem related to API design, the main factor to consider is the balance between private and public functionality. The aim is to maximize information hiding and API usability, while balancing these with other software qualities such as extensibility and code reuse.

##### Named exports
Consists of assigning all the values we want to make public to properties of the object referenced by `exports` (or `module.exports`). In this way, the resulting exported object becomes a container or namespace for a set of related functionality.
```js
//file logger.js
exports.info = (message) => {
  console.log('info: ' + message);
};
exports.verbose = (message) => {
  console.log('verbose: ' + message);
};
//file main.js
const logger = require('./logger');
logger.info('This is an informational message');
logger.verbose('This is a verbose message');
```
> Most of the Node.js core modules use this pattern.
The CommonJS specification only allows the use of the exports variable to expose public members.

##### Exporting a function
**substack pattern**: Expose the main functionality of a module by exporting only one function. Use the exported function as namespace to expose any auxiliary functionality.
```js
//file logger.js
module.exports = (message) => {
  console.log(`info: ${message}`);
};
// using the exported function as a namespace
module.exports.verbose = (message) => {
  console.log(`verbose: ${message}`);
};
```
> The modularity of Node.js heavily encourages the adoption of the **Single Responsibility Principle (SRP)**

##### Exporting a constructor
A module that exports a constructor is a specialization of a module that exports a function (create new instances + extend its prototype).
```js
//file logger.js
function Logger(name) {
  this.name = name;
}
Logger.prototype.log = function(message) {
  console.log(`[${this.name}] ${message}`);
};
Logger.prototype.verbose = function(message) {
  this.log(`verbose: ${message}`);
};
module.exports = Logger;
```
In the same fashion we can easily export an ES2015 class: Given that ES2015 classes are just syntactic sugar for prototypes, the usage of this module will be exactly the same as its prototype-based alternative.
> compared to the substack pattern, it exposes a lot more of the module internals;

A variation of this pattern consists of applying a guard against invocations that doesn't use the new instruction. This little trick allows us to use our module as a factory
```js
function Logger(name) {
  if(!new.target) { // ES6 & node6 === this instanceof Logger
    return new Logger(name);
  }
  this.name = name;
}
```
##### Exporting an instance
Because the module is cached, every module that requires the logger module will actually always retrieve the same instance of the object, thus sharing its state.
This pattern is very much like creating a singleton; however, it does not guarantee the uniqueness of the instance across the entire application, as it happens in the traditional singleton pattern.
```js
//file logger.js
function Logger(name) {
  this.count = 0;
  this.name = name;
}
Logger.prototype.log = function(message) {
  this.count++;
  console.log('[' + this.name + '] ' + message);
};
module.exports = new Logger('DEFAULT');
```
An extension to the pattern we just described, consists of exposing the constructor used to create the instance, in addition to the instance itself.
```js
module.exports.Logger = Logger;
```
##### Modifying other modules or the global scope
Useful for testing. Called **monkey patching**: modifying the existing objects at runtime to change or extend their behavior or to apply temporary fixes.
```js
// ./logger is another module
require('./logger').customMessage = () => console.log('This is a new functionality');
```
> The techniques described here are all dangerous ones to apply. The main concern is that having a module that modifies the global namespace or other modules is an operation with side effects.

### The observer pattern
Is one of the pillars of the platform and an absolute prerequisite for using many node-core and userland modules.
Ideal solution for modeling the reactive nature of Node.js
Definition: **Pattern (observer)** defines an object (called subject), which can notify a set of observers (or listeners), when a change in its state happens.
> The main difference from the callback pattern is that the subject can actually notify multiple observers.

#### The EventEmitter class
In node observer pattern is built into the core and is available through the `EventEmitter class`: allows us to register one or more functions as listeners.
<p align="center">
  <img src="/images/books/nodejs-patterns-2ed/ch2-event-emitter.jpg" width="500">
</p>
```js
const EventEmitter = require('events').EventEmitter;
const eeInstance = new EventEmitter();
```
The essential methods of the `EventEmitter` are:
* `on(event, listener)`: to register a new listener (a function) for the given event type (a string)
* `once(event, listener)`: registers a new listener, which is then removed after the event is emitted for the first time
* `emit(event, [arg1], [...])`: produces a new event and provides additional arguments to be passed to the listeners
* `removeListener(event, listener)`: removes a listener for the specified event type
> the first argument is not an error

#### Creating and using EventEmitter
```js
const EventEmitter = require('events').EventEmitter;
const fs = require('fs');
function findPattern(files, regex) {
  const emitter = new EventEmitter();
  files.forEach(function(file) {
    fs.readFile(file, 'utf8', (err, content) => {
      if(err) return emitter.emit('error', err);
      emitter.emit('fileread', file);
      let match;
      if(match = content.match(regex))
        match.forEach(elem => emitter.emit('found', file, elem));
    });
  });
  return emitter;
}
findPattern(['fileA.txt', 'fileB.json'], /hello \w+/g)
  .on('fileread', file => console.log(file + ' was read'))
  .on('found', (file, match) => console.log('Matched "' + match + '" in file ' + file))
  .on('error', err => console.log('Error emitted: ' + err.message));
```

#### Propagating errors
It is always best practice to register a listener for the `error` event, as Node.js will treat it in a special way and will automatically throw an exception and exit from the program if no associated listener is found.

#### Making any object observable
by extending the `EventEmitter` class.
```js
const EventEmitter = require('events').EventEmitter;
const fs = require('fs');
class FindPattern extends EventEmitter {...}
```
This is a pretty common pattern in the Node.js ecosystem, for example, the `Server` object of the core HTTP module defines methods such as `listen()`, `close()`, `setTimeout()`, and internally it also inherits from the `EventEmitter` function, thus allowing it to produce events, such as `request`, when a new request is received; or `connection`, when a new connection is established; or `closed`, when the server is closed.

#### Synchronous and asynchronous events
It is crucial that we never mix the two approaches in the same `EventEmitter`, but even more important, when emitting the same event type, to avoid producing the same problems that we described in the *Unleashing Zalgo* section.

The main difference between emitting synchronous and asynchronous events lies in the way listeners can be registered. When the events are emitted asynchronously, the program has all the time to register new listeners even after `EventEmitter` is initialized, because the events are guaranteed not to be fired until the next cycle of the event loop: `findPattern()` function, it represents a common approach that is used in most Node.js modules.

On the contrary, emitting events synchronously requires that all the listeners are registered before the `EventEmitter` function starts to emit any event.
> it's very important to clearly highlight the behavior of our EventEmitter in its documentation to avoid confusion, and potentially incorrect usage.

#### EventEmitter versus callbacks
`callbacks` should be used when a result must be returned in an asynchronous way; `events` should instead be used when there is a need to communicate that something has just happened.
> a lot of confusion is generated from the fact that the two paradigms are, most of the time, equivalent and allow you to achieve the same results.

* callbacks have some limitations when it comes to supporting different types of events.
* EventEmitter might be preferable when the same event can occur multiple times, or not occur at all.
* callback is expected to be invoked exactly once, whether the operation is successful or not.
* using callbacks can notify only a particular callback, while using an EventEmitter function is possible for multiple listeners to receive the same notification.

#### Combining callbacks and EventEmitter
One example of this pattern is offered by the [node-glob module](https://npmjs.org/package/glob)
`glob(pattern, [options], callback) `
the function returns EventEmitter that provides a more fine-grained report over the state of the process.

```js
const glob = require('glob');
glob('data/*.txt', (error, files) => console.log(`All files found:
  ${JSON.stringify(files)}`))
  .on('match', match => console.log(`Match found: ${match}`));
```
 the practice of exposing a simple, clean, and minimal entry point while still providing more advanced or less important features with secondary means is quite common in Node.js

**Note: Pattern:**
Create a function that accepts a callback and returns EventEmitter, thus providing a simple and clear entry point for the main functionality, while emitting more fine-grained events using EventEmitter.

## Ch3 Asynchronous Control Flow Patterns with Callbacks
```js
```


## CH4 Asynchronous Control Flow Patterns with ES2015 and Beyond

## CH5 Coding with Streams

## CH6 Design Patterns

## CH7 Wiring Modules

## CH8 Universal JavaScript for Web Applications

## CH9 Advanced Asynchronous Recipes

## CH10 Scalability and Architectural Patterns

## CH11 Messaging and Integration Patterns
