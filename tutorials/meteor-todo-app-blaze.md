# Creating your first app (Blaze)
### 1. Creating an App
* [Meteor supports in the ecmascript](https://docs.meteor.com/packages/ecmascript.html)
* Understanding ECMAScript 6 [book](https://github.com/nzakas/understandinges6)
* You Don't Know JS: ES6 & Beyond [book](https://github.com/getify/You-Dont-Know-JS/tree/master/es6%20%26%20beyond)
* ECMAScript 6 Features [github](https://github.com/lukehoban/es6features#readme)
* Meteor's Spacebars compiler [Blaze](http://blazejs.org/guide/spacebars.html)

### 2. Templates
* hot code push: changes are automatically push.
* files inside `imports/` only load if they are imported in `main.js`
* all html files are loaded
* HTML files in Meteor define templates: tags: <head>, <body>, and <template>. Everything inside <template> tags is compiled into Meteor templates, which can be included inside HTML with `{{> templateName}}` or referenced in your JavaScript with `Template.templateName`. Also, the `body` section can be referenced in your JavaScript with `Template.body`.
* Adding logic and data to templates: all HTML cose is compiled with Meteor's Spacebars compiler. You can pass data into templates from your JavaScript code by defining helpers.

### 3. Collections
collections accessed from both the server and the client. They also update themselves automatically.
[Collections and Schemas-Meteor Guide](https://guide.meteor.com/collections.html) 
A new collection in the server creates the mongodb collection, in the client creates the cache connected to the server collection.
* Steps: 
    * 1) create the imports/api/collection.js
    * 2) load collection on the server
    * 3) load collection on the client
    * 4) insert data in mongo db

### 4. Forms and events
Attaching events to templates: by calling `Template.templateName.events(...)` with a dictionary.

### 5. Update and Remove
Code

### 6. Running on mobile
`meteor run android`

### 7. Temporary UI state
use a `ReactiveDict` to store temporary reactive state on the client that don't sync with the server.
Add the `reactive-dict` package: `meteor add reactive-dict`
[patterns for writing components-Blaze](http://blazejs.org/guide/spacebars.html) 

### 8. User Accounts
Ad the accounts packages: `meteor add accounts-ui accounts-password`
include login buttons: `> loginButtons` (currentUser built-in helper)
* If you add the `accounts-facebook` package to enable Facebook login in your app - the Facebook button will automatically appear in the dropdown.

### 9. Security with methods
Every newly created Meteor project has the insecure package added by default. This is the package that allows any user of the app edit any part of the database. To remove this package: `meteor remove insecure` (removes all client-side database permissions)

#### Defining methods
We need one method for each database operation we want to perform on the client. Methods should be defined in code that is executed on the client and the server. ethods should be defined in code that is executed on the client and the server (Optimistic UI).

#### Optimistic UI
When you call a method on the client using `Meteor.call`, two things happen in parallel:  
    - 1.  The client sends a request to the server to run the method in a secure environment, just like an AJAX request would work.  
    - 2.  A simulation of the method runs directly on the client to attempt to predict the outcome of the server call using the available information.  

What this means is that a newly created task actually appears on the screen before the result comes back from the server.
If the result from the server comes back and is consistent with the simulation on the client, everything remains as is. If the result on the server is different from the result of the simulation on the client, the UI is patched to reflect the actual state of the server.  
[Methods article](https://guide.meteor.com/methods.html)  
[blog post about optimistic UI](http://info.meteor.com/blog/optimistic-ui-with-meteor-latency-compensation)

### 10. Filtering data with publish and subscribe
 We need a way of controlling which data Meteor sends to the client-side database.  
 Remove `autopublish` package: `meteor remove autopublish`  
When the app refreshes, the task list will be empty. Without the `autopublish` package, we will have to specify explicitly what the server sends to the client. The functions in Meteor that do this are `Meteor.publish` and `Meteor.subscribe`.

[Publications and Data Loading](https://guide.meteor.com/data-loading.html)

#### Implementing private tasks
code

#### Extra method security
code

### 11. Testing
add a [test driver](https://guide.meteor.com/testing.html#test-driver) for the Mocha JavaScript test framework: `meteor add practicalmeteor:mocha`  
run our app in "test mode": `meteor test --driver-package practicalmeteor:mocha`
file: originalfilename.tests.js
code  
Invoke server method: `Meteor.server.method_handlers['name']`  
Note that arrow function use with Mocha [is discouraged](http://mochajs.org/#arrow-functions).
More about [Meteor Testing](https://guide.meteor.com/testing.html). 

### 12. What's next?
meteor create --example todos
meteor create --example localmarket
