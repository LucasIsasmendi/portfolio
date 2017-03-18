# Types & Grammar

## CH1 Types
Having a proper understanding of each *type* and its intrinsic behavior is absolutely essential to understanding how to properly and accurately convert values to different types (see Coercion, Chapter 4). Nearly every JS program ever written will need to handle value coercion in some shape or form, so it's important you do so responsibly and with confidence.

### Built-in Types
JavaScript defines seven built-in types:

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`
* `symbol` -- added in ES6!

**Note:** All of these types except `object` are called "primitives".  
The `typeof` operator inspects the type of the given value, and always returns one of seven string values -- surprisingly, there's not an exact 1-to-1 match with the seven built-in types we just listed.  
`null` is buggy when combined with the `typeof` operator.

If you want to test for a `null` value using its type, you need a compound condition:
```js
var a = null;
(!a && typeof a === "object"); // true
```

### Values as Types
In JavaScript, variables don't have types -- **values have types**. Variables can hold any value, at any time.

If you use `typeof` against a variable, it's not asking "what's the type of the variable?" as it may seem, since JS variables have no types. Instead, it's asking "what's the type of the value *in* the variable?"

### `undefined` vs "undeclared"
Variables that have no value *currently*, actually have the `undefined` value. Calling `typeof` against such variables will return `"undefined"`:
An "undefined" variable is one that has been declared in the accessible scope, but *at the moment* has no other value in it. By contrast, an "undeclared" variable is one that has not been formally declared in the accessible scope.

### `typeof` Undeclared
The `typeof` operator returns `"undefined"` even for "undeclared" (or "not defined") variables. This safety guard is a useful feature when dealing with JavaScript in the browser, where multiple script files can load variables into the shared global namespace.
Example: DEBUG = true mode flag variable.
```js
// oops, this would throw an error!
if (DEBUG) {
	console.log( "Debugging is starting" );
}

// this is a safe existence check
if (typeof DEBUG !== "undefined") {
	console.log( "Debugging is starting" );
}
```
## CH2 Values
### Arrays
are just containers for any type of value.
**Warning:** Using `delete` on an `array` value will remove that slot from the `array`, but even if you remove the final element, it does **not** update the `length` property, so be careful! We'll cover the `delete` operator itself in more detail in Chapter 5.

Be careful about creating "sparse" `array`s (leaving or creating empty/missing slots):
```js
var a = [ ];
a[0] = 1;
// no `a[1]` slot set here
a[2] = [ 3 ];
a[1];		// undefined
a.length;	// 3
```
### Strings
JavaScript `string`s are immutable, while `array`s are quite mutable.
A further consequence of immutable `string`s is that none of the `string` methods that alter its contents can modify in-place, but rather must create and return new `string`s. By contrast, many of the methods that change `array` contents actually *do* modify in-place.
Also, many of the `array` methods that could be helpful when dealing with `string`s are not actually available for them, but we can "borrow" non-mutation `array` methods against our `string`
```js
var a = "foo";
a.join;			// undefined
a.map;			// undefined
var c = Array.prototype.join.call( a, "-" );
var d = Array.prototype.map.call( a, function(v){
	return v.toUpperCase() + ".";
} ).join( "" );
c;				// "f-o-o"
d;				// "F.O.O."
```
Reverse Example

### Numbers
Because `number` values can be boxed with the `Number` object wrapper (see Chapter 3), `number` values can access methods that are built into the `Number.prototype` (see Chapter 3).  
For example, the `toFixed(..)` method allows you to specify how many fractional decimal places you'd like the value to be represented with. `toPrecision(..)` is similar, but specifies how many *significant digits* should be used to represent the value.  
You don't have to use a variable with the value in it to access these methods; you can access these methods directly on `number` literals. But you have to be careful with the `.` operator. Since `.` is a valid numeric character, it will first be interpreted as part of the `number` literal, if possible, instead of being interpreted as a property accessor.

#### Numeric Syntax
```js
// invalid syntax:
42.toFixed( 3 );	// SyntaxError
// these are all valid:
(42).toFixed( 3 );	// "42.000"
0.42.toFixed( 3 );	// "0.420"
42..toFixed( 3 );	// "42.000"
```
`number` literals can also be expressed in other bases, like binary, octal, and hexadecimal.
As of ES6, the following new forms are also valid:
```js
0o363;		// octal for: 243
0O363;		// ditto: AVOID THIS, is confused
0b11110011;	// binary for: 243
0B11110011; // ditto : AVOID THIS, is confused
```
#### Small Decimal Values
The most (in)famous side effect of using binary floating-point numbers (which, remember, is true of **all** languages that use IEEE 754 -- not *just* JavaScript as many assume/pretend) is:

```js
0.1 + 0.2 === 0.3; // false
```

As of ES6, `Number.EPSILON` is predefined with this tolerance value, so you'd want to use it, but you can safely polyfill the definition for pre-ES6:
```js
if (!Number.EPSILON) {
	Number.EPSILON = Math.pow(2,-52);
}
```
We can use this `Number.EPSILON` to compare two `number`s for "equality" (within the rounding error tolerance)
```js
function numbersCloseEnoughToEqual(n1,n2) {
	return Math.abs( n1 - n2 ) < Number.EPSILON;
}
var a = 0.1 + 0.2;
var b = 0.3;
numbersCloseEnoughToEqual( a, b );					// true
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 );	// false
```
#### Safe Integer Ranges
The maximum integer that can "safely" be represented (that is, there's a guarantee that the requested value is actually representable unambiguously) is `2^53 - 1`, which is `9007199254740991`.
This value is actually automatically predefined in ES6, as `Number.MAX_SAFE_INTEGER`. Unsurprisingly, there's a minimum value, `-9007199254740991`, and it's defined in ES6 as `Number.MIN_SAFE_INTEGER`.

#### Testing for Integers
To test if a value is an integer, you can use the ES6-specified `Number.isInteger(..)`  
To test if a value is a *safe integer*, use the ES6-specified `Number.isSafeInteger(..)`  

#### 32-bit (Signed) Integers
While integers can range up to roughly 9 quadrillion safely (53 bits), there are some numeric operations (like the bitwise operators) that are only defined for 32-bit `number`s, so the "safe range" for `number`s used in that way must be much smaller.
The range then is `Math.pow(-2,31)` (`-2147483648`, about -2.1 billion) up to `Math.pow(2,31)-1` (`2147483647`, about +2.1 billion).

### Special Values
#### The Non-value Values
* `null` is an empty value
* `null` had a value and doesn't anymore
* `undefined` is a missing value
* `undefined` hasn't had a value yet  
`null` is a special keyword, not an identifier, and thus you cannot treat it as a variable to assign to (why would you!?). However, `undefined` *is* (unfortunately) an identifier, this mean that we can (avoit it) assign a value.

### Special Numbers
#### The Not Number, Number
Any mathematic operation you perform without both operands being `number`s (or values that can be interpreted as regular `number`s in base 10 or base 16) will result in the operation failing to produce a valid `number`, in which case you will get the `NaN` value. "the type of not-a-number is 'number'!". `NaN !== NaN`.
In ES6 we have `Number.isNaN(..)` to check values.

#### Infinities
1/ 0 = `Number.POSITIVE_INFINITY`. -1/0 = `Number.NEGATIVE_INFINITY`
#### Zeros
JavaScript has both a normal zero `0` (otherwise known as a positive zero `+0`) *and* a negative zero `-0`.
```js
function isNegZero(n) {
	n = Number( n );
	return (n === 0) && (1 / n === -Infinity);
}

