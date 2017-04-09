# Research NodeJS

## Various

### Versus

#### process.nextTick() vs. setImmediate()
`process.nextTick()` queues its callbacks before I/O and timer callbacks.  It is used when we need to postpone emitting an event until after the caller has had the chance to register an event listener for this event.

`setImmediate()` queues its callbacks in the event queue after I/O and timer callbacks.

**NOTE**: the names are counter-intuitive since in reality the setImmediate callback is actually executed after the process.nextTick callback.

## NPM Tools

### asyc
* [auto](http://caolan.github.io/async/docs.html#auto)
`auto(tasks, concurrency, callback)`
Determines the best order for running the AsyncFunctions in tasks, based on their requirements. Each function can optionally depend on other functions being completed first, and each function is run as soon as its requirements are satisfied.
