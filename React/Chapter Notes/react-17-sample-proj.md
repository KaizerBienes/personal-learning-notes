# Sample Project

- use effect must not directly use async await as the return (cleanup) must be synchronous
    - we can however create a function inside that is asynchronous
```js
// not
useEffect(async () => {
    const response = await fetch('https://react-test-c3c8a-default-rtdb.asia-southeast1.firebasedatabase.app/meals.json');
    const responseJson = await response.json();
}, []);

// but like this
  useEffect(() => {
    const fetchMeals = async() => {
      const response = await fetch('https://react-test-c3c8a-default-rtdb.asia-southeast1.firebasedatabase.app/meals.json');
      const responseJson = await response.json();
    }
  }, []);
```