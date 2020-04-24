[![Pursuit Logo](https://avatars1.githubusercontent.com/u/5825944?s=200&v=4)](https://pursuit.org)

# Testing Express Applications (Integration Testing)

Using supertest, jest, and express to run integration tests on the backend.

## Outline

* code overview
* recap integration testing
* use rock paper scissors express app
* introduce supertest
* split app and server.js
* set up request 
* 


## Integration testing

## Setup

Clone down [this repo](https://github.com/joinpursuit/FSW-RockPaperScissors-Express) and follow the setup instructions.

Take a look around at the code - you may have already built something like this! It's a fairly simple express app that keeps track of rock paper scissors throws. 

## Code Overview

Spend 10 minutes looking through the code. There is some new stuff in here that we haven't seen before:

* `knex.js` is a query builder for sql. It lets us write queries by chaining JS functions together, rather than just using plain strings. Knex also handles the connection to the database for us, just like `pgpromise` does.
* `objection` is an ORM, which stands for `object relational mapping`. Using objection, we declare classes that represent our database tables. These are called `models` because they model the shape and behavior of the database. Right now, there's only one model which is in `Throw.js`.
* `migrations` - a migration is simply a set of instructions to tell the database to change something about its structure. We use migrations to create the table that holds the throws. It also describes the shape of the table - sometimes called a `schema`. Normally, this is done manually from the command line by writing some sql like `CREATE TABLE throws (col1 datatype, col2 datatype)`. 
* `seeds` - a seed file contains sample data to populate the database with, so that it's not empty. Seed files can also be used to reset the database to a default state, which this one does.
* `controllers` are just a way of moving our logic for each route into its own file. It's a pattern that is useful to follow, because as our application grows in size, it helps reduce some of the complexity.

The only things we really need to understand today are seeds - we'll use them in our testing. 

## Integration testing

Integration testing verifies that different parts of the application work together - that the `contract` between pieces of code doesn't break. 

Today we'll be simulating making requests from the front end (as if we were the browser making GET requests). The requests will hit our express application which talks to the database and creates a row in a table. We'll verify that all of this works as expected.

## Installation

We're going to use a library called `supertest` which is based off another library called `superagent`, which is similar to `axios`.

The reason we use `supertest` is that it has assertions built in - so we can tell it to `.expect()` a specific type of response. It also knows how to talk to express directly, which saves us from having to start express ourselves when the tests run.

We'll also be using `jest` to actually run our tests. So let's install everything:

```
npm i --save-dev jest supertest 
```

## Test setup

In order for express to run properly, one of the things we need to do is split up our `app.js`. Supertest needs to be able to talk to the app and then have it exit when its done running tests - so we need to move the `.listen()` call to another file. 

* Create a new file `server.js` in the root
* Remove the `.listen()` and export the `app` variable from `app.js`

```js
module.exports = app
```

* In `server.js`:

```js
const app = require('./app')

app.listen(3000, () => {
  console.log('listening on 3000')
})
```

Now when we want to run our application normally, we have to execute `server.js` instead of `app.js`, and `app.js` will be used for testing only.

One last thing: create an `app.test.js` file.




## Resources

* [testing express with jest and supertest](https://www.albertgao.xyz/2017/05/24/how-to-test-expressjs-with-jest-and-supertest/)
* [another article with the same title](https://dev.to/nedsoft/testing-nodejs-express-api-with-jest-and-supertest-1km6)