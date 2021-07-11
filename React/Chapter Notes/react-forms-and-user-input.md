# Forms
- forms and inputs can assume different states
- one or more inputs are invalid
  - output input-specific errors
  - ensure form can't be submitted / saved
- all inputs are valid

### When to validate?
- when form is submitted
  - allow the user to enter a valid value before warning him / her
  - avoid unnecessary warning but maybe present feedback is already too late
- when input is losing focus
  - allow user to enter a valid value before warning him / her
  - very useful for untouched forms
  - if input is invalid, you don't tell the user if it is corrected
- on every keystroke
  - direct input
  - warns users before user had a chance of entering valid values
  - potential to provide more direct feedback

### When to use method
- if you only want the entered value once, we can use `useRef`
- use state if you want to check per entered character `useState`
  - you can also change the entered value back to its previous value upon submission
  - in ref, we can do it but it is not recommended to directly manipulate the dom

```js
  const formSubmissionHandler = (event) => {
    event.preventDefault();

    console.log(enteredName);
    const enteredValue = nameInputRef.current.value; // using ref
    console.log("REF: " + enteredValue)
    // nameInputRef.current.value = ''; // NOT RECOMMENDED; DON'T MANIPULATE THE DOM DIRECTLY
    setEnteredName(''); // using state
  };

    return (
        <form onSubmit={formSubmissionHandler}>
            <input
            ref={nameInputRef}
            type="text"
            id="name"
            onChange={nameInputChangeHandler}
            value={enteredName}
            />
        </form>
    );
```

### Tools for form generation
- Formik
  - library for rendering forms