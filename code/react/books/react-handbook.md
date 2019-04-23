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
- [3 In-depth](#3-in-depth)
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

## 3 In-depth

### JSX
declarative syntax of what a component UI should be. Babbel does the transpilation.
- camelCase is the new standard
- You need to close all tags
- class becomes className

#### JS in JSX
Whenever you need to add some JS, just put it inside curly braces `{}`

#### CSS in React
JSX style attribute only accepts an object.
- the keys property names are camelCased
- values are just strings
- you separate each tuple with a comma

JSX allows components (defined in React for example) to completely encapsulate their style. Is this the go-to solution?
Inline styles in JSX are good until you need to:
1. write media queries
2. style animations
3. reference pseudo classes (e.g. :hover)
4. reference pseudo elements (e.g. ::first-letter)

#### Forms in JSX
The `value` attribute always holds the current value of the field.
The `defaultValue` attribute holds the default value that was set when the field was created.

#### A more consistent onChange
Passing a function to the `onChange` attribute you can subscribe to events on form fields.

#### White space in JSX
 explicitly add white space `{' '}`

#### Adding comments in JSX
```js
<p>
  {/* a comment */}
  {
    //another comment
  }
</p>
```
#### Spread attributes
```js
<div>
  <BlogPost title={data.title} date={data.date} />
  <BlogPost {...data} />
</div>
```
#### How to loop in JSX
```js
const elements = ['one', 'two', 'three'];
return (
  <ul>
    {elements.map((value, index) => {
      return <li key={index}>{value}</li>
    })}
  </ul>
)
```

### Components
is one isolated piece of interface: everything is a component.

#### Custom components
There are 2 ways to define a component in React.
- A function component:
```jsx
const BlogPostExcerpt = () => {
  return (
    <div>
      <h1>Title</h1>
      <p>Description</p>
    </div>
  )
}
```
- A class component:
```jsx
import React, { Component } from 'react'
class BlogPostExcerpt extends Component {
  render() {
    return (
      <div>
        <h1>Title</h1>
        <p>Description</p>
      </div>
    )
  }
}
```

React Hooks adds state to function components, do it will be popular in the future.

### State
- Setting the default state of a component: `this.state`
- Accessing the state: `this.state.clicked`
- Mutating the state: `this.setState({ clicked: true })`
- Unidirectional Data Flow: A state is always owned by one Component
- Moving the State Up in the Tree: Because of the Unidirectional Data Flow rule, if two components need to share state, the state needs to be moved up to a common ancestor.

### Props
is how Components get their properties: every child component gets its props from the parent. A component either holds data (has state) or receives data through its props.

#### Children
is a prop that contains the value of anything that is passed in the body of the component

While Props allow a Component to receive properties from its parent, to be “instructed” to print some data for example, state allows a component to take on life itself, and be independent of the surrounding environment.

### Presentational vs container components
- Presentational components are mostly concerned with generating some markup to be outputted.
- Container components are mostly concerned with the “backend” operations.

### State vs props
In a React component, props are variables passed to it by its parent component. State on the other hand is still variables, but directly initialized and managed by the component.
Props should never be changed in a child component.
Most of your components will just display some kind of information based on the props they received, and stay stateless.

### PropTypes
`import PropTypes from 'prop-types'`

These are the fundamental types we can accept:
- PropTypes.array
- PropTypes.bool
- PropTypes.func
- PropTypes.number
- PropTypes.object
- PropTypes.string
- PropTypes.symbol
- PropTypes.oneOf(['Test1', 'Test2'])
- PropTypes.oneOfType([PropTypes.string,PropTypes.number])
- PropTypes.instanceOf(Something)
- PropTypes.node

#### Requiring properties
Appending `isRequired` to any PropTypes option will cause React to return an error if that property is missing

### React Fragment
to add multiple elements to a component `<React.Fragment> </React.Fragment>`

### Events
Bind this in methods: in ES6 `this` is not defined unless you define methods as arrow functions

#### The events reference, a list
- Clipboard: onCopy,  onCut, onPaste
- Composition: onCompositionEnd, onCompositionStart, onCompositionUpdate
- Keyboard: onKeyDown, onKeyPress, onKeyUp
- Focus: onFocus, onBlur
- Form: onChange, onInput, onSubmit
- Mouse: onClick, onDrag, onDrop
- Selection: onSelect
- Touch: onTouchCancel, onTouchEnd, onTouchMove
- UI: onScroll
- Mouse Wheel: onWheel
- Media: onAbort, onCanPlay, onWaiting
- Image: onLoad, onError
- Animation: onAnimationStart, onAnimationEnd, onAnimationIteration
- Transition: onTransitionEnd

### Lifecycle Events
