# Best Practices: JSDoc

## Syntax
### General
`@description <some description>`:
Can omit this tag if description is located at the beginning

### Membership
`@namespace [[{<type>}] <SomeName>]`:
an object creates a namespace for its members.

`@memberof <parentNamepath>`:
identifies a member symbol that belongs to a parent symbol.  
`@memberof! <parentNamepath>`:
 forces JSDoc to document a property of an object that is an instance member of a class.
[Examples](http://usejsdoc.org/tags-memberof.html#examples)

### Relational
`@implements {typeExpression}`:
a symbol implements an interface.

`@interface [<name>]`:
marks a symbol as an interface that other symbols can implement.

`@external <NameOfExternal>`:
identifies a class, namespace, or module that is defined outside of the current package.
`@see <text|namepath>` to add a link

### Entities
`@class [<type> <name>]`:
marks a function as being a constructor, meant to be called with the new keyword to return an instance.

`@function [<FunctionName>]`:
marks an object as being a function, even though it may not appear to be one to the parser

`@module [[{<type>}] module:<moduleName>]`:
marks the current file as being its own module. All symbols in the file are assumed to be members of the module unless documented otherwise.

`@static`:
symbol is contained within a parent and can be accessed without instantiating the parent.

#### Symbols, Parameters, Variables
`@type {typeName}`:
allows you to provide a type expression identifying the type of value that a symbol may contain, or the type of value returned by a function.
[examples](http://usejsdoc.org/tags-type.html)

`@typedef [<type>] <namepath>`:
custom types, particularly if you wish to refer to them repeatedly.

`@private | @public | @protected`:
* public: JSDoc treats all symbols as public
* proceted: a symbol is only available, or should only be used, within the current module.

`@inner vs @global`:

`@default [<some value>]`:
You can supply this tag with a value yourself or you can allow JSDoc to automatically document the value from the source code.

### Beavior
`@param {type} name - description`:
name, type, and description of a function parameter.
[examples](http://usejsdoc.org/tags-param.html)

`@returns`:

`@callback`:
callback function that can be passed to other functions.

`@throws {<type>} free-form description`:
an error that a function might throw.

### Events
`@event <className>#[event:]<eventName>`:

`@listens <eventName>`:
 indicates that a symbol listens for the specified event.

`@fires <className>#[event:]<eventName>`:

`@mixin`:
This provides methods used for event handling

## Tips
#### [@event with multiple function arguments](https://github.com/jsdoc3/jsdoc/issues/1126)

#### [How to correctly document namespaces?](https://github.com/jsdoc3/jsdoc/issues/443)
``` js
/** @namespace */
hgm = {};

/** @namespace */
hgm.cookie = {
    /** describe me */
    get: function (name) {  },

    /** describe me */
    set: function (name, value) {  },

    /** describe me */
    remove: function (name) {  }
};
```
#### [A namespace with defaults and nested default properties](http://usejsdoc.org/tags-property.html#examples)
``` js
/**
 * @namespace
 * @property {object}  defaults               - The default values for parties.
 * @property {number}  defaults.players       - The default number of players.
 * @property {string}  defaults.level         - The default level for the party.
 * @property {object}  defaults.treasure      - The default treasure.
 * @property {number}  defaults.treasure.gold - How much gold the party starts with.
 */
var config = {
    defaults: {
        players: 1,
        level:   'beginner',
        treasure: {
            gold: 0
        }
    }
};
```


#### [Documenting large apps, group modules into @categories](https://github.com/jsdoc3/jsdoc/issues/1136)
``` js
/**Parent module
* @module package-name
*/

/**Child of the parent module
* @namespace firstChild
* @memberof module:package-name
*/
```

##### Category example
to be done

#### [How to properly document node modules exporting a factory?](https://github.com/jsdoc3/jsdoc/issues/1086)

``` js
/**
 * @module jsdoctest
 */

/**
 * Creates a new jsdocTest object.
 * @name jsdocTestFactory
 * @returns {jsdocTest}
 */
function factory () {
  return {
    method1: method1
  };

  /**
   * Does something useful.
   */
  function method1 () {
  }
};

module.exports = factory
```

#### [how to use @function or @method ?](https://github.com/jsdoc3/jsdoc/issues/244)


#### [How do I JSDoc A Nested Object's Methods?](http://stackoverflow.com/questions/19230971/how-do-i-jsdoc-a-nested-objects-methods)
``` js
/**
 * @module mystuff/foo
 * @version 1.0
 */
define([], function() {

/** @lends module:mystuff/foo */
var foo = {
    /** A method in first level, just for test */
    testFirstLvl: function(msg) {},
    /**
     * @namespace
     */
    bar4: {
        /** This is the description for baz4. */
        baz4: function() { /*...*/ },
        /** This is the description for test. */
        test : true
    }
};
return foo;
});
```


## Examples
### By Code

### By Design Pattern
#### Module Pattern

#### Prototype Pattern


## Source code Examples
* [protobuf](https://github.com/dcodeIO/protobuf.js)
* [doc aws-xray-sdk](https://docs.aws.amazon.com/xray-sdk-for-nodejs/latest/reference/index.html)
* openpgpjs:
  - [doc page](https://openpgpjs.org/openpgpjs/doc/)
  - [github](https://github.com/openpgpjs/openpgpjs)
* Eggs:
  - [doc-page](https://eggjs.org/api/index.html)
  - [github](https://github.com/eggjs/egg/)

* [resin-sdk](https://github.com/resin-io/resin-sdk)
* pg-promise lib/database

## Guides
### [short jsdoc guide](http://cancerberosgx.github.io/short-jsdoc/doc/guide/)

### [JavaScript API documentation and comment standards](https://www.drupal.org/docs/develop/standards/javascript/javascript-api-documentation-and-comment-standards)
