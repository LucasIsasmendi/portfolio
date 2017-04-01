# Node.js Dessign Patterns - second edition
[code repository](https://github.com/PacktPublishing/Node.js_Design_Patterns_Second_Edition_Code)

## Index
[Chapter 1: Welcome to the Node.js Platform](#ch1-welcome-to-the-node.js-platform)  
[Chapter 2: Node.js Essential Patterns](#ch2-node.js-essential-patterns)

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
``` js
```

``` js
```

``` js
```

``` js
```
## CH3 Asynchronous Control Flow Patterns with Callbacks

## CH4 Asynchronous Control Flow Patterns with ES2015 and Beyond

## CH5 Coding with Streams

## CH6 Design Patterns

## CH7 Wiring Modules

## CH8 Universal JavaScript for Web Applications

## CH9 Advanced Asynchronous Recipes

## CH10 Scalability and Architectural Patterns

## CH11 Messaging and Integration Patterns
