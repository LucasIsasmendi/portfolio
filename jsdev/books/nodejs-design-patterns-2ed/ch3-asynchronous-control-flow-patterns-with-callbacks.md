## Ch3 Asynchronous Control Flow Patterns with Callbacks
In this chapter, we will see how it's actually possible to tame callbacks and write clean, manageable asynchronous code by using some discipline and with the aid of some patterns.

### The difficulties of asynchronous programming
Recognizing that our code is becoming unwieldy, or even better, knowing in advance that it might become unwieldy and then acting accordingly with the most adequate solution is what differentiates a novice from an expert.

#### Creating a simple web spider
Web spider, a command-line application that takes in a web URL as input and downloads its contents locally into a file. In the code presented in this chapter, we are going to use a few npm dependencies:
`request`: A library to streamline HTTP calls
`mkdirp`: A small utility to create directories recursively
*01_web_spider*

#### The callback hell
The situation where the abundance of closures and in-place callback definitions transform the code into an unreadable and unmanageable blob is known as **callback hell**(anti-pattern). The form is known as the **pyramid of doom**

Also, we have to keep in mind that closures come at a small price in terms of performance and memory consumption. In addition, they can create memory leaks that are not so easy to identify because we shouldn't forget that any context referenced by an active closure is retained from garbage collection.

**Link**: introduction to how closures work in V8 [link](http://mrale.ph/blog/2012/09/23/grokking-v8-closures-for-fun.html)

### Using plain JavaScript
 Iterating over a collection by applying an asynchronous operation in sequence is not as easy as invoking `forEach()` over an array, but it actually requires a technique similar to a recursion. This section is aboutn avoid callback hell and implement control flow in plain JS.

#### Callback discipline
When writing asynchronous code, the first rule to keep in mind is to not abuse closures when defining callbacks.
* Exit as soon as possible. Use `return`, `continue`, or `break`.
* Create named functions for callbacks (out of closures) and passing intermediate results as arguments. Plus looks better in stack traces.
* Modularize the code: split into smaller, reusable functions.

#### Applying the callback discipline
Optimization for our spider() function:
1) Exit as soon as possible. From:
```js
if(err) {
  callback(err);
} else {
  //code to execute when there are no errors
}
```
To:
```js
if(err) {
  return callback(err);
}
//code to execute when there are no errors
```
**Note**: It is then important to insert a `return` instruction to block the execution of the rest of the function. The return value of the asynchronous function is usually ignored.
This property allows us to write shortcuts such as the following: `return callback(...)` or `callback(...) return;`

2) Identify reusable pieces of code, write as functions at the beginning (or in a new file) and call them.

#### Sequential execution
Sequential execution flow: one at a time, one after the other. Start -> A -> B -> C -> End.

There are different variations of this flow:
* Executing a set of known tasks in sequence, without chaining or propagating results
* Using the output of a task as the input for the next (also known as **chain**, **pipeline**, or **waterfall**)
* Iterating over a collection while running an asynchronous task on each element, one after the other

##### Executing a known set of tasks in sequence (pattern)
```js
function asyncOperation(callback) {
  process.nextTick(callback);
}
function task1(callback) {
  asyncOperation(() => { task2(callback); });
}
function task2(callback) {
  asyncOperation(() => { task3(callback); });
}
function task3(callback) {
  asyncOperation(() => {
    callback(); //finally executes the callback
  });
}
task1(() => {
  //executed when task1, task2 and task3 are completed
  console.log('tasks 1, 2 and 3 executed');
});

```
The pattern puts the emphasis on the modularization of tasks, showing how closures are not always necessary to handle asynchronous code.

##### Sequential iteration
The pattern we described earlier works perfectly if we know in advance what and how many tasks are to be executed. What happens if we want to execute an asynchronous operation for each item in a collection?

**Web spider version 2**
 We now want to download all the links contained in a web page recursively. To do that, we are going to extract all the links from the page and then trigger our web spider on each one of them recursively and in sequence.

**Sequential crawling of links**
Adds nesting, allows us to iterate over an array by executing an asynchronous operation in sequence

**Pattern (sequential iterator)**
The code of the `spiderLinks()` function is a clear example of how it's possible to iterate over a collection while applying an asynchronous operation. This pattern can be generalized as follows:
```js
function iterate(index) {
  if(index === tasks.length)  {
    return finish();
  }
  const task = tasks[index];
  task(function() {
    iterate(index + 1);
  });
}
function finish() {
  //iteration completed
}
iterate(0);
```
**Note**: It's important to notice that these types of algorithm become really recursive if `task()` is a synchronous operation. In such a case, the stack will not unwind at every cycle and there might be a risk of hitting the maximum call stack size limit.

