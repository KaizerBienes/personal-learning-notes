# Building React Hooks
- outsource stateful login into re-usable functions
- custom hooks can use other react hooks and react state

### Building Custom hooks
- folder for hooks with kebab-case filename (e.g. usec-countrer.js)
- custom hooks must be prepended with `use` (required by React)
- other hooks can still be used inside custom hooks (i.e. setState, setEffect, etc.)
- if state is declared in custom hook, it can still be used in the component that called the custom hook (own instance per component; not global)

```js
import { useState, useEffect } from "react";

const useCounter = (forwards = true) => {
  const [counter, setCounter] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      if (forwards) {
        setCounter((prevCounter) => prevCounter + 1);
      } else {
        setCounter((prevCounter) => prevCounter - 1);
      }
    }, 1000);

    return () => clearInterval(interval);
  }, [forwards]);

  return counter;
};

export default useCounter;
```

- shortcut objects
```js
  return {
      isLoading: isLoading,
      error: error,
      sendRequest: sendRequest
  };
  // equal to
  return {
      isLoading,
      error,
      sendRequest
  };
```