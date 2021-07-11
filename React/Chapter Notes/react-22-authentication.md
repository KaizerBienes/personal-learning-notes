# Authentication

### Content
- How Authentication WOrks in React Apps
- Implementing User Authentication
- Authentication Persistence and Auto-Logout

### What, How, and Why?
- Needed if content should be protected (not accessible by everyone)
- Two-step Process:
    1. Get access / permission
    2. Send request to protected resource

### Getting Permission
- Client (Browser) -> Server
1. Client sends Request (with user credentials)  to Server
2. Response from Server to Client

### How Does Authentication Work?
1. Server-side Sessions
  - Once a server grants excess, server stores unique identifier on server, send some identifier to client
  - Response to Client is an identifier along with requests to protected resources
  - Disadvantage is it tightly couples the backend and frontend; server should be "stateless" to decouple
  - Server should not store the connection details
2. Authentication Tokens
  - Creates (but not store) "permission" token on server, send token to client
  - "Key" involved that is only known by the server
  - Client sends token along with requests to protected users

### Firebase API
- https://firebase.google.com/docs/reference/rest/auth

### Navigation Guards
- dynamically changing our routes based on whether we are logged in or not
- we can check if the user is logged in from the Context / Redux via if-else / inline 

```js
{authCtx.isLoggedIn && <Profile />}
{!authCtx.isLoggedIn && <Redirect to "/auth" />}
```

### Local storage vs Cookies
- https://academind.com/tutorials/localstorage-vs-cookies-xss/
- localStorage is a synchronous API!
```js
  const initialToken = localStorage.getItem('token'); // to retrieve the token
localStorage.setItem('token', loginToken); // to login
localStorage.removeItem('token'); // to logout
```