# Rapid ES6 Training
## 3. New ES6 Syntax
### let, const and Block Scoping
let is related with hoisting, const is unable to change, block scoping is a replace to function scope.

ES5
``` javascript
'use strict';
console.log(productId); //undefined, because hoisting
var productId = 12;
```

ES6
``` javascript
'use strict';
console.log(productId); //"ReferenceError: productId is not defined, no hoisting take place when we use the let keyword
let productId = 12;
```
``` javascript
'use strict';
let productId = 12;
console.log(productId); //12, the variable declaration takes place before been used.
```
Block scoping
``` javascript
'use strict';
let productId = 12;
{
  let productId = 2000;
}
console.log(productId); //12
```
Lexical Scope??
``` javascript
'use strict';
function updateProductId(){
  productId = 12;
}
let productId = null;
updateProductId();
console.log(productId); //12 ??
```
```javascript
'use strict';
let productId = 42;
for (let productId = 0; productId < 10; productId++)
{
  console.log(productId); // 0 1 2 3 4 5 6 7 8 9
}
console.log(productId);// 42
```
Resolve working with closures
```javascript
'use strict';
let updateFunctions = [];
for (let i = 0; i < 2; i++) {
	updateFunctions.push(function () { return i; });
}
console.log(updateFunctions[0]()); //0, if instead of let we use var, it will be 2 always
```
When we use `const` to declare a variable, we must initialize it.
```javascript
'use strict';
const DISTANCE_CENTER;
console.log(DISTANCE_CENTER); //SyntaxError: Missing initializer in const declaration
```
Once we initialize a constant, we can't change it
```javascript
'use strict';
const DISTANCE_CENTER =100;
DISTANCE_CENTER =10;
console.log(DISTANCE_CENTER); //TypeError: Assignment to constant variable.
```
### Arrow Functions
0 parameter
```javascript
var getPrice = () => 5.99;
console.log(getPrice()); // 5.99
```
1 parameter
```javascript
var getPrice = count => count * 4.00;
console.log(getPrice(2)); // 8
```
2 parameters
```javascript
var getPrice = (count, tax) => count * 4.00 * (1 + tax);
console.log(getPrice(2, 0.07)); // 8.56
```
2 Parameters and block with return
```javascript
var getPrice = (count, tax) => {
  var price = count * 4.00;
  price *= (1 + tax);
  return price;
}
console.log(getPrice(2, 0.07));// 8.56
```
The real purpose of `=>` functions is to handle the `this` within functions. We get the context of the code we are running.

```javascript
var invoice = {
  number: 123,
  process: function () {
    console.log(this);
  }
};
invoice.process(); // Object { number: 123 }
```
```javascript
var invoice = {
  number: 123,
  process: () => console.log(this)
};
invoice.process(); // Window {...}
```
The `bind` or `call`  don't works with `=>` functions.
``` javascript
var invoice = {
  number: 123,
  process: function (){
    return () => console.log(this.number);
  }
};
var newInvoice = {
    number: 456
};
invoice.process().bind(newInvoice)(); //123
invoice.process().call(newInvoice)(); //123
```
> We cannot put the arrow function in a new line
> in ES6, when we declare a function with `=>`, we don't have access to the prototype field
>

### Default Function Parameters
```javascript
var getProduct = function(productId= 1000, type = 'software'){
    console.log(productId+', '+type);
};
getProduct(undefined, 'hardware'); // 1000, hardware
```
when we set a default we have access to variables taht are in the context
```javascript
var baseTax = 0.07; // we can have baseTax = () => 0.07; and works the same way
var getTotal = function(price, tax = price * baseTax){
    console.log(price + tax);
};
getTotal(5.00); // 5.35
```
> arguments.length is related to the caller, not the definition, so in this case returns 1

```javascript
var getTotal = function(price = adjustment, adjustment = 1.00){
    console.log(price + adjustment);
}
getTotal(); //ReferenceError: adjustment is not defined
getTotal(5.00) // 6
```
Dynamic function: parameters are arguments, like eval()!!

```javascript
var getTotal = new Function("price = 20.00", "return price;");
console.log(getTotal()); // 20
```

### Rest and Spread
`rest` *...* refers to putting paramenters in a single array, `spread` refers to spread it out element from array.

