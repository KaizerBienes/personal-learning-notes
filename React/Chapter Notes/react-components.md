# Components
  - What and Why?
  - Working with Class-based Components
  - Error Boundaries

### Alternative Way of Building Components

```js
// current way
function Product(props) {
    return <h2>A Product!</h2>;
}

// class-based components
// required in the past
class Product extends Component {
    render() {
        return <h2>A Product!</h2>;
    }
}
```

- React < 16.8, you had to use Class-based Components to manage "State"
- React 16.8 introduced "React Hooks" for Functional Components
- Class based components can't use React Hooks!

### Using props
- to use props, we should extend `Component` to get the `this.props` property
```js
import { Component } from 'react';

class User extends Component {
  render() {
    return <li className={classes.user}>{this.props.name}</li>;
  }
}
```
- can be mixed and matched with functional based components

### States
- states should be an object must be named `state`
- states must be grouped in a single object
```js
// class based
class Users extends Component {
  constructor() {
    this.state = {
        showUsers: true
    };
  }
}

// functional
const Users = () => {
  const [showUsers, setShowUsers] = useState(true);
};
```
- setting state via classes
- setting state does not override the whole hash but only changes what is in the `setState`

```js
toggleUsersHandler() {
    this.setState({showUsers: false});
}

// to use previous
toggleUsersHandler() {
    this.setState((curState) => {
        return { showUsers: !curState.showUsers }
    });
}

// binding it in JSX refers to the `this` in the function
render() {
    return (
        <button onClick={this.toggleUsersHandler.bind(this)}>
    );
}
```

### Component Lifecycle
- `componentDidMount()`
  - called once component is mounted (was evaluated and rendered)
  - similar to `useEffect()` in functional components
- `componentDidUpdate(prevProps, prevState)`
  - called once component is updated (was evaluated and rendered)
  - similar to `useEffect(..., [someDependencies])` in functional components
  - first and second parameter are the previous values so that we can compare if those props / states changed
- `componentWillUnmount()`
  - called before component is unmounted (removed from DOM)
  - similar to  the return (cleanup) `useEffect(() => return () => {})` in functional components


### Context
- similar to functional components
- can only have one context at a time
- if two context needed, there are other workarounds for it
```js
class UserFinder extends Component {
    static contextType = UsersContext;

    return (
        <UsersContext.Consumer>
        </UsersContext.Consumer>
    );
}
```

### When to use?
- prefer functional components
- use class-based if:
    - you prefer it
    - working on existing projects
    - working with error-boundaries

### Error Boundaries
- for catching potential errors
- similar to try/catch
- `componentDidCatch()` makes class-based component an error boundary by convention
    - triggered when a child throws an error
```js
import { Component } from 'react';

class ErrorBoundary extends Component {
    constructor() {
        super();
        this.state = {hasError: false};
    }

    componentDidCatch() {
        this.setState({hasError: true});
    }

    render() {
        if (this.state.hasError) {
            return <p>Something went wrong!</p>
        }

        return this.props.children;
    }
}

// in usage
class ErrorBoundary extends Component {
  render() {
    return (
        <ErrorBoundary>
            <p>Hello world!</p>
        </ErrorBoundary>
    );
  }
}
```
- in dev, it will show a trace which can be closed
- not yet possible with functional components