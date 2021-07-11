# Reducer

- more complex states
- useState often becomes hard or error-prone to use
- if updating a state which depends on another state, reducer might be a good choice

### useReducer()

```js
const [state, dispatchFunction] = useReducer(
  reducerFunction,
  initialState,
  initialFunction
);
```

- `state` - snapshot used in the component
- `dispatchFunction` - function used to dispatch a new action (trigger an update of the state)
- `reducerFunction` - triggered automatically when an action is dispatched (via dispatchFunction()) - receives the latest state snapshot and should return the new, updated state `(prevState, action) => newState`
- `initialState` - initial state
- `initialFunction` - function to set the initial state programatically

```js

const emailReducer = (state, action) => {
  if (action.type === 'USER_INPUT') {
    return {value: action.val, isValid: action.val.includes('@') };
  }

  if (action.type === 'INPUT_BLUR') {
    return {value: state.value, isValid: state.value.includes('@') };
  }

  return {value: '', isValid: false };
};

const Login = (props) => {
  const [emailState, dispatchEmail] = useReducer(emailReducer, {value: '', isValid: null});

  dispatchEmail({type: 'USER_INPUT'});
```

### Object destructuring
```js
const { isValid: emailIsValid } = emailState;
// emailIsValid variable gets the emailState.isValid
// emailIsValid serves as an alias since isValid is also used
```
### useState() vs useReducer()
- you'll know when useReducer() when useState() becomes cumbersome / getting bugs / unintended behaviors
- `useState()`
    - main management "tool"
    - start with it
    - great for independent pieces of state / data
    - state updates is easy and limited to a few kinds of updates
- `useReducer()`
    - great if you need "more power"
    - more complex update logic
    - move logic out of component body
    - if dealing with related pieces of state / data
    - more complex state updates
