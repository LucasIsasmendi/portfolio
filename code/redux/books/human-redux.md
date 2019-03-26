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

##


