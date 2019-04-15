# React Handbook

[medium](https://medium.freecodecamp.org/the-react-handbook-b71c27b0a795)

The React Handbook follows the 80/20 rule: learn in 20% of the time the 80% of a topic.

you basically need to understand 4 concepts to get started:
- Components
- JSX
- State
- Props

How to use create-react-app
```ts
npx create-react-app todolist
```

**Content**
- [1 Core Concepts](#1-core-concepts)
- [2 React Concepts](#2-react-concepts)
- 3 In-depth
- 4 Styling
- 5 Tooling
- 6 Testing
- 7 A look at the React Ecosystem

## 1 Core Concepts

Arrow functions:
- The `this` scope is inherited from the execution context.
- are not suited as object methods.
- cannot be used as constructors.

Classes inheritance: Inside a class, you can reference the parent class calling super()

Async/Await: debug is very easy because to the compiler it’s just like synchronous code

ES Modules: is the ECMAScript standard for working with modules: `import` It’s either absolute, or has a `./` or `/` before the name. While  CommonJS uses `require`. 
An HTML page can add a module by using a `<script>` tag with the special `type="module"`

CORS: reference scripts for other domains: `Access-Control-Allow-Origin: *`

## 2 React Concepts
React Applications are also called Single Page Applications. Are easy to transform into Progressive Web Apps.

### Overriding the navigation
Since you get rid of the default browser navigation, URLs must be managed manually. This part of an application is called the router.

### Declarative
"Declarative approach to building UIs": no interaction with DOM, we just tell React we want a component to be rendered in a specific way. Opposite is imperative, like JQuery: You tell the browser exactly what to do, instead of telling it what you need.

### Immutability
An object is never updated, but copied before changing it. Change state properties through `setState()` and `reducers`.

### Purity
When a function does not mutate objects but just returns a new object, it’s called a pure function. No side effects. `components`.
Class components can be pure if their output only depends on the props.

### Composition
Create specialized version of a component. Pass methods as props. Using children. Higher order components

### The Virtual DOM
The DOM (Document Object Model) is a Tree representation of the page.
React uses a Virtual DOM to help the browser use less resources when changes (repint and reflow) need to be done on a page. React only updates when a Component changes the state explicitly. The key thing is that React batches much of the changes and performs a unique update to the real DOM

### Unidirectional Data Flow
- state is passed to the view and to child components
- actions are triggered by the view
- actions can update the state
- the state change is passed to the view and to child components

The view is a result of the application state. Thanks to one-way bindings, data cannot flow in the opposite way.
Changing state on a Component will only affect its childs.
