# Redux

- redux and why
- redux basics and using redux with react
- redux toolkit

### Redux?

- can be used with any javascript project
- state management system for cross-component or app-wide state
- types of state
  - Local state
    - state belonging to a single component
    - e.g. listening to user input in a input field; toggling a "show more" detail field
    - should be managed component-internal with `useState()` / `useReducer()`
  - Cross-component state
    - state that affect multiple components
    - e.g. open / closed state of a modal overlay
    - requires "prop chains" / "prop drilling"; passing props across multiple components
  - App-wide state
    - state that affects the entire app (most/all components)
    - e.g. user authentication status
    - requires "prop chains" / "prop drilling"; passing props across multiple components
    - can be cumbersome, and so, we can perform `useContext`
    - or React Context or Redux

### Why use React Context vs Redux?

- You can use both Redux and React Context; redux for app wide, context for specific parts of the system
- Redux
  - state management system for cross-component or app-wide state
    - don't we have "React Context" already?
- React Context
  - potential disadvantages
    - Complex Setup / Management
      - can become quite complex when you chain app-wide states
      - can lead to deeply nested JSX code of huge "Context Provider" components
    - Performance
      - contexts are used for low frequency unlikely updates (like locale / theme). Not great for high frequency changes
      - not flux-like

### How Redux Works and Core Redux Concepts

    - exactly one Central Data (State) Store
        - store themeing, auth, settings, etc.
        - data in store that can be used in components
        - components subscribe to the store to get the data their need when the state changes
        - components do not directly manipulate the Central Data (State) Store
    - Reducer Function
        - mutating / changing store data
        - not useReducer(); reducer functions are a general concept
        - takes some input and process/transform that input and generate a new output
        - should be a pure function -> same input leads to same output
            - inputs: old state + dispatched action
            - output: new state object
            - should not do http calls, add to localStorage, etc.; only transformations
    - Actions
        - components dispatch / trigger an action
        - javascript object that forwards the action to the Reducer Function
    - Central Data State Store -subscription-> components -dispatch-> Action -Forwarded to-> Reducer Function -Mutates / changes store data-> Central Data State Store

### Starting with Redux

```js
const redux = require("redux");

// reducer function
// executed once when the store is created; thus we should add a default value for state
const counterReducer = (state = { counter: 0 }, action) => {
  return {
    counter: state.counter + 1,
  };
};

// the central data state store
const store = redux.createStore(counterReducer);

console.log(store.getState()); // {counter: 1}

const counterSubscriber = () => {
  store.getState(); // contains the latest state
};

store.subscribe(counterSubscriber); // for subscribing; needs the function

store.dispatch({ type: "increment" }); // to dispatch to the reducer
```

### React redux

- install via `npm install redux react-redux`
- wrap a part / the app using react-redux

```js
// create your redux store
import { createStore } from 'redux';

const counterReducer = (state = { counter: 0 }, action) => {
    ...

    return state;
};

const store = createStore(counterReducer);

export default store;
```

```js
// usage in app
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";

import "./index.css";
import App from "./App";
import store from "./store/index";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

### useSelector / connect

- `useSelector`
  - uses a part of our Redux store
  - connects / subscribes the component to the store
- `connect` wrapper to connect the component to the store

```js
const Counter = () => {
    // retrieves only the counter
    // automatically sets up a subscription when the data changes
  const counter = useSelector(state => state.counter);
  ...
};
```

### Dispatching actions

- `useDispatch()` returns a dispatch action which we can then use for triggering the reduce function

```js
const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector(state => state.counter);

  const incrementHandler = () => {
    dispatch({ type: 'increment' });
  };

  return (
    <React.Fragment>
        <div>
            <button onClick={incrementHandler}>Increment</button>
            <button onClick={decrementHandler}>Decrement</button>
        </div>
        <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </React.Fragment>

  );
```

### Component-based classes

- must use connect which is a higher-order function
- mapping states and dispatch to props

```js
class Counter extends Component {
  incrementHandler() {
    this.props.increment();
  }

  decrementHandler() {
    this.props.decrement();
  }

