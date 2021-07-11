# Components

## React Core Syntax and JSX
  - React makes comple, interactive and reactive user interfaces simpler
  - React is all about "Components"
    - because all user interfaces are made up of components

## Component
  - Reusability
    - Don't repeat yourself
  - Separation of concern
    - Don't do too many things in one and the same place
  - React allows you to create re-usable and reactive components - Declarative Approach
  - Define the target state(s) and let React figure out the actual JS DOM instructions

## How is a component built?
  - HTML, CSS, and JavaScript - combined to components together to build the UI
  - Build your own, custom HTML elements

## Component Notes
  - Importing javascript should not have js
  - Naming conventions is to ProperCase the components
  - Only one root element per return!

  ```js
  import App from './App'; // instead of './App.js'
  ```

### JSX
  - transforming to browser friendly code
  - `Sources` tab in Developer Tools to show the filesystem
  - Transformation steps is done by `React`
  - using variables inside JSX
  ```js
  function Example () {
    const exampleText = 'hello world!';
    return (
      <div>
        <h2>{exampleText}</h2>
      </div>
    );
  }
  ```

  - raw jsx instead of compiled
  ```js
  // React.createElement(<element>, <parameters>, <...children>)
  return React.createElement(
    'div',
    {},
    React.createElement('h2', {}, "Let's get started!"),
    React.createElement(Expenses, {items: expenses})
  );

  // equal to
  return (
    <div>
      <h2>Let's get started</h2>
      <Expenses items={expenses} />
    </div>

  );
  ```

### How React works
  - Build your own custom elements
  - React is all about "Components" using a declarative approach
    - Define a desire target state and then React creates the DOM instructions based on its optimizations
    - Versus `imperatrive` which is doing multiple step by step JS code (e.g. document.createElement / document.getElementById().addElement() etc.)
  - Only the topmost component is rendered

### Props
  - passing data around from one component to another
  - much more preferrable than state
  ```js
  // in jsx
    function Example() {
    return (
      <ExpenseItem
        title={expenses[0].title}
        amount={expenses[0].amount}
        date={expenses[0].date.toISOString()}
      ></ExpenseItem>
    );
  }

  // in expenseItem
  function ExpenseItem(props) {
    return (
      <div>{props.date}</div>
      <div className="expense-item__description">
          <h2>{props.title}</h2>
          <div className="expense-item__price">{props.amount}</div>
      </div>
    );
  }
  ```


### Concept of "Composition"
  - building stuff from small building blocks
  - shell around any other kind of content
  - composing by isolating duplicate parts of the code

```css
// css
.card {
    border-radius: 12px;
    box-shadow: 0 1px 8px rgba(0, 0, 0, 0.25);
}
```

```js
import './Card.css';

function Card(props) {
    const classes = 'card ' + props.className;
    // combined card css and the className of what we want to add
    return <div className={classes}>{props.children}</div>;
}

export default Card;
```