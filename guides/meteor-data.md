# Meteor Guide - DATA

## Testing
How to test your Meteor application

### Types of tests:
* Unit Test
* integration test
* acceptance/end-to-end test
* load/stress test

### Challenges of testing in Meteor
* **Client/server data**: 
* **Reactivity**: delay with changes impact, resolve with `Tracker.afterFlush()`  

### The ‘meteor test’ command
#### meteor test
* Load any file in our application (including in imports/ folders) that look like *.test[s].*, or *.spec[s].*
* Doesn’t eagerly load any of our application code as Meteor normally would.
* Sets the Meteor.isTest flag to be true.
* Starts up the test driver package
This is ideal for **unit tests** and **simple integration tests**.

#### meteor test --full-app
Additionally, Meteor offers a “full application” test mode. This enables you to write much more **complex integration tests** and also load additional files for **acceptance tests**.
* It loads test files matching *.app-test[s].* and *.app-spec[s].*.
* It does eagerly load our application code as Meteor normally would.
* Sets the Meteor.isAppTest flag to be true (instead of the Meteor.isTest flag).

### Driver packages: Recommended: Mocha
These packages don’t do anything in development or production mode.




