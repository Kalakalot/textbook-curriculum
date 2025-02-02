# JavaScript Unit Testing with Jest

## Learning Goals

By the end of this lesson, students should be able to...

- Describe what makes testing JavaScript web apps complex
- Run Jest tests from the command line
- Enumerate the parts of a Jest test file

## Testing in JavaScript

Automated testing is an essential part of the process of writing good software. [Study](http://ieeexplore.ieee.org/abstract/document/1201238/) [after](http://www.sciencedirect.com/science/article/pii/S0950584903002040) [study](https://link.springer.com/article/10.1007/s10664-008-9062-z) have shown that following a strong test-driven development workflow reduces bug counts, increases code maintainability and helps teams deliver projects on time (and spend less time fixing them later).

While JavaScript is no exception to this rule, there are a few factors that make testing front-end web applications a little more difficult than what we've seen so far:

- Front-end JavaScript code is designed to run in the browser and often takes advantage of built-in browser mechanisms. This makes tests difficult to run automatically from the terminal.
- JavaScript applications often talk to an external API.
- Many JavaScript apps are built around manipulating the DOM and responding to user events, which are difficult to simulate and verify.

In short, most interesting JavaScript code is full of *external dependencies*. Much of the effort involved in testing a JavaScript app goes into mocking, eliminating or working around these dependencies.

### Testing Strategy

We'll be using a test suite called [Jest](https://facebook.github.io/jest/docs/en/getting-started.html) (maintained by Facebook). Jest is available as an npm package and commonly used in testing React & pure JavaScript applications.

Jest tests are run from the command line using `npm test` (more on this later).  It will be very reminiscent of model testing in Rails, and in fact we will focus our JavaScript testing on business logic methods. The testing techniques we learn here will also transfer directly to any command-line JavaScript programs you write, including something like a Node-Express server.  Later we will introduce testing in React and write tests which interact with DOM elements.

### Setup

We will install the Jest Command-line Interface globally.  This will allow us to run our tests from the terminal similar to how we tested in Ruby.  To install the Jest CLI execute the command:

```bash
$ npm install -g jest-cli
```

### Exercise

Our first testing exercise will consist of writing tests for the Pangram whiteboarding problem.  A Pangram is a sentence which uses every letter of the alphabet at least once.

#### Questions

With a neighbor devise the following testing scenarios in English:

1.  Create a nominal success-case input for a Pangram function? (A google search for "What is a pangram?" may be helpful.)
1.  Create a nominal failure case for a Pangram function.  This should be an input which is clearly not a pangram.
1.  Create an edge-case failure case for a Pangram function.  This should be "almost" a Pangram.
1.  Come up with an additional input you should test.

Next we will write tests for our `isPangram` function.

1. Clone this repo: [`https://github.com/AdaGold/JS-Testing`](https://github.com/AdaGold/JS-Testing)
1.  Examine the README and follow the instructions to install the Jest and node modules needed to run the app.

Then look through the code in the `src/is_pangram.spec.js` file, and see if the tests described match your predictions. To run the tests, type `npm test` at the console.

Similar to Rails, the `spec` directory has the same structure as the `src` directory. Each file `src/name.js` may have a corresponding `spec/name.spec.js`.

### Anatomy of a Test

There are already some tests written for the `isPangram` method, so let's begin there. Open up `spec/is_pangram.spec.js`.

Because Jest is a behavior-driven testing language, testing using Jest for JavaScript is fairly similar to what we saw using Minitest for Ruby and Rails. As always, there are some specific differences.

Each test should have these components to describe the test behavior.

#### `describe` Blocks

Describe blocks should create test groupings based on _objects_ and _functions_.  They are optional, but provide some readability and organization.  Like Minitest you can also nest `describe` blocks.

We will use one `describe` block for our overall `isPangram` function.

Each `describe` function has two parameters. The first is the description of the `describe` block and the second is the function which contains the actions/logic. Note that we add a `;` at the end of each `describe` block.

```javascript
describe('isPangram()', () => {

});
```

#### `test` Blocks

`test` blocks should define one distinct test. The description that goes along with the `test` block should describe the specific scenario that you are testing.

```javascript
describe('isPangram()',  () =>  {
  test('isPangram is defined', () => {

  }
  ...
});
```

Similar to what you have in Minitest you can use `it`, in place of `test` if `it` (hic) makes you feel more comfortable, but the Jest documentation uses `test`, and so Ada's examples will as well.

#### `expect` Statements

Expectations should be the "meat and potatoes" of your tests, inside of your `test` blocks. Each test has at least one `expect` statement to ensure the behavior is as expected.

The syntax of `describe` and `test` is pretty similar to Minitest (at least as similar as Ruby and JS can be), but the **expectation** methods are named a little differently.  These methods like `toBeDefined` function serve the same role as the matchers in Minitest.  They define a condition your test is looking to ensure.  `toBeDefined` is used to ensure that the argument to `expect` is not `undefined`.  You can find a table of expectations below.  There may be times where you have **no** expectations in a test and your test is simply verifying that a set of commands function without an error.

```javascript
describe('isPangram function',  () =>  {
  test('isPangram is defined', function() {
    expect(isPangram).toBeDefined();
  });

  ...
});
```

And that's all a test is. Go ahead and add another expect statement to the spec file, but this time make sure it will fail (e.g. `expect(false).toEqual(true);`). Then re-run the tests, just to see what a failure looks like. Fix the test and run it again. Not too different from Minitest, right?

## Skipping Tests

Tests can be skipped by changing `test(...` to `test.skip(...`.  Go ahead and change

```javascript
    test('isPangram() is defined', () => {
```

to:

```javascript
    test.skip('isPangram() is defined', () => {
```

## Exercise With A Neighbor

Read through the couple of implemented tests in `is_pangram.spec.js` and the stubbed out tests.

Now use a test-driven development workflow to implement the `isPangram()` function and complete the stubbed-out tests. Remember the TDD cycle: pseudocode-red-green-refactor!

### Going Further

Now write your own test in the section provided.

## Optional Exercise

If you feel you need more practice, create tests for an `isPalindrome` function.  Then write code to solve the problem.  A palindrome is a word or phrase spelled backwards the same as forwards and is further described in the README.

## Matchers

Just like in testing with Ruby and Rails, Jest has a number of **matchers** that allow us to construct our tests. Below are the most common:

| Code | Meaning     |
| :------------- | :------------- |
| `expect(x).toEqual(y);` | Compares objects `x` and `y` and passes if they are equivalent  |
| `expect(x).toBe(y);` | Compares objects `x` and `y` and passes if they are the same object, do **not** use this to test two objects or arrays for equality. |
| `expect(x).toMatch(pattern);` | Compares `x` to string or regular expression pattern and passes if they match |
| `expect(x).toBeDefined();` | Passes if `x` is **not** undefined |
| `expect(x).toBeNull();` | Passes if `x` is null |
| `expect(x).toBeTruthy();` | Passes if `x` evaluates to true |
| `expect(x).toBeFalsy();` | Passes if `x` evaluates to false |
| `expect(x).toContain(y);` | Passes if `array` or `string` `x` contains `y` |
| `expect(x).toBeLessThan(y);` | Passes if `x` is less than `y` |
| `expect(x).toBeGreaterThan(y);` | Passes if `x` is greater than `y` |
| `expect(fn).toThrow(e);` | Passes if a function, `fn`, throws exception `e` when executed |

The Jest docs also have some great examples of how to use the different matchers.
[https://facebook.github.io/jest/docs/en/expect.html](https://facebook.github.io/jest/docs/en/expect.html)


## What Have We Accomplished?

- Discuss the theory of testing JavaScript web applications
  - *External dependencies* such as the DOM, user events and APIs make it difficult
- Evaluate strategies for dealing with these challenges
  - *Mock* the DOM
  - Separate business logic from display logic and test it in isolation
- Examine Jest's BDD DSL
  - `describe` blocks for suites
  - `test` blocks for individual tests
  - `expect` statements (with matchers) to verify particular conditions
- Practice writing tests with Jest

## Additional Resources

- [Jest documentation](https://facebook.github.io/jest/docs/en/getting-started.html)
- [Selenium](http://www.seleniumhq.org/)
- [Testing React Apps](https://facebook.github.io/jest/docs/en/tutorial-react.html)
- [Jest Examples Folder](https://github.com/facebook/jest/tree/master/examples)
- [Karma a way to run tests through the browser](https://karma-runner.github.io/2.0/index.html)
- [Free Code Camp Jest Introduction](https://medium.com/p/b71c27b0a795#d8af)
