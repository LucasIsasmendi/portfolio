
# Main Concepts

- [2. Introducing JSX](#2.-introducing-jsx)
- [3. Rendering Elements](#3.-rendering-elements)
- [4. Components and Props](#4.-components-and-props)
- [5. State and Lifecycle](#5.-state-and-lifecycle)
- [6. Handling Events](#6.-handling-events)
- [7. Conditional Rendering](#7.-conditional-rendering)
- [8. Lists and Keys](#8.-lists-and-keys)
- [12. Thinking in React](#12.-thinking-in-react)


## 2. Introducing JSX
```jsx
const element = <h1>Hello, world!</h1>;
```
It is called JSX, and it is a syntax extension to JavaScript. We recommend using it with React to describe what the UI should look like. 

### Embedding Expressions in JSX 
```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

Warning: Since JSX is closer to JavaScript than to HTML, React DOM uses camelCase property naming convention instead of HTML attribute names.
For example, class becomes className in JSX, and tabindex becomes tabIndex.

## 3. Rendering Elements
> Elements are the smallest building blocks of React apps. Are what components are “made of”, are immutable.

To render a React element into a root DOM node, pass both to `ReactDOM.render()`

### React Only Updates What’s Necessary 
React DOM compares the element and its children to the previous one, and only applies the DOM updates necessary to bring the DOM to the desired state.

In our experience, thinking about how the UI should look at any given moment rather than how to change it over time eliminates a whole class of bugs.

## 4. Components and Props
> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

[API -> React.Component](../api/React.md#React.Component)

Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

> Always start component names with a capital letter.

for more details, check [JSX conventions](../advanced/JSX-in-depth.md)

### Composing Components 
Components can refer to other components in their output.

### Extracting Components
split components into smaller components. Name props from the component’s own point of view rather than the context in which it is being used.
A good rule of thumb is that if a part of your UI is used several times, or is complex enough on its own, it is a good candidate to be a reusable component.

### Props are Read-Only 

All React components must act like **pure functions** (they do not attempt to change their inputs, and always return the same result for the same inputs) with respect to their props.

## 5. State and Lifecycle
[API -> React.Component](../api/React.md#React.Component)

State is similar to props, but it is private and fully controlled by the component.

Changes from `this.props` to `this.state`: [Example](https://reactjs.org/docs/state-and-lifecycle.html#converting-a-function-to-a-class)

### Adding Lifecycle Methods to a Class 
In applications with many components, it’s very important to free up resources taken by the components when they are destroyed.
- set up is called **mounting** in React: `componentDidMount()`: Runs after the component output has been rendered to the DOM.
- clear is called **unmounting** in React: `componentWillUnmount()`

### Using State Correctly

#### Do Not Modify State Directly
Instead, use `setState()`.
The only place where you can assign `this.state` is the constructor.

#### State Updates May Be Asynchronous 
Because `this.props` and `this.state` may be updated asynchronously, you should not rely on their values for calculating the next state.
To fix it, use a second form of `setState()` that accepts a function rather than an object. 

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

#### State Updates are Merged 
your state may contain several independent variables that you can update them independently with separate `setState()` calls

### The Data Flows Down 
Neither parent nor child components can know if a certain component is stateful or stateless, and they shouldn’t care whether it is defined as a function or a class.

This is why state is often called local or encapsulated. It is not accessible to any component other than the one that owns and sets it.

> If the behaviour of a component is dependent on the state of the component then it can be termed as **stateful** component and if behaviour is independent of its state then it can be a **stateless** component.

A component may choose to pass its state down as props or as user-defined components to its child components

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

## 6. Handling Events
```md
/*HTML*/
<button onclick="activateLasers()">
  Activate Lasers
</button>
/*React*/
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

- You cannot return false to prevent default behavior in React. You must call `preventDefault`
- When you define a component using an ES6 class, a common pattern is for an event handler to be a method on the class.

You have to be careful about the meaning of this in JSX callbacks. In JavaScript, class methods are not bound by default. 
```js
// This binding is necessary to make `this` work in the callback
this.handleClick = this.handleClick.bind(this);
```
This is not React-specific behavior; it is a part of how functions work in JavaScript. Generally, if you refer to a method without () after it, such as onClick={this.handleClick}, you should bind that method.
If calling bind annoys you, you can use experimental [public class fields syntax](https://babeljs.io/docs/en/babel-plugin-proposal-class-properties) to correctly bind callbacks
```js
handleClick = () => { console.log('this is:', this);}
```

### Passing Arguments to Event Handlers 
Inside a loop it is common to want to pass an extra parameter to an event handler.
```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

## 7. Conditional Rendering
> In React, you can create distinct components that encapsulate behavior you need. Then, you can render only some of them, depending on the state of your application.

Returning null from a component’s render method does not affect the firing of the component’s lifecycle methods. For instance componentDidUpdate will still be called.

## 8. Lists and Keys


## 12. Thinking in React
https://reactjs.org/docs/thinking-in-react.html

### Step 1: Break The UI Into A Component Hierarchy (top-down)
### Step 2: Build A Static Version in React (bottom-up)
- **props** are a way of passing data from parent to child. **state** is reserved only for interactivity.
- React’s one-way data flow (also called one-way binding) keeps everything modular and fast.

more about:
- [components and props](#4-components-and-props)
- [state and lyfecycle](#5-state-and-lifecycle)

### Step 3: Identify The Minimal (but complete) Representation Of UI State 
Figure out the absolute minimal representation of the state (interactive) your application needs and compute everything else you need on-demand. 

Some questions:
1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it remain unchanged over time? If so, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

### Step 4: Identify Where Your State Should Live
Which component mutates, or owns, this state.

Remember: React is all about one-way data flow down the component hierarchy. It may not be immediately clear which component should own what state. 

### Step 5: Add Inverse Data Flow 
