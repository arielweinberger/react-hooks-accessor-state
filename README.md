# react-hooks-accessor-state

# The Problem
When Reaect 16.8 was released, React Hooks were introduced. React provided us with a way to have stateful functional components (goodbye, class-based components), using `useState`.

Defining the getter/setter of a state property was done using [Array Destructioning Assignment](https://developer.mozilla.org/nl/docs/Web/JavaScript/Reference/Operatoren/Destructuring_assignment).

Components can have several stateful properties. In such case, the state will be declared like so:

``` 
  const [name, setName] = useState();
  const [age, setAge] = useState();
  const [country, setCountry] = useState();
  const [role, setRole] = useState();
  const [company, setCompany] = useState();
```

This works fine, but *we repeat ourselves a lot*. Also, if we want to dynamically refer to a state property, we cannot do it without making additional changes (for example, with class-based components you could do `this.state[someVariable]`).

# How does "react-hooks-accessor-state" save the day?
This library takes advantes of the [Property Accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors) feature of JavaScript. You will create one variable in your functional component declaration, and your state will be stored within that object.

Behind the scenes, `React.useState` is still used. This library provides a wrapper for comfort - while still keeping the actual state immutable, yet providing a simple and intuitive way to manage your functional component's state using getters and setters.

The result? Cleaner code that is still dynamically referrable.

## createAccessorState
Creates a state object based on the given object. A default value can be provided per property.

### Example:
#### Before (React Hooks):
```
  import React, { useState } from 'react';

  export default function Person() {
    const [name, setName] = useState();
    const [age, setAge] = useState();
    const [country, setCountry] = useState();
    const [role, setRole] = useState();
    const [company, setCompany] = useState();
  }
```

#### After (react-hooks-accessor-state):
```
  import React from 'react';
  import createAccessorState from 'react-hooks-accessor-state';

  export default function Person() {
    const state = createAccessorState({
      name: 'Ariel Weinberger', // This is a default value
      age: null,
      country: null,
      role: 'Unemployed', // This is a default value
      company: null
    });
  }
```

## Using accessors (getters and setters)
After creating an accessor state, you can retrieve and define your state property values using getters and setters.
When you set a property, the proper React Hook is called behind your scenes, so your state *remains* immutable.

### Example:
```
import React from 'react';
import createAccessorState from 'react-hooks-accessor-state';

export default function Counter() {
  // Defining the state 
  const state = createAccessorState({
    count: 0
  });
  
  const increment = () => {
    // Setting a new value
    state.count = state.count + 1;
  };

  const decrement = () => {
    // Setting a new value
    state.count = state.count - 1;
  };

  return (
    <div>
      <h1>Count value:</h1>
      /* Retrieving the value */
      <h1>{state.count}</h1>

      <button onClick={increment}>Click me for +1</button>
      <button onClick={decrement}>Click me for -1</button>
    </div>
  );
}
```
