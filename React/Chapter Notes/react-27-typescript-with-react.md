# TypeScript with React

## Typing with props
- automatically has a `children` field
- FC is a generic type
- Use a generic type and plug in props
- Generic because different props have different values
- FC Generic merges to the base generic type
```tsx
import React from 'react';

// FC = functional components
const Todos: React.FC<{ items: string[] }> = (props) => {
  return (
    <ul>
      {props.items.map((item) => (
        <li key={item}>{item}</li>
      ))}
    </ul>
  );
};

export default Todos;
```

## Data Model
- we can create classes for data models

```tsx
class Todo {
  id: string;
  text: string;

  constructor(todoText: string) {
    this.text = todoText;
    this.id = new Date().toISOString();
  }
}

export default Todo;
```

- and then used as
```tsx
import React from "react";
import Todo from "../models/todo";

const Todos: React.FC<{ items: Todo[] }> = (props) => {
  return (
    <ul>
      {props.items.map((item) => (
        <li key={item.id}>{item.text}</li>
      ))}
    </ul>
  );
};

export default Todos;
```