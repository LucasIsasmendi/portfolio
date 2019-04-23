# Hooks

> Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class.

[1. Introducing Hooks](#1.-introducing-hooks)
[2. Hooks at a Glance](#2.-hooks-at-a-glance)
[3. Using the State Hook](#3.-using-the-state-hook)
4. Using the Effect Hook
5. Rules of Hooks
6. Building Your Own Hooks
7. Hooks API Reference
8. Hooks FAQ

## 1. Introducing Hooks

### No Breaking Changes 
- Completely opt-in
- 100% backwards-compatible
- Available now
- There are no plans to remove classes from React
- Hooks don’t replace your knowledge of React concepts: props, state, context, refs, and lifecycle

### Motivation 
- It’s hard to reuse stateful logic between components
- Complex components become hard to understand
- Classes confuse both people and machines 

## 2. Hooks at a Glance
Hooks are functions that let you “hook into” React state and lifecycle features from function components

### Rules
- Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions.
- Only call Hooks from React function components or yur own custom Hooks. Don’t call Hooks from regular JavaScript functions. 
 
Use https://www.npmjs.com/package/eslint-plugin-react-hooks


### `useState`
useState returns a pair: the current state value and a function that lets you update it.

### `useEffect`
adds the ability to perform side effects from a function component. It serves the same purpose as componentDidMount, componentDidUpdate, and componentWillUnmount in React classes, but unified into a single API.
you’re telling React to run your “effect” function after flushing changes to the DOM.

### Building Your Own Hooks: `useSomething`
To reuse some stateful logic between components. 
Traditionally, there were two popular solutions to this problem: higher-order components and render props. Custom Hooks let you do this, but without adding more components to your tree.

### other Hooks
- `useContext`: lets you subscribe to React context without introducing nesting
- `useReducer`: lets you manage local state of complex components with a reducer

## 3. Using the State Hook
```js
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Tip: Using Multiple State Variables: `[something, setSomething]`