# Next.js

### Module Content

- What is NextJS? And Why?
- File-based Routing and Page Pre-rendering
- Data Fetching and Adding an API

### What is NextJS

- The React Framework for Production
  - Isn't React a framework already?
  - Focuses on Components, State, Props; for routing, auth, etc. needs additional libaries
  - NextJS Framework builds on top of React
    - Frameworks have a clear guidance than a library
  - Makes building large scale easier
  - Solves common problems and makes building React apps easier!
- A fullstack framework for ReactJS
  - Lots of built-in features for solving common problems and clear guidance on how to use features
  - you can still write react code but NextJs enhances it
  - Production ready framework

### NextJS Key Features and Benefits

- Built-in Server-side Rendering
  - In React, rendering is on the Client-side; not really a problem but can be a problem because data fetching can result to loading screens
  - In React, SEO optimization will only see the initially empty html page which can be a problem
  - If page is prerendered on the Server-side, then the finished page will be shown to the SEO crawlers and users
  - Automatic page pre-rendering: Great for SEO and initial load
  - Blending client-side and server-side: Fetch data on the server and render finished pages

- Simplified File-based Routing
  - In React, routing is in code which is extra code you have to write
    - Replicates number of pages in `/pages` in Routes
  - In NextJS, routing is in a separate page
    - Defined routes and pages with files and folders instead of code
    - Less code, less work, highly understandable

- Fullstack Capabilities
  - standalone code to include to backend
  - server-side code to your next / react apps using Node.js
  - storing data, getting data, authentication, etc. can be added to your React projects

- to create a nextjs app: `npx create-next-app; cd <project_name>; npm install`

### NextJS folders

- has a built-in rendering; NextJS allows us to determine when to pre-render
- To run the dev folder: `npm run dev`

### Routing

- we just create files inside the `pages/` folder
- index.js is loaded as first page
- folders act as path e.g. `pages/news/index.js` loads `domain.com/news`
- subfolder files act as paths as well
  - e.g. `pages/news/something.js` loads to `domain.com/news/something.js`
- for dynamic content / data
  - using square brackets as the file name e.g. `pages/news/[newsId].js`
  - to access the square brackets value, we should use `useRouter'

```js
// in pages/news/[newsId].js
import { useRouter } from "next/router";

function TestPage() {
  const router = useRouter();
  const newsId = router.query.newsId;

  return <h1>News ID: {newsId}</h1>;
}

export default TestPage;
```

### Linking Pages

- NextJS navigates to a new page when linked to a different page but it prevents it from being a single page application
- This means that Redux, Context, etc. will be lost
- To make sure that we don't lose the application from being an SPA, we should use `Link`

```js
import Link from "next/link";

import { Fragment } from "react";
// our-domain.com/news

function NewsPage() {
  return (
    <Fragment>
      <h1>The News Page</h1>
      <ul>
        <li>
          <Link href="/news/nextjs-is-a-great-framework">
            NextJS Is A Great Framework
          </Link>
        </li>
        <li>Something Else</li>
      </ul>
    </Fragment>
  );
}

export default NewsPage;
```

- Link watches the anchor tag and loads the `to-be-loaded` component on the backend
