# Effects, Reducers, and Context
- Working with (Side) Effects
- Manage more complex state with Reducers
- App-wide or Component Wide State with Context


### Effect (or a "Side Effect")
- The whole react app has the main job: render UI and React to User Input
    - Evaluate Render JSX
    - Manage State and Props
    - React to User Events and Input
    - Re-evaluate Component upon State and Prop Changes
- Baked into React via tools hooks, props, state, etc. 
- Side effects: Anything Else
    - Storage data in browser storage
    - Send http requests to backend servers
    - Set and manage timers


### useEffect() Hook
- used to handle side effects; not just http but in response with any changes
```js
useEffect(() => {...}, [ dependencies ]);
```
- first parameter is executed after every component evaulation if the specified dependencies changed
- secopnd parameter - function only runs if the dependencies changed; contains the dependencies
- for example, storing token / if the user is logged
```js
  const loginHandler = (email, password) => {
    // We should of course check email and password
    // But it's just a dummy/ demo anyways
    localStorage.setItem('isLoggedIn', '1');
    setIsLoggedIn(true);
  };
```
- Developer Tools > Application > Local Storage
- code is executed after all the components are loaded and evaluated; only if the dependency change or if ran for the first time
- if no second parameter, it will just run once (since there are no dependencies to monitor)

### Sample with dependencies
  - will retrigger when either `enteredEmail` or `enteredPassword` changes
```js
  useEffect(() => {
    setFormIsValid(
      enteredEmail.includes('@') && enteredPassword.trim().length > 6
    );
  }, [enteredEmail, enteredPassword]);
```
- return in useEffect runs before every new side effect execution!
  - clears the timeout whenever the next side effect runs which causes a "debounce"

```js
  useEffect(() => {
    const identifier = setTimeout(() => {
      console.log('Checking...');
      setFormIsValid(
        enteredEmail.includes('@') && enteredPassword.trim().length > 6
      );
    }, 500);

    return () => {
      clearTimeout(identifier);
    };
  }, [enteredEmail, enteredPassword]);
```
- can have multiple `useEffect()`
  - to prevent it from running anytime, we can:
    - have no second argument
    - add dependency
    - add cleanup (return)