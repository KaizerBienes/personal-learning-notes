# Multi Page Application

- client-side routing
- using React-Router

### Building SPAs

- when building complex user interfaces, we're building SPAs
- only one initial HTML request and response
- page (URL) changes are then handled by client-side (React) code
- changes the visible content without fetching a new HTML file

### React Router - Declarative Routing for React.js

- `npm install react-router-dom`
- e.g.

our-domain.com => loads Component A
our-domain.com/products => Component B

- Wrap the <Route> within M<BrowserRouter>

```js
// loads the routing component
import { Route } from "react-router-dom";

import { BrowserRouter, Route } from "react-router-dom";
import Products from "./pages/Products";
import Welcome from "./pages/Welcome";

function App() {
  return (
    <BrowserRouter>
      <Route path="/welcome">
        <Welcome />
      </Route>
      <Route path="/products">
        <Products />
      </Route>
    </BrowserRouter>
  );
}

export default App;
```

# Link

- react-router-dom component to link components without reloading the page / adding further requests

```js
import { Link } from "react-router-dom";

const MainHeader = () => {
  return (
    <header>
      <nav>
        <ul>
          <li>
            <Link to="/welcome">Welcome</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
          </li>
        </ul>
      </nav>
    </header>
  );
};

export default MainHeader;
```

### NavLinks

- to include styling on active class via `activeClassName`

```js
return (
  <>
    <li>
      <NavLink activeClassName={classes.active} to="/welcome">
        Welcome
      </NavLink>
    </li>
    <li>
      <NavLink activeClassName={classes.active} to="/products">
        Products
      </NavLink>
    </li>
  </>
);
```

### Dynamic Paths

- to have different dynamic paths
- can have multiple segments
- should use the same key

```js
// in root component
return (
  <Route path="/product-detail/:productId">
    <ProductDetail />
  </Route>
);

// in product detail
import { useParams } from "react-router-dom";

const ProductDetail = () => {
  const params = useParams();

  return <p>{params.productId}</p>;
};
```

- react routers by default, all routes that matches the path is loaded
- matching in react-routers is if the "starting" path matches
  - e.g. `products/` and `products/:productId` will show both components
  - to fix this, we use the `Switch` component

### Switch

- matches top to bottom and will stop when it matches the first one matched on the first part
- to fix this, we need to act `exact` prop

```js
return (
  <Switch>
    <Route path="/products" exact>
      <Products />
    </Route>
    <Route path="/products/:productId">
      <ProductDetail />
    </Route>
  </Switch>
);
```

### Nested routing

- we can add routes anywhere but routes will only be loated when the component that contains the routing will enable the route

### Redirecting

- to redirect to a specific route, we can use `Redirect` component

```js
return (
  <Route path="/" exact>
    <Redirect to="/welcome" />
  </Route>
);
```

### Not Found

- To match not found pages
- must come last from the routes

```js
return (
  <Route path="*">
    <NotFound />
  </Route>
);
```

### Programmatic Navigation

- when for example, contact form is submitted
- we can use a Link but forms require a button to submit
- `useHistory` to change the history of the pages visited
- history.replace changes , history.push will push a new page

```js
import QuoteForm from "../components/quotes/QuoteForm";
import { useHistory } from "react-router-dom";

const NewQuote = () => {
  const history = useHistory();
  const addQuoteHandler = (quoteData) => {
    history.push("/quotes");
  };
  return <QuoteForm onAddQuote={addQuoteHandler} />;
};
export default NewQuote;
```

### Navigating away prompt

- `Prompt` shows a alert for prompting the user when they try to navigate away from the page
- first parameter accepts the state to look and watch
  - we can set a focus on the form which will modify the `isEntering` state
  - should set `isEntering` to false when submitting to prevent the prompt from showing up
- message contains a callback whose parameter is the location on where it wants to navigate to

```js
<Prompt
  when={isEntering}
  message={(location) => "Are you sure you want to leave?"}
/>
```

### Query Parameters

- `useHistory` to modify the url
- `useLocation` to manage the query parameters; info about the currently loaded url

```js
const QuoteList = (props) => {
  const history = useHistory();
  const location = useLocation();

  // location.search contains the query parameters returns a dictionary
  const queryParams = new URLSearchParams(location.search);
  const isSortingAscending = queryParams.get('sort') == 'asc';

  // custom function sortQuotes
  const sortedQuotes = sortQuotes(props.quotes, isSortingAscending);
```

### useRouteMatch

- to prevent us from repeating the paths, we can use useRouteMatch
- it contains the path and the url

```js
const QuoteDetail = () => {
  const match = useRouteMatch();

  return (
    <Link className="btn--flat" to={`${match.url}/comments/`}>
      Load Comments
    </Link>
  );
```