#### `rest` parameter
```javascript
var showCategories = function(productId, ...categories){
  console.log(categories);
};
showCategories(123, 'search', 'advertising'); //['search', 'advertising']
showCategories(123); // []
```
Dynamic function
```javascript
var showCategories = new Function("...categories", "return categories;");
console.log(showCategories('search', 'advertising')); //['search','advertising']
```

#### `spread` parameter
if the input is an array `...` will split the array in the list of parameters.
```javascript
var prices = [12,20,18];
var maxPrice = Math.max(...prices);
console.log(maxPrice); // 20
```

>with empty values, we get undefined
>with strings, the `spread` operator will breack it into they characters

```javascript
  console.log(Math.max(..."123645")); // 6
```

#### Object Literal Extensions
We don't have to specified the same value twice like "price: price"
```javascript
var price = 5.99, quantity = 30;
var productView = {
  price,
  quantity
};
console.log(productView); // {price: 5.99, quantity: 30}
```

Be carefull with the context of the code (lexical scope). Is diferent common functions and arrow functions
```javascript
var price = 5.99, quantity = 10;
var productView = {
  price: 7.99,
  quantity: 1,
  calculateValue() {
    return this.price * this.quantity
  },
  calculateValueAF : () => this.price * this.quantity,
  "calculate value"() {
    return this.price * this.quantity
  }
};
console.log(productView.calculateValue()); // 7.99
console.log(productView.calculateValueAF()); // 59.900000000000006
console.log(productView["calculate value"]()); // 7.99
```
>we can use variable name with brackets and also methods ['name'] and geters and seters

```javascript
var ident = 'productId';
var productView = {
  get [ident] () { return true;},
  set [ident] (value) { }
};
console.log(productView.productId); // true
```

#### for ...of Loops
```javascript
var categories = ['hardware', 'software', 'vaporware'];
for(var item of categories){
  console.log(item); //'hardware' 'software' 'vaporware'
}
```

#### Octal and Binary Literals
```javascript
var octalvalue = 0o10, binvalue = 0b10;
console.log(octalvalue); //8
console.log(binvalue); //2
```

### Template Literals
Interpolation take places when we run into this symbols
```javascript
let invoiceNum = '1350';
console.log(`Invoice Number: ${invoiceNum}`); // Invoice Number: 1350
console.log(`Invoice Number:\${invoiceNum}`); // Invoice Number:${invoiceNum}
console.log(`Invoice Number: ${"INV-" + invoiceNum}`); // Invoice Number: INV-1350
```
> When we create a template literal, blank spaces as newlines and tabs are maintained.

the interpolation takes place first, before the function call
```javascript
function showMessage(message) {
  let invoiceNum = '99';
  console.log(message);
}
let invoiceNum = '1350';
showMessage(`Invoice Number: ${invoiceNum}`); // Invoice Number: 1350
```
We can create our own tag template
```javascript
function processInvoice(segments, ...values) {
  console.log(segments);
  console.log(values);
}
let invoiceNum = '1350', amount = '2000';
processInvoice `Invoice: ${invoiceNum} for ${amount}`;
//["Invoice: ", " for ", ""]
//[1350, 2000]
```

### Desctructuring
```javascript
let salary = ['32000', '50000'];
let [ low, average, high ] = salary;
console.log(high); // undefined
console.log(average); // "50000"
```
>It can be apply to nested arrays. Can be done in declarations or assigments.

```javascript
function reviewSalary([low, average], high = '88000'){
  console.log(average); // 50000
}
reviewSalary(['32000','50000']);
```
>we can also destructure objects, be carefull with blocks, to avoid errors ({xx})
**More about desctructuring**
## 4. Modules and Classes

