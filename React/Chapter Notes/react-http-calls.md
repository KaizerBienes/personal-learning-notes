# React HTTP Calls

## Connecting a Backend and Database
- sending http requests
- how react apps interact with databases?
- send http requests and using responses
- handling errors and loading states

## Browser-side Apps Don't Directly Talk to Databases
- React App shouldn't talk directly to a database (sql, nosql, etc.)
  - database credentials would be exposed in the browser, performance issues, etc.
- Thus, there should be a backend app where the react app can connect to
  - React App -> Backend App -> Database

- swapi.dev - star wars api for testing

## Sending a GET Request
- axios can be used to retrieve apis
- but there is a builtin inside javascript, `Fetch API`
- default is GET
- handle responses using Promises
    - error handling is via `.catch(() => {})`
```js
  function fetchMoviesHandler () {
    fetch('https://swapi.dev/api/films/')
      .then(response => {
        return response.json();
      }).then(data => {
        // transform data
        const transformedMovies = data.results.map(movieData => {
          return {
            id: movieData.episode_id,
            title: movieData.title,
            openingText: movieData.opening_crawl,
            releaseDate: movieData.release_date
          };
        });

        setMovies(transformedMovies); // set via state
      });
  }
```

## Async Await
- error handling is via try-catch block
```js
  async function fetchMoviesHandler () {
    const response = await fetch('https://swapi.dev/api/films/');
    const data = await response.json();
    const transformedMovies = data.results.map(movieData => {
      return {
        id: movieData.episode_id,
        title: movieData.title,
        openingText: movieData.opening_crawl,
        releaseDate: movieData.release_date
      };
    });

    setMovies(transformedMovies);
  }
```

## useEffect() for fetching requests data
- functional dependencies inside useEffect, use useCallback
- props should be added as dependencies in the callback, but not states since React guarantees that those do not change
```js
  const fetchMoviesHandler = useCallback(async () => {
      ...
  }, []);

  useEffect(() => {
    fetchMoviesHandler();
  }, []);
```


## Sending a POST request
- using fetchAPI and adding method
```js
async function addMovieHandler(movie) {
    await fetch('https://react-test-c3c8a-default-rtdb.asia-southeast1.firebasedatabase.app/movies.json', {
        method: 'POST',
        body: JSON.stringify(movie),
        headers: {
        'Content-Type': 'application/json'
        }
    });

    const data = await response.json();
    console.log(data);
}
```