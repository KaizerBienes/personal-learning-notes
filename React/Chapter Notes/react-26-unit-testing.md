# Unit Testing

- writing automated testing
- testing react components

## Modules

- What is "Testing" and why?
- Understand Unit Tests
- Testing React Components and Building Blocks

## What? And Why?

- Manual Testing
  - Write code to implement <> Preview and Test in browser
  - Very important: You see what your users will see
  - Error prone: It's hard to test all possible combinations and scenarios
- Automated Testing
  - Write code that tests your code
  - You test the individual building blocks of your app
  - Very technical but allows you to test ALL building blocks at once

## Different Kinds of Automated Tests

- Unit Tests
  - Tests the indivdual building blocks in isolation (functions, components)
  - Projects typically contain dozens of hundreds of unit tests
  - The most common / important kind of test
- Integration Tests
  - Test the combination of multiple building blocks
  - Projects typically contain a couple of integration tests
  - Not easy to differentiate with Unit Tests
  - Also important, but focus on unit tests in most cases
- End-to-End (e2e) Tests
  - Testing complete scenarios in your app as the user would experience them
  - Projects typically contain only a few e2e tests
  - Important but can also be done manually (partially)

## What to Test

- What? + How?
  - What?
    - Test the different building blocks
    - Unit Tests: the smalles building blocks that make up your app
  - How?
    - Test success and error cases, also test case (but possible) results

## Required Tools and Setup

- We need a tool for running our tests and asserting the results
  - Jest
- We need a tool for "simulating" (rendering) out React app/components
  - React Testing Library
- Both tools are setup when via `create-react-app`

## Writing Tests

`test(<title>, <anonymous function>);`

- to run tests, `npm test`

## Write Tests - The Three "A"s

- Arrange - set up the test data, test conditions and test environment
- Act - run logic that should be tested (e.g. execute function)
- Assert - compare execution results with expected results

## Grouping Tests with Test Suites

- organize multiple tests into a single test suites
- Group via `describe`

```js
describe(<suite name>, () => {
  test(<test name>, () => {});
});

```

## Testing with State

```js
const buttonElement = screen.getByRole("button");
userEvent.click(buttonElement);
```

## Multiple components

- `getAllByRole` - get all under that specific role
- since fetch is asynchronous, we will not immediately get the results
- `findAllByRole` - returns a promise so it waits

## Mocking

```js
// creates a mock function
window.fetch = just.fn();
window.fetch.mockResolvedValueOnce({
  json: async () => [{ id: "p1", title: "First post" }],
});
render(<Async />);
```

## Jest - Delightful Javascript Testing
- not focused on just react
- general JavaScript testing tool
- mocking function

## React Testing Library
- testing react with that library
- complete API that you have available