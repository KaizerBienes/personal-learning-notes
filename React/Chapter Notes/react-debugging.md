# React Debugging

## Finding and Fixing Errors

### Understanding React Error Messages
- in `npm start`, errors may show especially for logical / syntax errors
- error snippet shows the file name and the line information
- common errors include:
  - different function names, only one root element, etc.

### Using Debugger in Chrome Dev Tools
- Developer Tools > Sources Tab
- click the line number to add a "Breakpoint"
  - Code execution will stop when it reaches that part
  - step into next to move one step at a time
  - hover over variables to see the values of the variables

### React Developer Tools
- Add the extension to Chrome
- Components
  - shows the content of the component trees