### Introduction and Setup
We will use a transpiler [traceur](https://github.com/google/traceur-compiler) or you can use [babel](https://github.com/babel/babel)
And a module loader [es6-module-loader-dev](https://github.com/ModuleLoader/es6-module-loader) ** Chrome CORS error, github issue ticket, Firefox Works ok*

### Module Basics
`import` and `export`

```javascript
/* File base.js*/
import { projectId as id, projectName} from 'module1.js';
console.log(`${projectName} has id: ${id}`); // BuildIt has id: 99
console.log(`${projectName} has id: ${projectId}`); // ReferenceError: projectId is not defined
/* File module1.js*/
export let projectId = 99;
export let projectName = 'BuildIt';
```
> Once we set an alias, we have to use taht alias.

Import is hoisted, the module loader will scan for the import statement and hoist that to the top of the file and dependencies will be execcute first
```javascript
/* File base.js*/
console.log("starting in base");
import { projectId } from 'module1.js';
console.log("ending in base");
/* File module1.js*/
export let projectId = 99;
console.log("in module1");
```

> `export default ` is loaded without {}, a module can have
> `import * as values` imporst all as an object
> `ìmport` variabes is read-only, we cannot change his value
> `ìmport` objects we can change his properties
> `import` functions we can use them, from exporting we can change functionallyties

### Class Fundamentals
There is not new type of class in ES6, typeof class is a function
Adding a method o a class, is similar to added to the prototype object

```javascript
class Task {
    constructor () {
        console.log("constructing Task");
    }
    showId() {
        console.log('99');
    }
}
let task = new Task(); // constructing Task
console.log(task.showId === Task.prototype.showId); // true
```
> if we have `constructor ()` in a class, when we instanciate that object, the function will be call
> When we work with classes, no coma "," between functions
> We can't have variables within a class
> Classes are not hoisted (no las reacomoda el compilador, si pongo la llamada antes que al definición es un error)

### Extends and super
Inheritance is by prototypes. We can use extend to work with prototypes.

```javascript
class Project {
    constructor() {
        console.log('constructing Project');
    }
}
class SoftwareProject extends Project {
    constructor() {
        super(); // without it, we have error from this
        console.log('constructing SoftwareProject');
    }
}
let p = new SoftwareProject();
// constructing Project
// constructing SoftwareProject
```

Prototype Chain
```javascript
class Project {
    getTaskCount() {
        return 50;
    }
}
class SoftwareProject extends Project {
    getTaskCount(){
        return super.getTaskCount() + 6;
    }
}
let p = new SoftwareProject();
console.log(p.getTaskCount()); // 56
```

Usign `super` with object literals
```javascript
let project = {
    getTaskCount() { return 50; }
}
let softwareProject = {
    getTaskCount() {
        return super.getTaskCount() + 7;
    }
}
Object.setPrototypeOf(softwareProject, project);
console.log(softwareProject.getTaskCount()); // 57
```

### Properties for Class Instances
initialize with `this`
```javascript
class Project {
    constructor() {
        this.location = 'Necochea';
        let area = 'Buenos Aires';
        var country = 'Argentina';
        let locationb = 'Silicon Valley';
    }
}
class SoftwareProject extends Project {
    constructor() {
        super();
        this.location = this.location + ', Playa';
        let areab = this.area + 'prov';
        var countryb = this.country + 'país'
    }
}
let p = new SoftwareProject();
console.log(p.location); // "Necochea, Playa"
console.log(p.areab);// undefined
console.log(p.countryb);// undefined
console.log(p.locationb); // undefined
```

### Static Memebers
```javascript
class Project {    
}
Project.id = 99;
console.log(Project.id); // 99
```

```javascript
class Project {
    static getDefaultId() {
        return 0;
    }
}
console.log(Project.getDefaultId());//0
p = new Project();
console.log(p.getDefaultId());// "error" TypeError: p.getDefaultId is not a function
```

### new.target
is a constructor, a function(), allways will point to the initial constructor that is called.
Can be use to access statics methods from the caller
```javascript
class Project {
    constructor() {
        console.log(new.target.getDefaultId());
    }
}
class SoftwareProject extends Project {
    static getDefaultId() { return 99;}
}
let p = new SoftwareProject(); // 99
```

## 5. New Types and Object Extensions

### Introduction

### Symbols
create unique id.
Use...?

### Well-known Symbols
(Mozilla Symbols)[https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol]

### Object Extensions
>Object.serPrototypeOf,
>Object.assign (second parameter overrides first if duplicate, ojo enumerable true si son números),
>Object.is to compare better than `===`

### String Extensions
>`startsWith`, `endsWith`, `includes`, Emoji \u{1f3c4}, `normalize`
(emoji Table)[http://apps.timwhitlock.info/emoji/tables/unicode]

```javascript
var surfer = "\u{1f30a}\u{1f3c4}\u{1f40b}";
console.log(Array.from(surfer).length);
console.log(surfer); // 3 "🌊🏄🐋"🐋
```

### Number Extensions
