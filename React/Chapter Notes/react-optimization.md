### React Optimization
- How does react work behind the scenes
- Understanding the virtual dom and dom updates
- Understanding state and state updates


### How Does React Work?
    - React - Javascript library for building user interfaces
    - ReactDOM - Interface to the web
    - Components
        - Props - data from parent component
        - State - internal data
        - Context - Component-wide data
        - React DOM What the user sees
    - Virtual DOM
        - Determines how that component tree currently looks like and what it should look like
        - ReactDOM receives the differences and then manipulates the real DOM (if needed)

### Re-Evaluating Components !== Re-Rendering the DOM
    - Components
        - When props, state, or context changes
        - React executes component functions
    - Real DOM
        - Changes the real dom are only made for differences between evaluations

### Virtual DOM Diffing

```js
// previous evaluation result
<div>
    <h1></h1>
</div>

// current evaluation result
<div>
    <h1></h1>
    <p></p>
</div>

// should be inserted in DOM; the rest should be unchanged
// whole component is rerendered
```

```js
return (
      <DemoOutput show={false} />
);
```
- DemoOutput will still be re-evaluated (and all its chilren)
    - All child components are re-evaluated since each html element is evaluated as functions in jsx

### React.memo()
- only for functional components

```js
export default React.memo(DemoOutput);
```
- Allows us to optimize functional components
- Compares the new props value from the current props
- This prevents us from reexecuting components if those change
- Why not use it in all components?
  - performance hit
  - because react needs to store two values: previous props value and comparing it
- `React.memo()` only compares values via `===` usually for primitives
- Functions is recreated for every execution cycle; hence, `React.memo()` will consider changes for functions

### Hook to prevent functions from being reevaluated for React.memo()
- we use the hook, `useCallback()`
- allows us to save a function
- second argument must contain the dependencies / anything that changes inside the function
```js
import React, { useCallback } from 'react';

function Example() {
  const toggleParagraphHandler = useCallback(() => {
    if (allowToggle) {
      setShowParagraph(prevShowParagraph => !prevShowParagraph);
    }
  }, [allowToggle]);

  return ();
}
```
- `useCallback()` tells react function exactly as it is
  - thus, we need to pass the second argument
  - when dependency changes, react saves a new copy of the function

### Components and State
- react handles both component and state management
- `useState()` creates a new state behind the scenes and is managed by react

### State scheduling and batching
- our code
 - `<MyProduct />` with state 'DVD'
 - `setNewProduct('Book')` DVD to Book
    - changing does not happen immediately
    - state updates is scheduled (scheduled update change)
    - react can postpone that update potentially "higher" priority task
    - react guarantees that state changes in-order but not immediately executed
 - multiple updates can be scheduled at the same time
 - thus, it is recommended to use the previous state value
```js
// can work but can get buggy for specific cases
setShowParagraph(!showParagraph);

//safer
setShowParagraph(prevShowParagraph => !prevShowParagraph);
```

### React.memo()
- arrays are objects, objects are reference values which is why they are always rerendered by React
- `useMemo()` - allows to memoize any statement
- memoizing probably will be used less often than `useCallback()`
- useful for performance issues

```js
import React, { useMemo } from 'react';


const DemoList = (props) => {
    const { items } = props;

    const sortedList = useMemo(() => {
        return props.items.sort(a, b) => a - b);
    }, [items]);
};
```