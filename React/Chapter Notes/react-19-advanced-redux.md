# Advanced Redux
- Handling Async
- where to put code
- redux tools

### Side effects, async tasks and Redux
- reducers must be pure, side-effect free, synchronous functions
    - input (old state + action) -> output (new state)
- never run async code inside the reducers
- where should side-effects and async tasks be executed?
    - Two possible ways:
        - inside the components (via useEffect()); only dispatch when dispatched runs
        - inside the "action creators"; redux has a solution to run async tasks without changing the reducer function

- never mutate a state outside of the reducer; outside the reducer, it must be read-only as overwriting the state will modify the state without notifying the store

### Fat Reducers vs Fat Components vs Fact Actions
- where should our logic (code) go?
    - synchronous code, side-effect free code (i.e. data transformations)
        - prefer reducers
        - avoid action creators or components
            - avoid transformation in components
    - asynchronous code
        - prefer action creators or components
        - avoid reducers

- if we want to do async after transformation via reducers, we can do the ff:
    1. use `useEffect` which listens to changes in a specific state via `useSelector`

### Thunk
- function that delays an action until later
- an action creator function that does not return the action itself but another function which eventually returns the action
- in redux, we create action creators by dispatching a dispatch
    - thus we move all logic to the reducer and then pass the dispatch to that action creator
- not a bad idea to keep our components lean
    - we can then move logic to our action creators
```js
function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    if (isInitial) {
      isInitial = false;
      return;
    }

    dispatch(sendCartData(cart));
  }, [cart, dispatch]);
```

### in action creator
```js
const fetchCartData = () => {
  return async (dispatch) => {
    const fetchData = async () => {
        ...
    };
    try {
        const cartData = await fetchData();
        dispatch(cartActions.replaceCart(cartData));
    } catch (error) {
        ...
    }
```

### Exploring the Redux DevTools
- makes debugging easier!
- Redux DevTools - https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en