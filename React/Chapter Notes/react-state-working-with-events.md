# Handling Events
- Updating the UI and Working with "State"
- Closer Look at Components and State

### Events
- events are handlers of actions that happen during specific events
- https://developer.mozilla.org/en-US/docs/Web/API/Element
- example
```js
// in jsx block
return (
    <button onClick={() => console.log("Click!")}>Change Title</button>
);

// when external function
const clickHandler = () => {
    console.log('Clicked!!');
};

return (
    <button onClick={clickHandler}>Change Title</button>
);
```
- ends with `Handler` when it is attached to an event listener (convention but it depends)

### Working with States
- define values as states which should reflect to those that are below
```js
import React, { useState } from 'react';

const Example = () => {
    const [title, setTitle] = useState(props.title);

    const exampleClickHandler = () => {
        setTitle('Updated!');
    };

    return (
        <button onClick={exampleClickHandler}>Change Title</button>
    );
}
```
- must be called inside main exported functions; cannot be called inside handler functions or outside the exported functions
- always returns two elements, one that holds the value, and another for setting the value
- uses setters instead of direct assignment (`=`) as using setters will trigger a rerender for React
- setState is not handled in real time but is scheduled (probably in the event loop)
- uses a const since we are not reassigning the value and the whole function is rerendered anyway

### Adding Form Inputs
- `onChange` vs `onInput`; `onChange` handles dropdowns as well
- automatically passes the `event` parameter which contains values

```js
const ExpenseForm = () = {
    const titleChangeHandler = (event) => {
        console.log(event.target.value);
    };

    return (
        <input type="text" onChange={titleChangeHandler} />
    );
}
```

### Setting Multiple States
- set multiple states
- alternatives
- CAVEAT: must update all values as those will be lost if that is the case
```js
const [userInput, setUserInput] =  useState({
    enteredTitle = '',
    enteredAmount = '',
    enteredDate = ''
});

const titleChangeHandler = (event) => {
    // old way
    // setEnteredTitle(event.target.value);

    // naive way
    setUserInput({
        ...userInput,
        enteredTitle: event.target.value
    });
    // since states are scheduled in the loop, we should use the "safer" way which uses the previous state
    setUserInput((prevState) => {
        return {
            ...prevState,
            enteredTitle: event.target.value
        }
    });
};
```

### Submitting forms
- form refreshes when the submit is clicked
- to prevent this, we can disable it using the example below
```js
const Example = (props) => {
    const submitHandler = (event) => {
        event.preventDefault();
    };

    return (
        <form onSubmit={submitHandler}>
            <button type="submit">Submit</buiton>
        </form>
    );
}
```

### Child to parent passing data (Bottom-up)
- passed as props towards the parent

```js
// parent
    const saveDataHandler = (enteredData) => {
        const data = {
            ...enteredData,
            id: Math.random().toString()
        };
        console.log(data);
    };
const ParentComponent = () => {
    return (
        <ChildComponent onSaveData={saveDataHandler}/>
    );
}

// child
const ChildComponent = (props) => {
    const submitHandler = (event) => {
        event.preventDefault();

        props.onSaveData(); // trigger the function
    }

    return (
        <form onSubmit={submitHandler}>
        ...
        </form>
    );
}
```

### Lifting State Up
- for example
    - data / state is generated in a child component
    - data / state is needed in its sibling
    - can only communicate to sibling by passing data from child to parent, and then child to other child
- utlize state in common ancestor to "lift the state up"

### Controlled vs Uncontrolled Components
- Controlled Component
  - when using two-way binding
  - both using and setting the value is "controlled" by the parent component

### Stateless vs Stateful Components
- Stateless/Dumb/Presentational vs Stateful/Smart components
- Couple of components that handle the state
- Other components do not manage the states
- Should have less stateful than stateless which is indicative of having modular components
