# Advanced NextJS

### Notes

- square brackets can be used for dynamic urls even in folders `e.g. ./pages/[meetupId]/index.js`

### \_app.js

- special component that acts as root component that next.js uses to render every page
- `Component` contains the component of the page
- `pageProps` contains the props passed to the component
- we can then wrap the \_app.js with the Layout / Header

```js
import Layout from "../components/layout/Layout";
import "../styles/globals.css";

function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  );
}

export default MyApp;
```

### Navigating programatically

```js
import { useRouter } from 'next/router';

function TestItem(props) {
    // equivalent of using Link
    router.push('/' + props.id);
```

- first parameter of `router.push` is the path to navigate to
- want to prerender a page before it loads? NextJs has a solution for that

### Page Pre-Rendering

- `/some-route` - return pre-rendered-page; good for SEO! Hydrate with React code once loaded
- NextJS provides Two Forms of Pre-rendering
  1. Static Generation
     - page is prerendered when it is built for production
     - export ONLY in pages files
     - `getStaticProps()` is a required function name and can be async
     - nextJS will wait for this function to finish
     - any code written in there will not be executed in the client side; executed before the "second" rendering is done by react; done when app is built
     - should always return an object
     - can be useful for caching certain parts of the system

```js
function HomePage(props) {
  return <MeetupList meetups={props.meetups} />;
}

export async function getStaticProps() {
  // fetch data from an API
  return {
    props: {
      meetups: DUMMY_MEETUPS,
    },
  };
}

export default HomePage;
```

    - Drawbacks
        - data could be outdated; since it is triggered when built, only the data during that time will be fetched
        - To fix, add a `revalidate` key to add revalidation every x seconds if there are requests coming in
        - Depending on the frequency of the data being accessed; the number of seconds can be increased or decreased

```js
export async function getStaticProps() {
  // fetch data from an API
  return {
    props: {
      meetups: DUMMY_MEETUPS,
    },
    revalidate: 10, // number of seconds it waits before the function is regenerated
  };
}
```

    2. Server-side Rendering
        - when you want to pregenerate on-the-fly, not every couple of seconds / not every build
        - we can use `getServerSideProps()`
        - will not run when the server is built but will run everytime there are new requests
        - will only run in the server
        - can contain `context` parameter which can contain request `context.req` and response `context.res`
        - Drawback is that you need to wait for the function to finish for every request
        - Only use if data needs to change every request and if you need to access the incoming request and response

```js
export async function getServerSideProps(context) {
  const req = context.req;
  const res = context.res;

  // fetch data from an API
  return {
    props: {
      meetups: DUMMY_MEETUPS,
    },
  };
}
```

### getStaticProps
- to get the params from the pages `e.g. [meetupId]` inside getStaticProps, we can use the context

```js
export async function getStaticProps(context) {
  // fetch data for a single meetup
  const meetupId = context.params.meetupId;
}
```
- `getStaticPaths()`
- `fallback`
    - if `false`, the paths contains all supported values; unsupported paths will return 404
    - if `true`, will check against server if it does not show any

### Metadata!
- must add metadata
```js
import Head from 'next/head';
```
- added for header metadata

### Deploying
- Vercel
  - deploy github repository made by the next.js team
  - don't need to build `npm run build` for Vercel but needed if you have to build


### Setting fallbacks
- set to `fallback: 'blocking'` will generate the page on demand
- `true` - immediately return an empty page until the page is served
- `blocking` - wont show anything until the page is served