isNegZero( -0 );		// true
isNegZero( 0 / -3 );	// true
isNegZero( 0 );			// false
```
### Special Equality
As we saw above, the `NaN` value and the `-0` value have special behavior when it comes to equality comparison.
As of ES6, there's a new utility that can be used to test two values for absolute equality, without any of these exceptions. It's called `Object.is(..)`:
```js
var a = 2 / "foo";
var b = -3 * 0;
Object.is( a, NaN );	// true
Object.is( b, -0 );		// true
Object.is( b, 0 );		// false
```
### Value vs. Reference
In many other languages, values can either be assigned/passed by value-copy or by reference-copy depending on the syntax you use.
In JavaScript, there are no pointers, and references work a bit differently. You cannot have a reference from one JS variable to another variable. That's just not possible.
```js
var a = 2;
var b = a; // `b` is always a copy of the value in `a`
b++;
a; // 2
b; // 3

var c = [1,2,3];
var d = c; // `d` is a reference to the shared `[1,2,3]` value
d.push( 4 );
c; // [1,2,3,4]
d; // [1,2,3,4]
```
Simple values (aka scalar primitives) are *always* assigned/passed by value-copy: `null`, `undefined`, `string`, `number`, `boolean`, and ES6's `symbol`.

Compound values -- `object`s (including `array`s, and all boxed object wrappers -- see Chapter 3) and `function`s -- *always* create a copy of the reference on assignment or passing.
```js
function foo(x) {
	x.push( 4 );
	x; // [1,2,3,4]
	// later
	x = [4,5,6];
	x.push( 7 );
	x; // [4,5,6,7]
}
var a = [1,2,3];
foo( a );
a; // [1,2,3,4]  not  [4,5,6,7]
```
To accomplish changing `a` to have the `[4,5,6,7]` value contents, you can't create a new `array` and assign -- you must modify the existing `array` value:

```js
function foo(x) {
	x.push( 4 );
	x; // [1,2,3,4]
	// later
	x.length = 0; // empty existing array in-place
	x.push( 4, 5, 6, 7 );
	x; // [4,5,6,7]
}
var a = [1,2,3];
foo( a );
a; // [4,5,6,7]  not  [1,2,3,4]
```
Remember: you cannot directly control/override value-copy vs. reference -- those semantics are controlled entirely by the type of the underlying value.

To pass a scalar primitive value in a way where its value updates can be seen, kinda like a reference -- you have to wrap the value in another compound value (`object`, `array`, etc) that *can* be passed by reference-copy:
```js
function foo(wrapper) {
	wrapper.a = 42;
}
var obj = {
	a: 2
};
foo( obj );
obj.a; // 42
```
## CH3 Natives



## CH4 Coercion



## CH5 Grammar



## Appendix A: Mixed Environment JavaScript
