# 1. Up & Going

- [Chapter 1: Into Programming](#ch1-intro-programming)
- [Chapter 2: Into JavaScript](#ch2-intro-javascript)
- [Chapter 3: Into YDKJS](#ch3-intro-ydkjs)

## Ch1 Intro Programming

### Code
#### Statements
In a computer language, a group of words, numbers, and operators that performs a specific task is a *statement*. In JavaScript, a statement might look as follows:
```js
a = b * 2;
```

#### Expressions
Statements are made up of one or more *expressions*. An expression is any reference to a variable or value, or a set of variable(s) and value(s) combined with operators.

* `2` is a *literal value expression*
* `b` is a *variable expression*, which means to retrieve its current value
* `b * 2` is an *arithmetic expression*, which means to do the multiplication
* `a = b * 2` is an *assignment expression*, which means to assign the result of the `b * 2` expression to the variable `a` (more on assignments later)

A general expression that stands alone is also called an *expression statement*, such as the following:
```js
b * 2;
```
It's typically asserted that JavaScript is **interpreted**, because your JavaScript source code is processed each time it's run. But that's not entirely accurate. The JavaScript engine actually **compiles** the program **on the fly** and then immediately runs the compiled code.

#### Converting Between Types
But a controversial topic is what happens when you try to compare two values that are not already of the same type, which would require **implicit  coercion**.  
So if you use the `==` loose equals operator to make the comparison `"99.99" == 99.99`, JavaScript will convert the left-hand side `"99.99"` to its `number` equivalent `99.99`. The comparison then becomes `99.99 == 99.99`, which is of course `true`.

### Variables
JavaScript uses **dynamic typing**, meaning variables can hold values of any **type** without any **type** enforcement.

The newest version of JavaScript at the time of this writing (commonly called "ES6") includes a new way to declare *constants*, by using `const` instead of `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;
var amount = 99.99;
// ..
```
If you tried to assign any different value to `TAX_RATE` after that first declaration, your program would reject the change (and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2).

### Blocks
a block statement does not need a semicolon (;) to conclude it.

### Functions
Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.

### Scope
Scope (technically called **lexical scope**) is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's scoped variables.  
The same variable name could appear in different scopes.  
A scope can be nested inside another scope

### Practice
There is absolutely no substitute for practice in learning programming. No amount of articulate writing on my part is alone going to make you a programmer.

## Ch2 Into JavaScript

### Values & Types
JavaScript has typed values, not typed variables. The following built-in types are available:
* `string`
* `number`
* `boolean`
* `null` and `undefined`
* `object`
* `symbol` (new to ES6)

JavaScript provides a `typeof` operator that can examine a value and tell you what type it is

#### Objects
The `object` type refers to a compound value where you can set properties (named locations) that each hold their own values of any type. This is perhaps one of the most useful value types in all of JavaScript.

##### Arrays

An array is an `object` that holds values (of any type) not particularly in named properties/keys, but rather in numerically indexed positions.

The best and most natural approach is to use arrays for numerically positioned values and use `object`s for named properties.

##### Functions
subtype of `objects`

#### Built-In Type Methods
The built-in types and subtypes we've just discussed have behaviors exposed as properties and methods that are quite powerful and useful.
```js
var a = "hello world";
var b = 3.14159;
a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```

#### Comparing Values
##### Coercion: *explicit* and *implicit*
Here's an example of **explicit** coercion:
```js
var a = "42";
var b = Number( a );
a;				// "42"
b;				// 42 -- the number!
```
And here's an example of **implicit** coercion:
```js
var a = "42";
var b = a * 1;	// "42" implicitly coerced to 42 here
a;				// "42"
b;				// 42 -- the number!
```

##### Truthy & Falsy
In Chapter 1, we briefly mentioned the "truthy" and "falsy" nature of values: when a non-`boolean` value is coerced to a `boolean`, does it become `true` or `false`, respectively?

The specific list of "falsy" values in JavaScript is as follows:

* `""` (empty string)
* `0`, `-0`, `NaN` (invalid `number`)
* `null`, `undefined`
* `false`

Any value that's not on this "falsy" list is "truthy." Here are some examples of those:

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)

It's important to remember that a non-`boolean` value only follows this "truthy"/"falsy" coercion if it's actually coerced to a `boolean`. It's not all that difficult to confuse yourself with a situation that seems like it's coercing a value to a `boolean` when it's not.

##### Equality
The difference between `==` and `===` is usually characterized that `==` checks for value equality and `===` checks for both value and type equality.
However, this is inaccurate. The proper way to characterize them is that `==` checks for value equality with coercion allowed, and `===` checks for value equality without allowing coercion; `===` is often called "strict equality" for this reason.
```js
var a = "42";
var b = 42;
a == b;			// true
a === b;		// false
```
[ES6 7.2.12 Abstract Equality Comparison](http://www.ecma-international.org/ecma-262/6.0/index.html#sec-abstract-equality-comparison) to see the exact rules.

My simple rules:
* If either value (aka side) in a comparison could be the `true` or `false` value, avoid `==` and use `===`.
* If either value in a comparison could be of these specific values (`0`, `""`, or `[]` -- empty array), avoid `==` and use `===`.
* In *all* other cases, you're safe to use `==`. Not only is it safe, but in many cases it simplifies your code in a way that improves readability.

If you can be certain about the values, and `==` is safe, use it! If you can't be certain about the values, use `===`. It's that simple.


The `!=` non-equality form pairs with `==`, and the `!==` form pairs with `===`. All the rules and observations we just discussed hold symmetrically for these non-equality comparisons.


You should take special note of the `==` and `===` comparison rules if you're comparing two non-primitive values, like `object`s (including `function` and `array`). Because those values are actually held by reference, both `==` and `===` comparisons will simply check whether the references match, not anything about the underlying values.
```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

##### Inequality

The `<`, `>`, `<=`, and `>=` operators are used for inequality, referred to in the specification as "relational comparison." Typically they will be used with ordinally comparable values like `number`s. It's easy to understand that `3 < 4`.

But JavaScript `string` values can also be compared for inequality, using typical alphabetic rules (`"bar" < "foo"`).

What about coercion? Similar rules as `==` comparison (though not exactly identical!) apply to the inequality operators. Notably, there are no "strict inequality" operators that would disallow coercion the same way `===` "strict equality" does.

### Variables
variable names (including function names) must be valid *identifiers*.
An identifier must start with `a`-`z`, `A`-`Z`, `$`, or `_`. It can then contain any of those characters plus the numerals `0`-`9`. Certain words cannot be used as variables.

**Note:** For more information about reserved words, see Appendix A of the *Types & Grammar* title of this series.

#### Function Scopes
##### Hoisting
when a `var` declaration is conceptually "moved" to the top of its enclosing scope.
> It's much more common and accepted to use *hoisted* function declarations

##### Nested Scopes
When you declare a variable, it is available anywhere in that scope, as well as any lower/inner scopes.

If you try to access a variable's value in a scope where it's not available, you'll get a `ReferenceError` thrown. If you try to set a variable that hasn't been declared, you'll either end up creating a variable in the top-level global scope (*bad practice. Don't do it! Always formally declare your variables*) or getting an error, depending on "strict mode".

ES6 *lets* you declare variables to belong to individual blocks (pairs of `{ .. }`), using the `let` keyword.

### Conditionals
* `if`
* `if..else..if`
* `switch`
* `ternary|conditional operator`: condition ? true: false
```js
var a = 42;
var b = (a > 41) ? "hello" : "world";
// similar to:
// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

### Strict Mode
`use strict` keeps the code to a safer and more appropriate set of guidelines. Makes your code generally more optimizable by the engine.

You can opt in to strict mode for an individual function, or an entire file.

If strict mode causes issues in your program, you have things in your program you should fix.

### Functions As Values
```js
// Anonymous function expressions
var foo = function() {
	// ..
};
// Named function expressions
var x = function bar(){
	// ..
};
```
**Named function expressions** are generally more preferable, though **anonymous function expressions** are still extremely common.

#### Immediately Invoked Function Expressions (IIFEs)
Aldo `foo()` or `x()`, there's another way to execute a function expression, which is typically referred to as an *immediately invoked function expression* (IIFE):

```js
(function IIFE(){
	console.log( "Hello!" );
})(); //() executes the function
// "Hello!"
```
Can return a value, used to declare variables that won't affect the surrounding code outside the IIFE.

#### Closure
It will be one of the most important techniques in your JS skillset.

You can think of closure as a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.

##### Modules
The most common usage of closure in JavaScript is the module pattern. Modules let you define private implementation details (variables, functions) that are hidden from the outside world, as well as a public API that *is* accessible from the outside.
```js
function User(){
	var username, password;
	function doLogin(user,pw) {
		username = user;
		password = pw;
		// do the rest of the login work
	}
	var publicAPI = {
		login: doLogin
	};
	return publicAPI;
}
// create a `User` module instance
var fred = User();
fred.login( "fred", "12Battery34!" );
```
**Warning:** We are not calling `new User()` here, on purpose, despite the fact that probably seems more common to most readers. `User()` is just a function, not a class to be instantiated, so it's just called normally. Using `new` would be inappropriate and actually waste resources.

Executing `User()` creates an *instance* of the `User` module -- a whole new scope is created, and thus a whole new copy of each of these inner variables/functions. We assign this instance to `fred`. If we run `User()` again, we'd get a new instance entirely separate from `fred`.

### `this` Identifier
If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

It's important to realize that `this` *does not* refer to the function itself, as is the most common misconception.

### Prototypes
When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

But a more natural way of applying prototypes is a pattern called **"behavior delegation"**, where you intentionally design your linked objects to be able to *delegate* from one to the other for parts of the needed behavior.

### Old & New
There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

#### Polyfilling
used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

#### Transpiling (transforming + compiling)
There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.
So the better option is to use a tool that converts your newer code into older code equivalents: Babel, Traceur

## Ch3 Into YDKJS
