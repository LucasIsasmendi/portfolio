# Create react app
[create-react-app](https://facebook.github.io/create-react-app/)

- Supported Language Features
  - Exponentiation Operator (ES2016).
  - Async/await (ES2017).
  - Object Rest/Spread Properties (ES2018).
  - Dynamic import() (stage 3 proposal)
  - Class Fields and Static Properties (part of stage 3 proposal).
  - JSX, Flow and TypeScript.

## Steps For Setup 
Adding TypeScript
`yarn create react-app my-app --typescript`

Formatting Code Automatically (modify package.json)
`yarn add husky lint-staged prettier`

Getting Started with Storybook
- install: `npx -p @storybook/cli sb init`
- run: `yarn storybook`

check https://storybook.js.org/basics/writing-stories/

Analyzing the Bundle Size
- install: `yarn add source-map-explorer`
- run: `yarn run build && yarn run analyze`

Using HTTPS in Development
`HTTPS=true yarn start`

### Code Splitting
Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small chunks which you can then load on demand.
https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html

[With React Router](https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)

## Building Your App

### Adding Custom Environment Variables
`REACT_APP_NOT_SECRET_CODE=abcdef yarn start`

To define permanent environment variables, create a file called .env in the root of your project:

### Making a Progressive Web App
in `src/index.js` switching serviceWorker.unregister() to serviceWorker.register() will opt you in to using the service worker. It can make debugging deployments more challenging.
Check links for more detail

### Creating a Production Build
npm run build creates a build directory with a production build of your app. Inside the build/static directory will be your JavaScript and CSS files. Each filename inside of build/static will contain a unique hash of the file contents. This hash in the file name enables long term caching techniques.
- main.[hash].chunk.js: application code
- [hash].chunk.js: vendor code
- runtime~main.[hash].js: webpack runtime

## Testing
### Running Tests
Create React App uses Jest as its test runner. We recommend that you use a separate tool for browser end-to-end tests if you need them. They are beyond the scope of Create React App.

Filename Conventions: `__tests__` | `.test.js` | `.spec.js`

We recommend to put the test files (or __tests__ folders) next to the code they are testing so that relative imports appear shorter.

#### Testing Components
- Option 1: Shallow Rendering: If you’d like to test components in isolation from the child components they render, we recommend using shallow() rendering API from Enzyme.
- Option 2: React Testing Library: react-testing-library is a library for testing React components in a way that resembles the way the components are used by end users. It is well suited for unit, integration, and end-to-end testing of React components and applications.

#### Initializing Test Environment
```jsx
const localStorageMock = {
  getItem: jest.fn(),
  setItem: jest.fn(),
  removeItem: jest.fn(),
  clear: jest.fn(),
};
global.localStorage = localStorageMock;
```
#### Focusing and Excluding Tests
You can replace `it()` with `xit()` to temporarily exclude a test from being executed. Similarly, `fit()` lets you focus on a specific test without running any other tests.

#### Coverage Reporting
`yarn test -- --coverage`

#### Continuous Integration
- Travis CI: docs
- Circle CI: docs
- On your own environment: `CI=true yarn test`

If you know that none of your tests depend on jsdom, you can safely set --env=node, and your tests will run faster

### Debugging Tests
Debugging Tests in Visual Studio Code: check code

## Back-End Integration

### Proxying API Requests in Development
https://facebook.github.io/create-react-app/docs/proxying-api-requests-in-development

https://github.com/fullstackreact/food-lookup-demo

### Fetching Data with AJAX Requests
axios or fetch

### Title and Meta Tags
Generating Dynamic <meta> Tags on the Server

## Deployment
https://facebook.github.io/create-react-app/docs/deployment

- Azure
- Firebase
- GitHub Pages
- Heroku
- Now
- Surge