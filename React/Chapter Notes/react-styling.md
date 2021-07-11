# Styling Components
### Conditional / Dynamic Styling
  - In-line styling
    - adding it dynamically / hard coded
```js
const Example = (props) => {
    return (
        <label style={{color: !isValid ? 'red' : 'black'}}>Course Goal</label>
    );
}
```
  - dynamic class styling
```css
.form-control.invalid input {
  border-color: red;
  background: #ffd7d7;
}

.form-control.invalid label {
  color: red;
}
```

```js
const Example = (props) => {
    return (
      <div className={`form-control ${!isValid ? 'invalid' : ''}`}>
    );
}
```

### Styled Components
  - https://styled-components.com/
  - `npm install --save styled-components`
  - invoked via "styled.<html component>``;"
  - selectors are invoked with `&` to pertain to itself
  - style names will be generated automatically

```js
import styled from 'styled-components';

const Button = styled.button`
  font: inherit;

  &:focus {
    outline: none;
  }

  &:hover,
  &:active {
    background: #ac0e77;
  }
`;
```

### Styled Components (Dynamic)

```js

const FormControl = styled.div`
border: 1px solid ${props => props.invalid ? 'red' : '#ccc' };

@media (min-width: 768px) {
    width: auto;
}
`;

const Example = (props) => {
    return (
      <FormControl invalid={!isInvalid}>
    );
}
```

### CSS Modules
- CSS file must be named `<componentName>.module.css`
- classname will then become <file_name>_<css_class_name>__<hash>

```js
import styles from './Button.module.css';

const Button = props => {
  return (
    <button className={styles.button}>Name</button>
  );
};
```

- Dynamic Syntax
  - using template literals
```js
      <div className={`${styles['form-control']} ${!isValid && styles.invalid}`}>
```