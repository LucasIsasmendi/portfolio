#2. Scope & Closures

- [Chapter 1: What is Scope?](#ch1-what-is-scope)
- [Chapter 2: Lexical Scope](#ch2-lexical-scope)
- [Chapter 3: Function vs. Block Scope](#ch3-function-vs.-block-scope)
- [Chapter 4: Hoisting](#ch4-hoisting)
- [Chapter 5: Scope Closures](#ch5-scope-closures)
- [Appendix A: Dynamic Scope](#app-a-dynamic-scope)
- [Appendix B: Polyfilling Block Scope](#app-b-polyfilling-block-scope)
- [Appendix C: Lexical-this](#app-c-lexical-this)

## Ch1 What is Scope?
### Compiler Theory
Despite the fact that JavaScript falls under the general category of "dynamic" or "interpreted" languages, it is in fact a compiled language.

In traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps *before* it is executed, roughly called "compilation":
1. **Tokenizing/Lexing:** breaking up a string of characters into meaningful (to the language) chunks, called tokens.
2. **Parsing:** taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program: "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree).
3. **Code-Generation:** the process of taking an AST and turning it into executable code.

For JavaScript, the compilation that occurs happens, in many cases, mere microseconds (or less!) before the code is executed. To ensure the fastest performance, JS engines use all kinds of tricks (like JITs, which lazy compile and even hot re-compile, etc.)

### Understanding Scope
1. **Engine**: responsible for start-to-finish compilation and execution of our JavaScript program.

2. **Compiler**: handles all the dirty work of parsing and code-generation (see previous section).

3. **Scope**: collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

When you see the program `var a = 2;`, **Engine** sees two distinct statements, one which **Compiler** will handle during compilation, and one which **Engine** will handle during execution.


Scope is the set of rules that determines where and how a variable (identifier) can be looked-up. This look-up may be for the purposes of assigning to the variable, which is an LHS (left-hand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (right-hand-side) reference.

LHS references result from assignment operations. *Scope*-related assignments can occur either with the `=` operator or by passing arguments to (assign to) function parameters.

The JavaScript *Engine* first compiles code before it executes, and in so doing, it splits up statements like `var a = 2;` into two separate steps:

1. First, `var a` to declare it in that *Scope*. This is performed at the beginning, before code execution.

2. Later, `a = 2` to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing *Scope*, and if need be (that is, they don't find what they're looking for there), they work their way up the nested *Scope*, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don't.

Unfulfilled RHS references result in `ReferenceError`s being thrown. Unfulfilled LHS references result in an automatic, implicitly-created global of that name (if not in "Strict Mode" [note-strictmode]), or a `ReferenceError` (if in "Strict Mode" [note-strictmode]).

[note-strictmode]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode

## Ch2 Lexical Scope


## Ch3 Function vs. Block Scope

## Ch4 Hoisting

## Ch5 Scope Closures

## App A Dynamic Scope

## App B Polyfilling Block Scope

## App C Lexical this