  render() {
    return (
      <>
        <div className={classes.value}>{this.props.counter}</div>
        <div>
          <button onClick={this.incrementHandler.bind(this)}>Increment</button>
          <button onClick={this.decrementHandler.bind(this)}>Decrement</button>
        </div>
      </>
    );
  }

  // alternative to useSelector
  const mapStateToProps = state => {
    return {
      counter: state.counter
    };
  };

  // equivalent to useDispatch
  // must be used to connect to redux
  const mapDispatchToProps = dispatch => {
    return {
      increment: () => dispatch({type: 'increment'}),
      decrement: () => dispatch({type: 'decrement'})
    }
  };
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

### Redux Dispatching
- must always return all of the states when writing to the reducer
- you should NEVER mutate the state; instead, always override by returning a brand new state object
```js
const initialState = { counter: 0, showCounter: true };

const counterReducer = (state = initialState, action) => {
    // state.counter++;
    // return state; // NEVER do this!

    if (action.type === 'increment') {
        // always return all of the states!
        return {
            counter: state.counter + 1,
            showCounter: state.showCounter
        }
    }
};
```

### Redux Toolkit

```sh
npm install @reduxjs/toolkit
```

- can use `createSlice` to create different slices of similar states possibly in different files
- when using redux toolkit, we CAN manipulate the state because MAGIC

```js
import { createSlice, configureStore } from "@reduxjs/toolkit";

createSlice({
  name: "counter", // identifier entirely up to you
  initialState,
  reducers: {
    // automatically receive the current state and action
    increment(state) {
        state.counter++;
    },
    decrement(state) {
        state.counter--;
    },
    increase(state, action) {
        state.counter += action.amount;
    },
    toggleCounter(state) {
        state.showCounter = !state.showCounter;
    },
  },
});

// action creators automatically created behind the scenes
export const counterActions = counterSlice.actions;

// if only one reducer
const store = configureStore({
  // expected property
  reducer: counterSlice.reducer
});

// for multiple reducers
const store = configureStore({
    // expected property
    reducer: {
        // key of our choice; value is the slice; to merge all reducers
        counter: counterSlice
    }
};
// action creators automatically created behind the scenes
// counterSlice.actions.toggleCounter;

export default store;
```

### To Use
```js
import { counterActions } from "../store/index";
const Counter = () => {
  const dispatch = useDispatch();
  const counter = useSelector((state) => state.counter);
  const show = useSelector((state) => state.showCounter);

  const incrementHandler = () => {
    dispatch(counterActions.increment());
  };

  return (...);

};
```

- `configureStore` creates a store but we can merge multiple reducers / slices

### Handling multiple slices
##### in the store
```js
const counterSlice = createSlice({ ... });
const authSlice = createSlice({ ... });

const store = configureStore({
  // expected property
  reducer: {
    counter: counterSlice.reducer,
    authentication: counterSlice.reducer,
  },
});

// action creators automatically created behind the scenes
export const counterActions = counterSlice.actions;
export const authActions = authSlice.actions;
```

##### to use
```js
const Counter = () => {
  const dispatch = useDispatch();
  // use the key on the reducer against the state (state.<store_key>.<state_name>)
  const counter = useSelector((state) => state.counter.counter);
  const show = useSelector((state) => state.counter.showCounter);

  const incrementHandler = () => {
    dispatch(counterActions.increment());
  };
};
```

### Separation of Concern

```js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from './counter';
import authReducer from './auth';

const store = configureStore({
  // expected property
  reducer: {
    counter: counterReducer,
    auth: authReducer
  },
});

export default store;
```

```js
import { createSlice } from "@reduxjs/toolkit";

const initialCounterState = { counter: 0, showCounter: true };

const counterSlice = createSlice({...});

export const counterActions = counterSlice.actions;

export default counterSlice.reducer;
```

```js
import { createSlice } from "@reduxjs/toolkit";

const initialAuthState = { isAuthenticated: false };
const authSlice = createSlice({
  name: "authentication",
  initialState: initialAuthState,
  reducers: { ... }
});

export const authActions = authSlice.actions;

export default authSlice.reducer;
```