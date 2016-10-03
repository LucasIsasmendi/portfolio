# Meteor Guide
Meteor is a full-stack JavaScript platform for developing modern web and mobile applications. Meteor includes a key set of technologies for building connected-client reactive applications, a build tool, and a curated set of packages from the Node.js and general JavaScript community.

>Meteor comes with npm bundled.

Meteor Pakages in [Atmosphere](https://atmospherejs.com/)

## Code Style
>Suggested style guidelines for your code.

* Use the `ecmascript` package. To get the full experience, you should also use the es5-shim package which is included in all new apps by default. 
    - [Get started with ES2015 (ES6) and Meteor](http://info.meteor.com/blog/es2015-get-started)
* Follow a JavaScript style guide: Airbnb style guide with the ES6 extensions
* Check your code with ESLint: Airbnb eslint configuration, run the code or add a eslintConfig section to your package.json
```
meteor npm install --save-dev babel-eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-meteor eslint-plugin-react eslint-plugin-jsx-a11y eslint-import-resolver-meteor eslint
```

to run linter: 
```
meteor npm run lint
```
* Integrate ESLint with your code editor: sublime, atom

### Meteor code style
#### Collections
Collections should be named as a plural noun, in **PascalCase**.  
Fields in the database should be **camelCased**

#### Methods and publications
Method and publication names should be **camelCased**

#### Files, exports, and packages
You should use the ES2015 `import` and `export` features to manage your code. 
Each file in your app should represent one logical module.  
When a file represents a single class or UI component, the file should be named the same as the thing it defines, with the same capitalization. 
``` js
export default class ClickCounter { ... }
...
import ClickCounter from './ClickCounter.js';
```

#### Templates and components
Since Spacebars templates are always global, can’t be imported and exported as modules, and need to have names that are completely unique across the whole app, we recommend naming your Blaze templates with the full path to the namespace, separated by underscores. 
If you are writing your UI in React, you don’t need to use the underscore-split names because you can import and export your components using the JavaScript module system.

## Application Structure
>How to structure your Meteor app with ES2015 modules, ship code to the client and server, and split your code into multiple apps.

Meteor applications include code that runs on the client, on the server and in boths environments.

### ES2015 modules
As of version 1.3, Meteor ships with full support for ES2015 modules. 
In ES2015, you can make variables available outside a file using the export keyword. To use the variables somewhere else, you must import them using the path to the source. Files that export some variables are called “modules”, because they represent a unit of reusable code. 
Explicitly importing the modules and packages you use helps you write your code in a modular way, avoiding the introduction of global symbols and “action at a distance”. [more about modules package](https://docs.meteor.com/packages/modules.html)

### Introduction to using `import` and `export`
Meteor allows you to `import` not only JavaScript in your application, but also CSS and HTML to control load order.
Meteor also supports the standard ES2015 modules `export` syntax

### Importing from packages
In Meteor, it is also simple and straightforward to use the import syntax to load npm packages on the client or server and access the package’s exported symbols as you would with any other module.
``` js
import moment from 'moment';          // default import from npm
import { HTTP } from 'meteor/http';   // named import from Atmosphere
```

### File structure
To fully use the module system and ensure that our code only runs when we ask it to, we recommend that all of your application code should be placed inside the `imports/` directory. This means that the Meteor build system will only bundle and include that file if it is referenced from another file using an `import` (also called “lazy evaluation or loading”).

Meteor will load all files outside of any directory named `imports/` in the application using the default file load order rules (also called “eager evaluation or loading”). It is recommended that you create exactly two eagerly loaded files, `client/main.js` and `server/main.js`, in order to define explicit entry points for both the client and the server.

### Example directory layout
``` shell
imports/
  startup/
    client/
      index.js                 # import client startup through a single index entry point
      routes.js                # set up all routes in the app
      useraccounts-configuration.js # configure login templates
    server/
      fixtures.js              # fill the DB with example data on startup
      index.js                 # import server startup through a single index entry point

  api/
    lists/                     # a unit of domain logic
      server/
        publications.js        # all list-related publications
        publications.tests.js  # tests for the list publications
      lists.js                 # definition of the Lists collection
      lists.tests.js           # tests for the behavior of that collection
      methods.js               # methods related to lists
      methods.tests.js         # tests for those methods

  ui/
    components/                # all reusable components in the application
                               # can be split by domain if there are many
    layouts/                   # wrapper components for behaviour and visuals
    pages/                     # entry points for rendering used by the router

client/
  main.js                      # client entry point, imports all client code

server/
  main.js                      # server entry point, imports all server code
```

### Structuring imports

