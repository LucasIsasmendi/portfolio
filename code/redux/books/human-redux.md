# Human Redux
[link](https://read.reduxbook.com/)

## Content

**Introduction**

**Part I - Redux Basics**
- [1 What exactly is Redux, anyway?](#1-what-exactly-is-redux,-anyway?)
- [2 Actions and reducers: updating state](#2-actions-and-reducers:-updating-state)
- [3 Action Creators](#3-action-creators)
- [4 Middleware, Store Enhancers, and other fancy words!]
- [5 Using state from your Store in your views]
- [6 Selectors]
**Part II - Redux Patterns**
- 7 Decoupling views from data fetches
- 8 Routing
- 9 Reactors
- 10 Reliable apps
- 11 Persisting state locally
- 12 Handling authenticated requests
- 13 Redux bundles

**Appendix**
- 14 Betting on the Web
- 15 List of Referenced Examples

**Introduction**
Redux itself is a relatively simple idea, but as it turns out, using it to build real applications is not all that simple. But to be fair, Redux never claimed to be particularly opinionated or complete. After all, it's just a state management library, not an application framework.

Build ambitious web apps while managing complexity.

## 1 What exactly is Redux, anyway?
> State management is the hardest problem in building UI applications.

`state` is what mode the app is in right now. 

The basic idea is to just isolate application state from your view layer.

We'd need to do something like this:
1. Define our initial application state when the app starts.
2. When something happens, we need to update our application state accordingly.
3. We re-render our view with the new state.

If you're not familiar with Preact, you can think of it as a "React Lite." It implements nearly the same API and is only 3.5kb minified and gzipped, which about 1/10th the size of React.

Redux is simply that: a pattern for storing, updating, and reading application state outside of your components.

## 2 Actions and reducers: updating state
In Redux all changes to the application state happen via an `action`. An **action** in Redux terms is simply **a thing that happened** (fact) expressed as a plain JavaScript Object: `{type: 'AUTH_TOKEN_RECEIVED', payload: 'eyJpZCI6Ii1LaF9oxzNzMwfQ'}`. They must have a `type` property and should be atomic (self-contained). The use of `payload` as the name of the property that contains relevant information is a personal choice.

In Redux `actions` are "dispatched" to the store to let it know which things happened: `store.dispatch(action)`

> hack to keep yourself writing actions this way is to make all your action types past-tense.

### Reducers
It's a plain old function you write that takes the current `state`, and the `action` to be processed, and returns the state as it should be, based on that action occurring.

Reducers in Redux have a few simple rules:
1. Always return the state, even if you didn't change it, and even if it's just null. You may not return undefined.
2. If you change state at all, replace it.
3. Every reducer processes every action even if it doesn't do anything with it.

Redux store only takes one reducer, the `rootReducer`.
The `combineReducers` function is a helper included with the Redux library. It takes an object where each key is a name, each value is a reducer function, and it returns a single reducer that combines them all into a single reducer.
Every reducer should have an initial state (default function parameters).

> Every reducer will get called for every action and whatever it returns will be the new state tracked by that reducer

#### Redux reducer rule #1: You may never return `undefined`
So you always have to return something, even if it's `null`.

#### Redux reducer rule #2: If you change it, replace it (...spread).
state is "immutable". Use assign to copy properties from one object to other:
`const other = Object.assign({}, one)` or with "Object spread": `const other = { ...one, diff: 'replacement' }`

#### Understanding immutability
Instead of looping through an object to check for differences, we can use immutability and check if something is the same reference or a new object reference. That’s the basic idea of “immutability.” It’s doesn't necessarily mean that we make objects that are somehow frozen, so they cannot be changed. It means that we don't change the old objects, we replace them instead.

### Ok, so how does the app know if the state was modified?
Being able to know when something has happened is made possible via the third method: `.subscribe()`. It returns a function that you can call to cancel the subscription, there is not unsubscribe method.

## 3 Action Creators

### Chapter recap
1. An action creator is merely a function that returns an action object.
2. Redux includes a utility function called bindActionCreators for binding one or more action creators to the store's dispatch() function.
3. Calling an action creator does nothing but return an object, so you have to either bind it to the store beforehand, or dispatch the result of calling your action creator.

Example of action creator:
```ts
const doAddToDoItem = payload => ({ type: 'TODO_ADDED', payload })
store.dispatch(doAddToDoItem('be awesome today'))
```
> Using action creators allows us to expresses the intent without having to worry about the implementation (async or not).

To binding an action creator and a store dispatch, we can use `bindActionCreators()`
```ts
const boundToDoActions = bindActionCreators(
  {
    add: doAddToDoItem,
    remove: doRemoveToDoItem,
    clear: doClearToDos
  },
  store.dispatch
)
```
> Remember: Only dispatching actions can update your application state.

## 4 Middleware, Store Enhancers, and other fancy words!
### Chapter recap
1. Middleware lets us reach into the dispatch process in Redux to change how it works.
2. You can write middleware that enables you to dispatch a function instead of a plain object.
3. If we write action creators that clearly express intent, there's no need to update how they are called even if we decide to change how they work. Encapsulation for the win!
4. Store enhancers are a formal mechanism for adding capabilities to Redux itself. Most people will never need to write one.
5. To use middleware in Redux, we use the `applyMiddleware()` function exported by the Redux library.
6. applyMiddleware is itself a store enhancer that lets us change how dispatch() works.
7. There's a library called `redux-thunk` written by Redux's creator that allows you to dispatch functions as was shown in the example.


- A store enhancer, adds some additional capabilities to the store. It could change how reducers process data, or how dispatch works.
- Middleware is a function that lets us "tap into" what's happening inside Redux when we dispatch an action, is added to our Redux store using a store enhancer that enhances the .dispatch() function.

> Note, createStore can receive 2 or 3 arguments: `createStore(rootReducer, preloadedState, enhancer)` or `createStore(rootReducer, enhancer)`

There's only one enhancer that's included in the Redux library itself, `applyMiddleware()`, and we can pass it middleware functions to inject functionality into `.dispatch()`.
```ts
const store = createStore(rootReducer, applyMiddleware(reduxLogger, reduxThunk))
```
The function inside applyMiddleware returns a function, that returns a function.
Example of middleware that does nothing:
```ts
const noOpMiddleware = store => next => action => {
  return next(action)
}
```
Only the inner-most function runs on each dispatch, the outer two functions help set things up.

```ts
const doClearToDos = () => dispatch => {
  dispatch({ type: 'CLEAR_TODOS_STARTED' })
  fetch('/todos', { method: 'DELETE' })
    .then(() => {
      dispatch({ type: 'CLEAR_TODOS_FINISHED' })
    })
    .catch(error => {
      dispatch({ type: 'CLEAR_TODOS_FAILED' })
    })
}
```
If we do async, we will need middleware to let us dispatch a function (dispatch only receives plain object with a type property): This is an example of the logic behind `redux-thunk`.
```ts
const asyncMiddleware = store => next => action => {
  if (typeof action === 'function') {
    return action(dispatch)
  }
  return next(action)
}
const store = createStore(rootReducer, applyMiddleware(asyncMiddleware))
```
Middleware examples in [official doc](https://redux.js.org/advanced/middleware).

## 5 Using state from your Store in your views
### Chapter recap
1. We use a binding library, such as `react-redux` to get state from our store into our components.
2. Creating a global store variable is fragile and a bit messy. But, we need a way to give components access to the store we create. We can do this using the context API in React / Preact.
3. The `<Provider />` component is part of the react-redux library and simply puts the store into the context which makes it available to its child components. This component does nothing else.
4. `connect()` is how we can get state and dispatch actions from the store to our components.
5. We write a `select()` function that takes the application state and returns an object of props for the component.
6. The object that is returned by your select function should only include what the component needs to know. If it's too general, it will re-render even when it's unnecessary.
7. We can use the second argument to `select()` to pass an object of action creators we want to bind to the store and turn into props the component can call.
8. We can use `connect()` to make action creators available to our components with a minimal performance impact.

This idea of maintaining application state separate from the UI enables a lot of great things.
In react-redux we have component `<Provider>` and function `connect()`

### The Provider
It "Put Store Into React Context API" where child components can access it.
```ts
<Provider store={getStore()}>
  <RootComponent />
</Provider>
```

### connect()
connect a given component in a way that allows it access to the store (it calls subscribe internally).
```ts
const MyRegularOldComponent = ({ query, results }) => (
  <div>
    query: {query}
    <ul> {results.map(..<li>..)}</ul>
  </div>
)
// Use `()` to return an object from an arrow function.
const select = appState => ({
  results: appState.results,
  query: appState.query
})
export default connect(select)(MyRegularOldComponent)
```
#### Understanding how connected components update
Every time we dispatch an action to the store, each connected component will run its `select()` and compare the values of each key in the object that is returned using a strict equality check `===`. If any of the values are different, the component will re-render.

> You may be wondering how `connect()` is implemented; it creates something called a "higher order component" that wraps your component in another component that uses a `shouldComponentUpdate` life-cycle method that will make this shallow comparison and return `false` if nothing has changed.

#### What about dispatching actions?
With `connect("state", "actions")` you give your components the ability to dispatch actions. If you don't specify the second argument (`mapDispatchToProps`) to `connect()`, your component will receive `dispatch` by default. 

```ts
const actions = { doClearToDos, doAddToDo }
export default connect(null, actions)(OurComponent)
```
#### Putting it all together
A connected component that uses both `select` and `actions` or `mapStateToProps` and `mapDispatchToProps`

Consider using select and actions as a way to organize what extra props get passed to our connected component.

This component will re-render when either:
1. Any parent component that renders this one passes it a different prop
2. Any of the values on the object returned by your select functions are different.

> Be aware of variable shadowing: Avoid the issue altogether by not re-using names when building your actions object.

#### Ok, so when do I use connect?
Extremes are bad. Typically you should connect all the major sections of the app.

#### Selecting just what you need, nothing more.
The key thing to understand is that what you select determines whether or not something will be re-rendered. By being smart about what we connect we're not wasting cycles re-rendering things that don't need to be.

#### Connecting just actions
if all you want to do is give your component access to a few action creators, pass null as the first argument to `connect()`, it will not subscribe to the Redux store at all

#### When not to connect a component
You probably don't want to connect a component that will be rendered many times in a list. 
As I've said before, the goal of this book is not to replace the official documentation so you should know there are more parameters and options you can pass to connect, I would suggest limiting yourself to this subset of functionality to help you maintain a simpler, more maintainable codebase.