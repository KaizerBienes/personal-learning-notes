# Use Context
- forwarding data to different components that do not use the props is unnecessary and can cause readability issues
- Component-wide, "behind the scenes" state storage
- when forwarding data between multiple components, it could be better to use contexts instead

- naming of contexts could be of "kebab" case as ProperCase could imply Components

### Context
- first parameter is the default state
    - usually an object but could be anything

```js
import React from 'react';

const AuthContext = React.createContext({
    isLoggedIn: false
});

export default AuthContext;
```

- usage in JSX
```js
const Example = (props) => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
      <AuthContext.Provider value={{isLoggedIn: isLoggedIn}}>
      </AuthContext.Provider>
  );
}
```

- in Consumer
```js
    return (
        <AuthContext.Consumer>
        {(ctx) => {
            return  (
            <nav className={classes.nav}>
                <ul>
                {ctx.isLoggedIn}
            );
        }}
        </AuthContext.Consumer>
    );
```

- better context usage

```js
import React, { useContext } from 'react';

const Navigation = (props) => {
  const ctx = useContext(AuthContext);
  return (
        {ctx.isLoggedIn}
  );
```

- in most cases, `props` is used
- only if forwarding to multiple components / specific behavior, use `context`
- for us to further isolate the auth context, we can add a custom component within the context that handles it

```js
export const AuthContextProvider = (props) => {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  const loginHandler = (email, password) => {
    localStorage.setItem("isLoggedIn", "1");
    setIsLoggedIn(true);
  };

  const logoutHandler = () => {
    localStorage.removeItem("isLoggedIn");
    setIsLoggedIn(false);
  };

  return (
    <AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
        onLogout: logoutHandler,
        onLogin: loginHandler,
      }}
    >
      {props.children}
    </AuthContext.Provider>
  );
};
export default AuthContext;

```

- and then used via
```js
import {AuthContextProvider} from './store/auth-context';

const Example = (props) => {
    return (
        <AuthContextProvider><App /></AuthContextProvider>
    );
};
```

### Context Limitations
- great for app-wide but it is not a replacement for component configuration
- not great and not optimized for high frequency changes!
    - we'll explore a better tool for it, called Redux
- React context should replace all components props
    - context is useful for avoiding prop chains


## Rules of Hooks
1. Must only call react hooks in react functions
    - react component functions
    - custom hooks
    - you cannot call `useState` inside `useReducer` for example
2. Only call React Hooks at the Top Level
    - don't call in nested
    - don't call in block components
    - e.g. cannot call `useState` inside `useEffect` (inside the component function)
3. In useEffect(), always add everything you refer to as dependencies


### Forward Refs
- `useImperativeHandle` use imperatively by directly manipulating the component
- should be used `rarely`
- internal functionalities and "outside" world
- second reference for components is `ref` (i.e. (props, ref))
    - binding a ref will establish a connection
    - ref is a reserved word
- expose functionality from child to a parent component
- can be a bad practice since it is no longer top-down

```js
    return (
        <Input
            ref={emailInputRef}
        />
    );
```

```js
const Input = React.forwardRef((props, ref) => {
  const inputRef = useRef();

  const activate = () => {
      inputRef.current.focus();
  }

  useImperativeHandle(ref, () => {
      return {
          focus: activate
      };
  });

    return ();
});
```