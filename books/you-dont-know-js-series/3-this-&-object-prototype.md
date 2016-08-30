## Appendix A: ES6 `class`
If there's any take-away message from the second half of this book (Chapters 4-6), it's that classes are an optional design pattern for code (not a necessary given), and that furthermore they are often quite awkward to implement in a `[[Prototype]]` language like JavaScript.
This awkwardness is not just about syntax, although that's a big part of it. Chapters 4 and 5 examined quite a bit of syntactic ugliness, from verbosity of .prototype references cluttering the code, to explicit pseudo-polymorphism (see Chapter 4) when you give methods the same name at different levels of the chain and try to implement a polymorphic reference from a lower-level method to a higher-level method. .constructor being wrongly interpreted as "was constructed by" and yet being unreliable for that definition is yet another syntactic ugly.

But the problems with class design are much deeper. Chapter 4 points out that classes in traditional class-oriented languages actually produce a copy action from parent to child to instance, whereas in `[[Prototype]]`, the action is not a copy, but rather the opposite -- a delegation link.

When compared to the simplicity of OLOO-style code and behavior delegation (see Chapter 6), which embrace `[[Prototype]]` rather than hide from it, classes stand out as a sore thumb in JS.

### `class` Gotchas
Firstly, the class syntax may convince you a new "class" mechanism exists in JS as of ES6. Not so. class is, mostly, just syntactic sugar on top of the existing `[[Prototype]]` (delegation!) mechanism.

That means class is not actually copying definitions statically at declaration time the way it does in traditional class-oriented languages.
For performance reasons (this binding is already expensive), `super` is not bound dynamically. It's bound sort of "statically", as declaration time.

You're used to being able to assign around methods to different objects to automatically take advantage of the dynamism of `this` via the implicit binding rule (see Chapter 2). But the same will likely not be true with methods that use `super`.

### Static > Dynamic?
In traditional class-oriented languages, you never adjust the definition of a class later, so the class design pattern doesn't suggest such capabilities. But **one of the most powerful parts** of JS is that it *is* dynamic, and the definition of any object is (unless you make it immutable) a fluid and mutable *thing*.


### Review (TL;DR)

`class` does a very good job of pretending to fix the problems with the class/inheritance design pattern in JS. But it actually does the opposite: **it hides many of the problems, and introduces other subtle but dangerous ones**.

`class` contributes to the ongoing confusion of "class" in JavaScript which has plagued the language for nearly two decades. In some respects, it asks more questions than it answers, and it feels in totality like a very unnatural fit on top of the elegant simplicity of the `[[Prototype]]` mechanism.

Bottom line: if ES6 `class` makes it harder to robustly leverage `[[Prototype]]`, and hides the most important nature of the JS object mechanism -- **the live delegation links between objects** -- shouldn't we see `class` as creating more troubles than it solves, and just relegate it to an anti-pattern?