We can map the values of an array or we can pass the results of an operation to the next one in the iteration to implement a reduce algorithm, we can quit the loop prematurely if a particular condition is met, or we can even iterate over an infinite number of elements:
`iterateSeries(collection, iteratorCallback, finalCallback)`

#### Parallel execution (concurrency)
There are some situations where the order of execution of a set of asynchronous tasks is not important and all we want is just to be notified when all those running tasks are completed: Start -> A, B, C -> End.

We realize that even though we have just one thread, we can still achieve concurrency, thanks to the non-blocking nature of Node.js. In fact, the word parallel is used improperly in this case, as it does not mean that the tasks run simultaneously, but rather that their execution is carried out by an underlying non-blocking API and interleaved by the event loop: concurrency.

The following diagram shows how two asynchronous tasks can run in parallel in a Node.js program:
<p align="center">
  <img src="/images/books/nodejs-patterns-2ed/ch3-async task in parallel.jpg" width="700">
</p>

In short, this means that in Node.js, we can only execute in parallel asynchronous operations, because their concurrency is handled internally by the non-blocking APIs. In Node.js, synchronous (blocking) operations cannot run concurrently unless their execution is interleaved with an asynchronous operation, or deferred with `setTimeout()` or `setImmediate()`.

##### Web spider version 3
So far, our application is executing the recursive download of the linked pages in a sequential fashion. We can easily improve the performance of this process by downloading all the linked pages in parallel.
We will notice a huge improvement in the speed of the overall process, as every download is carried out in parallel without waiting for the previous link to be processed.

**Pattern (unlimited parallel execution)**
Run a set of asynchronous tasks in parallel by spawning them all at once, and then wait for all of them to complete by counting the number of times their callbacks are invoked.
```js
const tasks = [ /* ... */ ];
let completed = 0;
tasks.forEach(task => {
  task(() => {
    if(++completed === tasks.length) {
      finish();
    }
  });
});
function finish() {
  //all the tasks completed
}
```
**Fixing race conditions with concurrent tasks**
Running multiple asynchronous tasks in parallel is one of the most important strengths for Node.js
Another important characteristic of the concurrency model of Node.js is the way we deal with task synchronization and race conditions: same file can be download twice. solution: Map urls.

#### Limited parallel execution
Often, spawning parallel tasks without control can lead to an excessive load.
In a web application, it may also create a vulnerability that is exploitable with Denial of Service (DoS) attacks.
In all such situations, it is a good idea to limit the number of tasks that can run at the same time.

**Limiting the concurrency**
```js
const tasks = ...
let concurrency = 2, running = 0, completed = 0, index = 0;
function next() {                                             //[1]
  while(running < concurrency && index < tasks.length) {
    task = tasks[index++];
    task(() => {                                              //[2]
      if(completed === tasks.length) {
        return finish();
      }
      completed++, running--;
      next();
    });
    running++;
  }
}
next();
function finish() {
  //all tasks finished
}
```
This algorithm can be considered a mix between a sequential execution and a parallel execution

**Globally limiting the concurrency**
Node.js versions before 0.11 already limit the number of concurrent HTTP connections per host to 5. This can, however, be changed to accommodate our needs.

**Queues to the rescue**
We are now going to implement a simple class named `TaskQueue`, which will combine a queue with the algorithm we presented before.

##### Web spider version 4
Generic queue to execute tasks in a limited parallel flow

### The async library
The [async](https://www.npmjs.com/package/async) library is a very popular solution, in Node.js and JavaScript in general, to deal with asynchronous code.

#### Sequential execution
There are around 20 different functions to choose from.
Choosing the right function is an important step in writing more compact and readable code.

**Sequential execution of a known set of tasks**: Web spider version 2 with `async.series(tasks, [callback])`, alternative `async.waterfall()`

**Sequential iteration** `async.eachSeries()` to iterate over the links and call spider.

#### Parallel execution
Web spider version 3 with `async.each()`

#### Limited parallel execution
Functions available: eachLimit(), mapLimit(), parallelLimit(), queue(), and cargo().

Web spider version 4 with `async.queue(worker, concurrency)`
```js
const downloadQueue = async.queue((taskData, callback) => {
  spider(taskData.link, taskData.nesting - 1, callback);
}, 2);
```
