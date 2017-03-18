# Using ES6 in Your Node.js Web Application
Jonathan Mills  
Javascript syntax, Function changes, node.js

## ECMAScript and Node.js
ECMA-262 : the standards that JavaScript is built on.
ES6 add a new set of features to the Ecma 262 Standards. Its up to each engine to support each feature.
[kangax](http://kangax.github.io/compat-table/es6/) show ECMAScript features adoption.
Node 6 is almost total adoption.

## Some Syntactic Sugar
#### Function Defaults
Simplifies the validation process.
``` js
function sum(x=0, y=0){
    return x+y;
}
console.log(sum(0)); //0
```
Enables the use of variables as well.
``` js
function product(x=0, y=x){
    return x*y;
}
console.log(product(2)); //4
```

#### Rest Parameters
the number of parameters pass to a function is not set.
`arguments` is an object with function arguments. with rest `...` we get an array from anything
``` js
function log(level, ...args){
    console.log(args); // ['obj: ', {a:'a', b:'b'}, 'that was my obj']
    if(level === 'debug'){
        console.log(args[0]); // obj:
    }
}
var obj = {a:'a', b:'b'}
log('debug', 'obj: ', obj, 'that was my obj');
```

#### Spread Operator
Changes an array to a list of CSV (comma separate values).
``` js
function log(level, ...args){
    if(level === 'debug'){
        console.log(args[0]); // [{bar:'baz'},{a:'a'}, {b:'b'}]
    }
}
var foo = {bar:'baz'}
var obj = [{a:'a'}, {b:'b'}]
obj = [foo, ...obj];
log('debug', obj);
```

#### Object Literal Shortcut Syntax
```js
function greetingService(name){
    var sayHi = function(){
        console.log('Hello '+ name + '!');
    }
    var sayBye = function(){
        console.log('Goodbye '+ name + '!');
    }
    return { sayHi, sayBye }
}
greetingService('Jon');
```

#### For Of
To iterate over an array.
``` js
function sum (first,...args) {
    var sum = first;
    for(num of args){
        sum = sum+num;
    }
    return sum;
}
console.log( sum(1,2,3,4,5)); //15
```

#### Template Literals
everything between backtick marks is what appears on the screen
``` js
function greetingService(name) {
    return {
        sayHi() {
            console.log(`Hello ${name}!`);
        }, 
        sayBye() {
            console.log(`Goodbye ${name}!`);
        }
    }
}
var gs= greetingService('jon');
gs.sayHi(); //Hello jon!
```

#### Destructuring
I take only what I need. I can pull peaces from an object.
``` js
function greetingService(name){
    var sayHi = function(){
        console.log('Hello '+ name + '!');
    }
    var sayBye = function(){
        console.log('Goodbye '+ name + '!');
    }
    return {
        sayHi,
        sayBye
    }
}
var {sayHi:hi} = greetingService('Jon');
hi();
```

## New Bindings
Variable changes, scoping issues (block scope).
#### Block Scope with Let
let is not hoisted, will return error instead of undefined.
``` js
console.log(x); // ReferenceError: x is not defined
var x = 1;
//more code.... 
for(let x = 0; x<10; x++){
    console.log(x); // 0,1,2,3,4,5,6,7,8,9
}
console.log('x = ' + x); // 1
```

#### Const
to create constants. Is block scope
``` js
const env = 'debug';
env = 'test'; // Error Assignment
if(true){
    const env = 'prod';
    console.log(env); //prod
}
console.log(env);// debug
```

## Function Changes
different function types:
* Arrow functions
* Lexical Bindings
* Classes (not what you are used to)

#### Arrow Functions
shortcut to write a function. The cool part is when leading with objects: the way that `this` is bound, are bound lexically. Only use in callbacks, replaces `ctrl = this`. Keep atention destructuring when function use `this`
```js
function GreetingService(name) {
    this.name = name;
    this.sayHi = () => {
        console.log(this); //Function Scope: GreetingService
        console.log(`Hello ${this.name}!`);
    };
    this.sayHiAsync = function () {
        setTimeout(function(){
            console.log(this); //Global Scope: Window
            console.log(`Hello ${this.name}!`);
        }, 1000)
    };
    this.sayHiAsyncArrow = function () {
        setTimeout(() =>{
            console.log(this); //Global Scope: check
            console.log(`Hello Arrow ${this.name}!`);
        }, 1000)
    };    
    this.sayBye = function () {
        console.log(`Goodbye ${this.name}!`);
    }
}


var gs = new GreetingService('jon');
gs.sayHi(); //Hello jon!
gs.sayBye(); //Goodbye jon!
gs.sayHiAsync(); //Hello !
gs.sayHiAsyncArrow(); //Hello Arrow jon! - Global Scope:GreetingService
var {sayHi,sayBye,sayHiAsync,sayHiAsyncArrow} = new GreetingService('Lucas');
sayHi(); // Hello Lucas!
sayBye(); // Goodbye !
sayHiAsync(); //Hello !
sayHiAsyncArrow(); //Hello Arrow ! - Global Scope:Window
```

#### Constructor Functions and Prototypes
constructors: first letter capital. Propotype is some kind of abstraction. All instances access to the same functions defined with prototypes: define 1 use *.
```js
function Task(name) {
    var called=0;
    this.name = name;
    this.toString = function() { //this will copy with every instance
        console.log(++called);
        var not='';
        if(!this.complete) {
            not = 'not'
        }
        return `${this.name} is ${not} complete`
    }
}
var task1 = new Task('module 1');
var task2 = new Task('module 2');
task1.complete = true
console.log(task1.toString());
console.log(task2.toString());
```

```js
function Task(name) {
    this.name = name;
    // issue1: the only way to interact with prototype is this
    // issue2: I cannot reference a private variable within prototype (called)
}
Task.prototype.toString = function() {
    var not='';
    if(!this.complete) {
        not = 'not'
    }
    return `${this.name} is ${not} complete`
}
var task1 = new Task('module 1');
var task2 = new Task('module 2');
task1.complete = true
console.log(task1.toString());
console.log(task2.toString());
```

#### Classes
In general are strip down versions of constructor functions. the prototype is accessible. At run time we are changing the classes because of prototype and inheritance. Classes do not work as OO programs in JS
``` js
class Task {
    constructor(name) {
        this.name = name;
    }
    toString() { //is not in the object, is in the prototype
        var not = '';
        if (!this.complete) {
            not = 'not'
        }
        return `${this.name} is ${not} complete`;
    }
}
var task1 = new Task('module 1');
var task2 = new Task('module 2');
task1.complete = true;
console.log(task1.toString());
console.log(task2.toString());
```

#### Extends and Super
Are cool things to do with classes. Clean implementation of inheritance
``` js
class Task {
    constructor(name) {
        this.name = name;
    }
    toString() { 
        var not = '';
        if (!this.complete) {
            not = 'not'
        }
        return `${this.name} is ${not} complete`;
    }
}
class Urgent extends Task{
    constructor(name){
        super('!' + name + '!');
    }
    toString(){
        return 'urgent task ' + super.toString()
    }
}
var task1 = new Task('module 1');
var task2 = new Urgent('module 2');
task1.complete = true;
console.log(task1.toString());
console.log(task2.toString());
```

#### Generators
Functions with multiple end points
```js
function* gen(input){
    var netIn= yield(input)
    //do stuff
    yield(netIn)
}
var it = gen('Init');
console.log(it.next()); //{value: "Init", done: false}
console.log(it.next('NextInput')); //{value: "NextInput", done: false}
console.log(it.next()); //{value: undefined, done: true}
```
I can take a repository and control the flow: interrupt ejecution flow and get feedback.
```js
function* bookRepo(num) {
    var list = [
        {title: 'War and Peace'}, 
        {title: 'Wind in the Willows'}, 
        { title: 'That Potter Book'}, 
        {title: 'The other one too... '}, 
        {title: 'A Book about Rings'}, 
        {title: 'A Second Book About Rings'}, 
        {title: 'The Last Rings Book....'}
    ];
    var out = [];
    for (item of list) {
        out.push(item);
        if (out.length >= num) {
            num = yield out;
            out = [];
        }
    }
    yield out;
}

var repo = bookRepo(2);
console.log(repo.next()); //{value: [{title: 'War and Peace'},
//{title: 'Wind in the Willows'}], done: false}
console.log(repo.next(4));// {value: Array[4], done: false}
console.log(repo.next(4)); // {value: [{title: 'The Last Rings Book....'}], done: false}
```

## Built-ins
Sets, Maps, Promises.

#### Sets and WeakSets
Sets: order list with unique items. WeakSets: allows garbage collection
```js
var items = new Set();
var x = {foo:'4'};
var y = {bar:'3'};
items.add(x);
items.add(y);

if(true){
    let x = {baz:123};
    items.add(x);
    console.log(items.has(x));
}
for(let item of items){
    console.log(item);
}
items.clear();
console.log(items);
```

#### Map and WeakMap
Maps are dictionaries, Map is ordered. On a WeakMap you can not iterate over. WeakMap keys are not enumerable
```js
var person = new Map([['town', 'kc'], ['kids',3]]);
person.set('name','jon')
person.set('age','old');
person.set('single',false);
console.log(person.size);
console.log(person.get('age'));
if(person.has('single')){
    console.log('easier to check for this...');
}
person.forEach(function(value, key, map) {
    console.log(key, value);
})
```

#### Promises
issue with separation of concerns: errors and results are processed in one function. 
```js
function asyncMethod(message) {
    return new Promise(function (fulfill, reject) {
        setTimeout(function () {
            console.log(message);
            reject('error');
        }, 500)

    })
}
function findUser() {
    return asyncMethod('Find User')
}
function validateUser() {
    return asyncMethod('validate User')
}
function doStuff() {
    return asyncMethod('do stuff')
}
function error(err){
  throw(err);
}
function lastError(err){
  console.log(`we had an error: ${err}`);
}
asyncMethod('Open DB Connection')
    .then(findUser, error)
    .then(validateUser, error)
    .then(doStuff, error)
    .then(function () {}, lastError)
```

