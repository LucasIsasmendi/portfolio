# ES6 & Beyond
## CH1 ES? Now & Feature
Unlike ES5, ES6 is not just a modest set of new APIs added to the language. It incorporates a whole slew of new syntactic forms, some of which may take quite a bit of getting used to. There's also a variety of new organization forms and new API helpers for various data types.

#### Transpiling
*transpiling* (transformation + compiling). Roughly, the idea is to use a special tool to transform your ES6 code into equivalent (or close!) matches that work in ES5 environments.

#### Shims/Polyfills
Not all new ES6 features need a transpiler. Polyfills (aka shims) are a pattern for defining equivalent behavior from a newer environment into an older environment, when possible. Syntax cannot be polyfilled, but APIs often can be.  
There's a great collection of ES6 shims called "ES6 Shim" (https://github.com/paulmillr/es6-shim/) that you should definitely adopt as a standard part of any new JS project!

It is assumed that JS will continue to evolve constantly, with browsers rolling out support for features continually rather than in large chunks. So the best strategy for keeping updated as it evolves is to just introduce polyfill shims into your code base, and a transpiler step into your build workflow, right now and get used to that new reality.

## CH2 Syntax
ES6Fiddle (http://www.es6fiddle.net/) is a great, easy-to-use playground for trying out ES6, as is the online REPL for the Babel transpiler (http://babeljs.io/repl/)

### Block-Scoped Declarations
 the fundamental unit of variable scoping in JavaScript has always been the `function`. If you needed to create a block of scope, the most prevalent way to do so other than a regular function declaration was the immediately invoked function expression (IIFE).

#### `let` Declarations
we can now create declarations that are bound to any block: *block scoping*. This means all we need is a pair of `{ .. }` to create a scope. Instead of using `var`, which always declares variables attached to the enclosing function (or global, if top level) scope, use `let`:

```js
var a = 2;
{   let a = 3;
    console.log( a );   // 3
}
console.log( a );       // 2
```
`let` declarations attach to the block scope but are not initialized until they appear in the block. Accessing a `let`-declared variable earlier than its `let ..` declaration/initialization causes an error, whereas with `var` declarations the ordering doesn't matter (except stylistically).
```js
{
    console.log( a );   // undefined
    console.log( b );   // ReferenceError!
    var a;
    let b;
}
```
**Warning:** This `ReferenceError` from accessing too-early `let`-declared references is technically called a *Temporal Dead Zone (TDZ)* error -- you're accessing a variable that's been declared but not yet initialized.

##### `let` + `for`
```js
var funcs = [];
for (let i = 0; i < 5; i++) {
    funcs.push( function(){
        console.log( i );
    } );
}
funcs[3]();     // 3
```
The `let i` in the `for` header declares an `i` not just for the `for` loop itself, but it redeclares a new `i` for each iteration of the loop. That means that closures created inside the loop iteration close over those per-iteration variables the way you'd expect.
If you tried that same snippet but with `var i` in the `for` loop header, you'd get `5` instead of `3`, because there'd only be one `i` in the outer scope that was closed over, instead of a new `i` for each iteration's function to close over.

#### `const` Declarations
`const` is a variable that's read-only after its initial value is set. 
You are not allowed to change the value the variable holds once it's been set, at declaration time.  
 The value is not frozen or immutable because of `const`, just the assignment of it. If the value is complex, such as an object or array, the contents of the value can still be modified:

```js
{
    const a = [1,2,3];
    a.push( 4 );
    console.log( a );       // [1,2,3,4]
    a = 42;                 // TypeError!
}
```
**Warning:** Assigning an object or array as a constant means that value will not be able to be garbage collected until that constant's lexical scope goes away, as the reference to the value can never be unset.

##### `const` Or Not
To avoid potentially confusing code, only use `const` for variables that you're intentionally and obviously signaling will not change. In other words, don't *rely on* `const` for code behavior, but instead use it as a tool for signaling intent, when intent can be signaled clearly.

#### Block-scoped Functions
Starting with ES6, function declarations that occur inside of blocks are now specified to be scoped to that block.
```js
{
    foo();                  // works!
    function foo() {        // "hoisted"
        // ..
    }
}
foo();                      // ReferenceError
```

Block-scoping of function declarations could be a problem if you've ever written code like this before, and relied on the old legacy non-block-scoped behavior:
```js
if (something) {
    function foo() {
        console.log( "1" );
    }
}
else {
    function foo() {
        console.log( "2" );
    }
}
foo();      // ??
```

In pre-ES6 environments, `foo()` would print `"2"` regardless of the value of `something`, because both function declarations were hoisted out of the blocks, and the second one always wins.
In ES6, that last line throws a `ReferenceError`.

### Spread/Rest
```js
function foo(x,y,z) {
    console.log( x, y, z );
}
foo( ...[1,2,3] );              // 1 2 3
```
When `...` is used in front of an array (or any *iterable*), it acts to "spread" it out into its individual values.


The other common usage of `...` can be seen as essentially the opposite; instead of spreading a value out, the `...` *gathers* a set of values together into an array. Consider:
```js
function foo(x, y, ...z) {
    console.log( x, y, z );
}
foo( 1, 2, 3, 4, 5 );           // 1 2 [3,4,5]
```

### Default Parameter Values
```js
function foo(x = 11, y = 31) {
    console.log( x + y );
}

foo();                  // 42
foo( 5, 6 );            // 11
foo( 0, 42 );           // 42
foo( 5 );               // 36
foo( 5, undefined );    // 36 <-- `undefined` is missing
foo( 5, null );         // 5  <-- null coerces to `0`
foo( undefined, 6 );    // 17 <-- `undefined` is missing
foo( null, 6 );         // 6  <-- null coerces to `0`
```
**Note:** A rest/gather parameter (see "Spread/Rest") cannot have a default value.

#### Default Value Expressions

Function default values can be more than just simple values like `31`; they can be any valid expression, even a function call:
```js
function bar(val) {
    console.log( "bar called!" );
    return y + val;
}
function foo(x = y + 3, z = bar( x )) {
    console.log( x, z );
}
var y = 5;
foo();                              // "bar called"
                                    // 8 13
foo( 10 );                          // "bar called"
                                    // 10 15
y = 6;
foo( undefined, 10 );               // 9 10
```
As you can see, the default value expressions are lazily evaluated, meaning they're only run if and when they're needed -- that is, when a parameter's argument is omitted or is `undefined`.
It's a subtle detail, but the formal parameters in a function declaration are in their own scope, not in the function body's scope. That means a reference to an identifier in a default value expression first matches the formal parameters' scope before looking to an outer scope.

### Destructuring
```js
function foo() {
    return [1,2,3];
}
function bar() {
    return {x: 4, y: 5, z: 6};
}
var [ a, b, c ] = foo();
var { x: x, y: y, z: z } = bar();

console.log( a, b, c );             // 1 2 3
console.log( x, y, z );             // 4 5 6
```
You're likely more accustomed to seeing syntax like `[a,b,c]` on the righthand side of an `=` assignment, as the value being assigned.
Destructuring symmetrically flips that pattern, so that `[a,b,c]` on the lefthand side of the `=` assignment is treated as a kind of "pattern" for decomposing the righthand side array value into separate variable assignments.
Similarly, `{ x: x, y: y, z: z }` specifies a "pattern" to decompose the object value from `bar()` into separate variable assignments.

#### Object Property Assignment Pattern

## CH3 Organization

## CH4 Async Flow Control

## CH5 Collections

## CH6 API Additions