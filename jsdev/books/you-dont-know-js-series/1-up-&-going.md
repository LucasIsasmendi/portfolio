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



## Ch3 Into YDKJS
