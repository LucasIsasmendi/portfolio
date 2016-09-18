# Best Practices
## Base
### REST principles
#### 1. Uniform Interface
Individual resources are identified using URLS. The resources (database) are themselves different from the representation (XML, JSON, HTML) sent to the client. The client can manipulate the resource through the representations provided they have the permissions. Each message sent between the client and the server is self-descriptive and includes enough information to describe how it is to be processed. The hypermedia that is hyperlinks and hypertext act as the engine for state transfer.

#### 2. Stateless Interactions
none of the clients context is to be stored on the server side between the request. All of the information necessary to service the request is contained in the URL, query parameters, body or headers.

#### 3. Cacheable
Clients can cache the responses. The responses must define themselves as cacheable or not to
prevent the client from sending the inappropriate data in response to further requests.

#### 4. Client-Server
The clients and the server are separated from each other thus the client is not concerned with the data storage thus the portability of the client code is improved while on the server side the server is not concerned with the client interference thus the server is simpler and easy to scale.

#### 5. Layered System
At any time client cannot tell if it is connected to the end server or to an intermediate. The  intermediate layer helps to enforce the security policies and improve the system scalability by enabling load-balancing

####  6. Code on Demand
an optional constraint where the server temporarily extends the functionality of a client by the transfer of executable code.
[servage magazine](https://www.servage.net/blog/2013/04/08/rest-principles-explained/)

## Angular
### Directory Structure
[scotch.io](https://scotch.io/tutorials/angularjs-best-practices-directory-structure)  
```js
app/
----- shared/   // acts as reusable components or partials of our site
---------- sidebar/
--------------- sidebarDirective.js
--------------- sidebarView.html
---------- article/
--------------- articleDirective.js
--------------- articleView.html
----- components/   // each component is treated as a mini Angular app
---------- home/
--------------- homeController.js
--------------- homeService.js
--------------- homeView.html
---------- blog/
--------------- blogController.js
--------------- blogService.js
--------------- blogView.html
----- app.module.js
----- app.routes.js
assets/
----- img/      // Images and icons for your app
----- css/      // All styles and style related files (SCSS or LESS files)
----- js/       // JavaScript files written for your app that are not for angular
----- libs/     // Third-party libraries such as jQuery, Moment, Underscore, etc.
index.html
```
#### Best Practices (For Huuuuge Apps)
* Modularize the Header and Footer
* Modularize the routes
* Don't forget to minify
* Keep the names consistent

### Angular 1 Style Guide
[GitHub](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)  
Opinionated Angular style guide for teams by [@john_papa](//twitter.com/john_papa)*
>If you like this guide, check out my [Angular Patterns: Clean Code](http://jpapa.me/ngclean) course at Pluralsight which is a companion to this guide.

### Angular 1.x styleguide (ES2015)
[GitHub](https://github.com/toddmotto/angular-styleguide)
Architecture, file structure, components, one-way dataflow and best practices

### Angular 1.4.x styleguide (ES5/old version)
[GitHub](https://github.com/toddmotto/angular-styleguide/tree/angular-old-es5)
A standardised approach for developing Angular applications in teams. 

## Node.js
### Node Hero - Node.js Project Structure Tutorial
[RisingStack](https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/)
#### The 5 fundamental rules of a Node.js Project Structure
* Rule 1 - Organize your Files Around Features, Not Roles
* Rule 2 - Don't Put Logic in index.js Files
* Rule 3 - Place Your Test Files Next to The Implementation
* Rule 4 - Use a config Directory

### Best Practices for Node.js Development
[Heroku](https://devcenter.heroku.com/articles/node-best-practices)
* Start every new project with npm init
* Use a smart .npmrc
* Hop on the ES6 train
* Stick with lowercase
* Cluster your app
* Be environmentally aware
* Avoid garbage
* Hook things up
* Only git the important bits
* Simplify

## Frameworks
### Meteor 
#### Application Structure
[Meteor](https://guide.meteor.com/structuRule 4 - Use a config Directoryre.html)  
How to structure your Meteor app with ES2015 modules, ship code to the client and server, and split your code into multiple apps.

### Express
#### Best practices for Express app structure
[terlici](https://www.terlici.com/2014/08/25/best-practices-express-structure.html)

