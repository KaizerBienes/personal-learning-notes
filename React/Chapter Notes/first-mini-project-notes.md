# Structure

### src/components
- UI - general UI components which can be used in multiple places
- Layout - header component and other components related to it
- Meals - holds list of meals and individual meals
- Cart - cart-related components


### Layout
- Header - containing header tab and image 

### Including an image

```js
import React, { Fragment } from 'react';

import mealsImage from '../../assets/meals.jpg';

const Header = (props) => {
    return <Fragment>
        <img src={mealsImage}/>
    </Fragment>
};

export default Header;
```

### Spread operator inside jsx


```js
    return (
        <Input label="Amount" input={{
            id: 'amount',
            type: 'number',
            min: '1',
            max: '5',
            step: '1',
            defaultValue: '1'
        }} />
    );

    // in another file
    return (
        <input {...props.input} />
    );
```

### Reduce
- transforms an array to a single value
- first argument is a function, second argument is a starting value
```js
  const numberOfCartItems = cartCtx.items.reduce((curNumber, item) => {
    return curNumber + item.amount;
  }, 0);
```