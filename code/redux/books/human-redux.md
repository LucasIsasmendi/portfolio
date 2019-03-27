# Human Redux
[link](https://read.reduxbook.com/)

## Content

**Introduction**

**Part I - Redux Basics**
- [1 What exactly is Redux, anyway?](#1-what-exactly-is-redux,-anyway?)
- [2 Actions and reducers: updating state](#2-actions-and-reducers:-updating-state)
- [3 Action Creators]
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

## 6 Selectors
### Chapter recap
1. Selectors are an official, unofficial part of Redux. They are how we read state from our app.
2. Anytime you find yourself updating a "calculated" or "derived" value as part of handling a particular action type in a reducer, you should probably be deriving that answer using a selector instead.
3. A demo of deriving complex data using selectors can be found in the mortgage calculator example here: https://reduxbook.com/mortgage
4. Selectors can potentially be run a lot so they should be as efficient as possible to avoid unnecessary work.
5. Reselect lets us derive increasingly complex answers while containing complexity. These patterns allow us to efficiently answer incredibly complex questions of our state by isolating chunks of logic into their separate selectors.
6. Selector composition is much like a spreadsheet, where we simply reference other cells instead of having to copy and paste their formula everywhere we want to use it.
7. A runnable example of nested selectors is located at: https://reduxbook.com/selectors



**Part II - Redux Patterns**

## 7 Decoupling views from data fetches
### Chapter recap
1. Coupling data fetching and presence of a given component on the page is tempting, but somewhat limiting.
2. In the context of a PWA, you want to make that app have a life of its own, regardless of the current internet connection. Doing this often involves explicitly managing locally cached data separate from the UI logic in your app.
3. Pre-fetch data you know you'll need regardless of current components on the page. We did this at Starbucks, and it worked great.



## 8 Routing
### Chapter recap
1. Clientside routing is a form of application state.
2. Most routing libraries want to "own" URL state, which puts them inherently at odds with Redux.
3. We actually need a two-way binding between the browser's URL and our application state.
4. We can do this by listening for popstate in the browser and using the new URL to update our Redux store and by subscribing to the store and updating the browser if it doesn't match what our Redux state says it should be.
5. A runnable example showing minimal routing can be found here: https://reduxbook.com/minimal-routing
6. We can extend this approach to extract route parameters by using selectors and a small route matching library.




## 9 Reactors
### Chapter recap
1. I introduced the idea of what I call a "reactor" which is just a selector function that either returns nothing, or returns something that can be dispatched.
2. Whenever you use the current state to trigger a dispatch you increase the risk of creating loops. To avoid loops when using this pattern, we must ensure that the action that our reactor returns will immediately causes a state change that the reactor checks for. If not, you'll have infinite loops.
3. The best way to debug infinite loop scenarios is to insert a debugger statement inside the if statement in your reactor function so you can inspect the conditions that are causing it to dispatch again.
4. We can track "app time" in our Redux state by adding a simple reducer that is just Date.now. This effectively timestamps each state change.
5. We can select app time in our selectors to keep selectors deterministic and pure, therefore easy to test.
6. If we want our app to be able to react to the time stored in Redux, we need to make sure something is dispatched every so often. We can solve this by adding an idle action that is called on a debounce to ensure something will be dispatched at the interval we set.
7. We can optionally turn off idling when the tab is not active by using requestAnimationFrame. We can also make sure the browser isn't busy with a more important task by using requestIdleCallback.
8. We discussed redux-saga and redux-loop and what I consider to be their primary weakness: they use your actions to cause side-effects instead of using your application state to inform what should happen next.


## 10 Reliable apps
## 11 Persisting state locally
## 12 Handling authenticated requests
## 13 Redux bundles