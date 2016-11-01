# Learning Functional JavaScript
## Introduction
Course Overview: Sections
* 1 Introductiob to Functional Programming
* 2 Higher Order Functions
* 3 Functional Asynchrony
* 4 Function Composition
* 5 Immutability
* 6 Recursion
* 7 Laziness
The game is built base on Functional Core, Imperative Shell [Gary Bernhardt's](https://gist.github.com/jhrr/7368923)
### 1.2 What Is Functional Programming?
Understand the basic premise of functional programming and how it is helpful.
#### Creating concise, high-level abstractions
Imperative:
```js
function sum(numbers) {
  var sum = 0;
  for (var i = 0, l = numbers.length; i < l; i++) {
    sum += numbers[i];
  }
  return sum;
}
```
High Level abstraction
```js
function sum2(numbers) {
  return numbers.reduce(function (sum, num) {
    return sum + num;
  }, 0);
}
```

#### Avoiding side effects
function with side effect, the function change the values without communicate
```js
function updateView() {
  var sum = 0;

  $('.line-item').each(function () {
    sum += Number(this.innerHTML);
  });

  $('.total').text(sum); // side effect updating the DOM
}
```
code without and with side effects: arguments in, return values out: we can reuse
```js
function getNumbers(coll) {
  return coll.map(function () {
    return Number(this.innerHTML);
  });
}
function updateView2() {
  $('.total').text(sum(getNumbers($('.line-item'))));
}
```

#### Understanding the benefits of referential transparency
not side effect: we can replace a function with his value an nothing will change
### 1.3 Your First Functional Programming Concepts
Ease into functional programming with some important core concepts.
#### Explain pure functions
*pure* functions are easy to test: `sum2`, `getNumbers`, in order to create a user interface, the program must have *impure* functions:`updateView2`
#### Introducing immutability
Data can not change
```js
var arr = [1,2,3];
var newArr = arr.concat(4);
arr //[1,2,3]
newArr //[1,2,3,4]
```
#### Introducing lazy evaluation
Doesn't perform any operation, list withou end, use function calls to control flow.

## 2 Higher Order Functions
### 2.1 First-class Functions
#### Distinguishing between function declarations and function expressions
```js
// function declaration
function helloWorld() {
  return "Hello, bleak world";
};
//function expression
var helloWorld = function helloWorld() {
  return "Hello, bleak world";
};

```
#### Using functions as methods
```js
var person = {
  hello: function () {
    return "Hello";
  }
};
person.hello();
```
#### Assigning properties to function objects
helloWorld.id = 42;

### 2.2 Passing Functions as Arguments
Preparing the AST(Abstract Syntax Tree) for building the view.
Imperative way: focus on details about looping list and building list.
```js
function nodesByType(type, ast) {
    var result = [];
    for (var i = 0, l = ast.length; i < l; i++) {
        if (type === ast[i][0]){
            result.push(ast[i]);
        }
    }
    return result;
}
```
Functional: Building an AST walker-filter, Building an AST walker-map, Building and using splitBy-reduce and more examples of map/filter
```js
function contains(list, item) {
  return list.indexOf(item) >= 0;
}

function nodesByTypes(types, ast) {
  return ast.filter(function (node) {
    return contains(types, node[0]);
  });
}

function second(list) {
  return list[1];
}

function splitBy(predicate, list) {
  var result = [];

  list.forEach(function (elem) {
    if (predicate(elem)) {
      result.push([]);
      return result;
    } else if (result.length === 0) {
      result.push([]);
    }

    result[result.length - 1].push(elem);
  });

  return result;
}

function isParagraph(node) {
  return node[0] === 'paragraph';
}

function makeParagraph(entries) {
  return entries.map(second).join('\n');
}

function identity(v){
    return v;
}
function prepareView(page) {
  return splitBy(
    isParagraph,
    nodesByTypes(['plainText', 'paragraph'], page.ast)
  ).map(makeParagraph).filter(identity);
}

var ast = [
  ['plainText', 'You are in a'],
  ['plainText', 'cold and dark place'],
  ['paragraph'],
  ['paragraph'],
  ['plainText', 'You are cold']
];

console.log(prepareView({ast: ast}));
```

### 2.3 Returning Functions
Applying a changeset to a user.
* Filtering out flags to be removed
* Extracting the filter function
* Solving the problem with a combination of function-building functions
```js
function merge() { //create a new object with all the propertyes from all objects????
  return [].reduce.call(arguments, function (res, obj) {
    return Object.keys(obj).reduce(function (res, prop) {
      res[prop] = obj[prop];
      return res;
    }, res);
  }, {});
}
// instead of notContained, for more clear split
function not(fn) {
  return function (val) {
    return !fn(val);
  };
}

function contained(list) {
  return function (item) {
    return list.indexOf(item) >= 0;
  };
}

function applyChangeset(user, changes) {
  return merge({
    flags: user.flags.filter(not(contained(changes.removeFlags))).
      concat(changes.addFlags),
    currentPage: changes.currentPage,
    pages: user.pages.concat(changes.currentPage)
  });
}

var user = {
  name: 'Chris',
  flags: ['n00b'],
  pages: [0]
};

var changeset = {
  removeFlags: ['n00b', 'advanced'],
  addFlags: ['beginner'],
  currentPage: 1
};

console.log(applyChangeset(user, changeset));
```

## 3 Functional Asynchrony
### 3.1 Callbacks
Creating new users.
* Generating the salt
* Creating the password hash
* Persisting the user in the database
This example is nested, is not a good example of functional programming (the goal is to model data relationships, transform and dependencies). The example state what, how and when to do it: is imperative.
```js
var bcrypt = require("bcrypt");

function save(user, callback) {
  bcrypt.genSalt(10, function (err, salt) {
    if (err) { return callback(err); }
    bcrypt.hash(user.password, salt, function (err, hash) {
      if (err) { return callback(err); }
      db.put(user.username, {
        username: user.username,
        salt: salt,
        passwordHash: hash,
        games: []
      }, function (err, user) {
        if (err) { return callback(err); }
        callback(undefined, {
          username: user.username,
          games: user.games
        });
      });
    });
  });
}
```

### 3.2 Continuation Passing Style (CPS)
Applying iterator methods on async operations. 
It can be apply only if the function follows the form CPS: first the error, then the result.
* Reimagining save() as a sequence of operations
* Implementing series()
```js
var bcript = require('bcript');

function series(steps, callback){
  var stepfn = steps.slice(); // copy the input object so no mutation
  function doNext(){

  }
  doNext();
}

function save(user, callback){
  series([
    function(){
      bcript.genSalt(10,next);
    },
    function(salt, next){
      bcript.hash(user.password, salt, next);
    },
    function(hash, next){
      db.put(user.username, {
        username: user.username,
        salt: salt,
        passwordHash: hash,
        games: []
      }, next);
    }
  ], function(err, user){
    if(err){return callback(err)}
    callback(undefined, {
      username: user.name,
      games: user.games
    });
  });
}
```
* Developing a CPS-aware map and using async.js
```js
```


```js
```




Promises
Partial Function Application
Currying
Composition
What Is Immutability?
Working with Immutability
Immutability in the UI
Mechanics and Performance
Recursive Thinking
A Recursive AST Parser
Trampolining
Lazy Evaluation
Lazy Sequences
Infinite Sequences