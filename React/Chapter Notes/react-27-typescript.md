# TypeScript
- What and Why?
- TypeScript Basics
- Combining React and TypeScript

## Typescript
- superset to JavaScript
- adds more features to the javascript syntax
- not a library for javascript
- adds static typing to JavaScript

## Install locally
- to install `npm install typescript`
- to run the compiler via tsc `npx tsc` 
  - expects a typescript configuration file e.g. `npx tsc <filename>.ts`
  - will create a javascript file

## Declaration
```ts
let age: number = 24;
let userName: string = "Kai";
let isInstructor: boolean = true;

let hobbies: string[];
let person: {
  name: string;
  age: number;
};

let people: {
  name: string;
  age: number;
}[];
```

## Type inference
- infers type based on initialization
- redundant to add type `course: string`
- better practice to remove redundancy
```ts
let course = 'React - The Complete Guide';

course  = 12341; // will cause an error as we stated the initialization
```

## Multiple types
- not redundant if allowing multiple types via pipe `|`
```ts
let course: string | number | boolean = 'React - The Complete Guide';
```

## Struct
```ts
type Person = {
  name: string;
  age: number;
};

let person: Person;

person = {
  name: "Max",
  age: 12,
};
```

## Function type inference
```ts
// infers that the return is number
function add(a: number, b: number) {
  return a + b;
}

// can also set
function add(a: number, b: number) : number | string {
  return a + b;
}
```

## Generics
- types must be the same (instead of `any` which can be of different types)
```ts
// T tells us that their types must be the same (different from any)
function insertAtBeginning<T>(array: T[], value: T) {
  const newArray = [value, ...array];
  return newArray;
}
```

## Adding typescript with create-react-app
`npx create-react-app my-app --template typescript`

## Typescript for function events

- React.FormEvent

```jsx
  const submitHandler = (event: React.FormEvent) => {
    event.preventDefault();
  };

  return (
    <form onClick={submitHandler}>
```

## Typing references

- make sure to explicitly set the initial value

```jsx
  const todoTextInputRef = useRef<HTMLInputElement>(null);

  // add ? to set as null-coalescing
  // add ! if you are sure that there will always be value
  const enteredText = todoTextInputRef.current!.value;

  return (
      <input type="text" id="text" ref={todoTextInputRef} />
  )
```

## Function Props

format is `{ props_name: (parameters) => return_value }`
```jsx
const NewTodo: React.FC<{onAddTodo: (enteredText: string) => void}> = (props) => {
```

## tsconfig.json

- add to where you are compiling typescript code
- invoked when dev server is started
- `target` - target js function depending on the project setup
- `dom` is responsible for the handling of HTMLTypes
- `strict` - set to true; cannot implicitly any values

## Other things
- gatsby.js
  - allows leverage react to produce static websites
  - pre-rendered in advance
  - helps SEO and alternative in Next.js
- preact
  - react with a smaller footprint
  - stripping out internal things
- react native
  - build mobile apps with React