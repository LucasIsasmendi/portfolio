# Redux tutorials

## Redux
[link](https://redux.js.org)

### introduction

The whole `state` of your app is stored in an object tree inside a single `store`.

The only way to change the state tree is to emit an `action`, an object describing what happened.

To specify how the actions transform the state tree, you write pure `reducers`.

Following in the steps of Flux, CQRS, and Event Sourcing, Redux attempts to make state mutations predictable by imposing certain restrictions on how and when updates can happen. These restrictions are reflected in the three principles of Redux.

#### Core Concepts:
- `state` is like a “model” except that there are no setters.
- `action` is a plain JavaScript object that describes what happened
- `reducers` are functions to tie state and actions together. Takes state and action as arguments, and returns the next state of the app.

#### Three Principles
1. **Single source of truth:** The `state` of your whole application is stored in an object tree within a single `store`.
2. **State is read-only:**  The only way to change the state is to emit an `action`, an object describing what happened.
3. **Changes are made with pure functions:** To specify how the state tree is transformed by actions, you write pure `reducers`.

### Basic Tutorial
In this guide, we'll walk through the process of creating a simple Todo app.

#### Actions
- are payloads of information that send data from your application to your store. They are the only source of information for the store. You send them to the store using `store.dispatch()`.
- must have a `type` property that indicates the type of action being performed. Other than type, the structure of an action object is really up to you.
- **Action Creators**: functions that create actions (return actions). You can use `bindActionCreators()` to automatically bind many action creators to a dispatch() function.

#### Reducers
- specify how the application's state changes in response to actions sent to the store. 
- keep your state as normalized as possible, without any nesting.
- is a pure function that takes the previous state and an action, and returns the next state.
- Keep it Pure. Things you should never do inside a reducer:
  - Mutate its arguments;
  - Perform side effects like API calls and routing transitions;
  - Call non-pure functions, e.g. Date.now() or Math.random().


### Advanced Tutorial

### Recipes


### Links

https://redux.js.org/introduction/learning-resources
