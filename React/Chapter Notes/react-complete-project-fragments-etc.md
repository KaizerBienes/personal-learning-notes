# Complete project notes
- `for` in `<label>` is `htmlFor` in react

## Fragments, Portals, and Refs
- JSX Limitations and Fragments
- Cleaner doms with portals
- Working with refs

### JSX Limitations
- you can't return more than one "root" jsx element
- this is because you can't return more than one element in JS
- Solution: wrap inside a `<div>`
    - however, this results into a `<div>` soup
    - not a good practice since we will end up with tons of unnecessary `div`s
```js
return (
    <div>
        <h1>Header!</h1>
        <p>Body!</p>
    </div>
);
```
- Alternative Solution:  return an array; but "key" is needed
```js
return (
    [
        <h1>Header!</h1>,
        <p>Body!</p>
    ]
);
```

### Fragments
- Best Solution: use a higher-order component (Fragment)
```js
const Wrapper = props => {
    return props.children;
};

export default Wrapper;

// and then used in jsx as
return (
    <Wrapper>
        <h1>Header!</h1>
        <p>Body!</p>
    </Wrapper>
);
```

- or using framents
```js
import React, { Fragment } from 'react';
return (
    <Fragment>
        <h1>Header!</h1>
        <p>Body!</p>
    </Fragment>
);
```

- or using the empty tag for later versions of React
```js
return (
    <>
        <h1>Header!</h1>
        <p>Body!</p>
    </>
);
```

### React Portals
- In adding modals to components, we add a modal to the component and style it so that it will work
- Semantically, and from a "clean html structure" , having a nested modal is not ideal
- Modals are overlayed to the entire page
- While it works, it's not a good practice
- Thus, we can use "portals"!
- Needs 2 things:
    1. place where the portal is moved
    2. notify where it will be used

```html
// in index.html
<div id="backdrop-root"></div>
<div id="overlay-root"></div>
<div id="root"></div>
```

- `createPortal()` from `react-dom` takes two arguments:
    1. what to render
    2. pointer to that container in the real dom where it should be rendered

```js
const Backdrop = (props) => {
  return <div className={classes.backdrop} onClick={props.onConfirm}></div>;
};

const ModalOverlay = (props) => {
  return  (
      <Card className={classes.modal}>
        ...
      </Card>
  );
};

const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <Backdrop onConfirm={props.onConfirm} />,
        document.getElementById("backdrop-root")
      )}
      {ReactDOM.createPortal(
        <ModalOverlay
          title={props.title}
          message={props.message}
          onConfirm={props.onConfirm}
        />,
        document.getElementById("overlay-root")
      )}
    </React.Fragment>
  );
};
```

### Refs
- references allow us to access other dom elements and work with them
- stores the actual dom element to the reference
- uncontrolled components if we used a ref since their internal state is not controlled by react and since we don't feed data into the input
- use react to do any heavy lifting
```js
import React, { useRef } from 'react';

const Example = (props) => {
  const nameInputRef = useRef();

  return (
      <form>
          <input
            id="username"
            type="text"
            ref={nameInputRef}
          />
      </form>
  );
}
```
- we can then access the value via the ref when the input is changed
- alternative to state
```js
    console.log(nameInputRef.current.value);
```