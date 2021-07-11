# Deploying React Apps
- From Development to Production

### Content
- Deployment Steps and Pitfalls
- Server-side Routing vs Client-side Routing

### Deployment Steps
- Test Code
- Optimize Code
    - Look into lazy loading
- Build App for Production
- Upload Production Code to Server
- Configure Server

### Lazy Loading - Optimization Technique
- Load code only when it is needed
- Split code into multiple chunks which are only downloaded when needed
- We can use `React.lazy()`
- However, we need to add a fallback so that when it takes a while to load, there is a fallback display

```js
import React, { Suspense } from "react";

const NewQuote = React.lazy(() => import('./pages/NewQuote'));

function App() {
  return (
    <Layout>
      <Suspense
        fallback={
          <div className="centered">
            <LoadingSpinner />
          </div>
        }
      >
        <Switch>
          <Route path="/new-quote">
            <NewQuote />
          </Route>
        </Switch>
      </Suspense>
    </Layout>
  );
}
```

### Build React Script to Production
- run the build via `npm run build`
- will create a folder called `build` which will contain the js files
- should not modify the build files
- you can modify the `robots.txt` for crawlers

### Upload Production Code to Server
- A React SPA is a "Static Website"
- Only HTML, CSS and Javascript
- A Static Site Host is needed to deploy it
- Firebase for example
    - public directory: build
    - Configure as a single-page app? Y
    - Set up builds with GitHub? N
    - Overwrite index.html? N
- To delete: `firebase hosting:disable`
- And to delete the project in the deployment tab


### Client-side vs Server-side Routing
- Server (Hosts the Production-ready React Code)
- Client (User)
- when Client sends a request to the Server, Server then sends the Response (HTML, CSS, JS ReactCode `/some-route`) to the Client (User)
- Client then requests for the route
    - when using a SPA, we should always return the same path
    - React should then look at the URL
    - Server should ignore the path (`/some-route`)
    - Find in your hosting provider how it can ignore the path
