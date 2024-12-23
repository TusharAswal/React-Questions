# JavaScript Concepts and Examples

## Prototype Chain
In JavaScript, the prototype chain allows objects to inherit properties and methods from other objects.

### Example:
```javascript
// Creating a simple object
let animal = {
    species: "Mammal",
    sound: function() {
      console.log("Some sound");
    }
};

// Creating another object that inherits from 'animal'
let dog = Object.create(animal);
dog.breed = "Labrador";

// Accessing properties from dog
console.log(dog.species);  // "Mammal" (inherited from animal)
dog.sound();  // "Some sound" (inherited from animal)
console.log(dog.breed);  // "Labrador" (own property of dog)
```

### Prototype Chain Flow:
- When accessing `dog.species`, JavaScript looks for `species` on `dog`. If not found, it checks the prototype (`animal`).
- When calling `dog.sound()`, JavaScript searches the prototype chain for the method.
- `dog.breed` is found directly on `dog`, so no prototype lookup is needed.

---

## Array Methods
### Slice
The `slice()` method extracts a shallow copy of a portion of an array without modifying the original array.
```
let x=["A","B","C","D","E","F","G"]
console.log(x.slice(0,3)) // Returns [ 'A', 'B', 'C' ]
```
### Splice
The `splice()` method modifies the contents of an array by adding, removing, or replacing elements.
```
let x=["A","B","C","D","E","F","G"]
let y=x.splice(0,3)
console.log(y) //Returns [ 'A', 'B', 'C' ]
console.log(x) // Returns [ 'D', 'E', 'F', 'G' ]
```
---

## Promises
### Promise.all
Waits for all promises to resolve or for any to reject.

### Promise.allSettled
Waits for all promises to settle (resolve or reject) and returns an array of results.

### Promise.race
Resolves or rejects as soon as the first promise settles.

### Promise.any
Resolves as soon as the first promise fulfills or throws an `AggregateError` if all reject.

---

## InteractionManager (React Native)
Used for managing animations or high-priority UI tasks.

```javascript
InteractionManager.runAfterInteractions(() => {
    console.log('Heavy computation starts');
    const fetchedData = Array.from({ length: 100000 }, (_, i) => i + 1);
    setData(fetchedData);
    setLoading(false);
    console.log('Heavy computation ends');
});
```

---

## JavaScript Scopes
### `var`, `let`, `const`
- **`var`**: Function-scoped.
- **`let` and `const`**: Block-scoped.
- **Hoisting**:
  - `var`: Initialized to `undefined` during hoisting.
  - `let` and `const`: Uninitialized and throw a `ReferenceError` if accessed before declaration.

---

## Shallow Copy vs. Deep Copy
### Shallow Copy
Copies only the first level of properties. Nested objects/arrays still reference the original structure.
- Using Spread Operator (...)
```javascript
const obj = { a: 1, b: { c: 2 } };
const shallowCopy = { ...obj };
```
- Using Object.assign
```javascript
const obj = { a: 1, b: { c: 2 } };
const shallowCopy = Object.assign({}, obj);
```
### Deep Copy
Creates a completely independent copy of the original object/array.
- Using structuredClone (Modern and Recommended)
```javascript
const obj = { a: 1, b: { c: 2 } };
const deepCopy = structuredClone(obj);
```
- Using JSON.stringify and JSON.parse
```javascript
const deepCopy = JSON.parse(JSON.stringify(original));
```
---

## JavaScript Closures
A closure is a function that "remembers" variables from its outer scope even after the outer function has finished executing.

### Example:
```javascript
function outer() {
    const a = 10;
    return function inner() {
        console.log(a); // Uses `a` from outer scope.
    };
}
```
**Drawbacks of closures**

1. Increased Memory Usage:
- A closure retains references to variables in its lexical scope (even after the function execution completes).
- If these variables are large objects or arrays, they stay in memory as long as the closure exists, which could cause memory leaks.
2. Memory Leaks:
- Closures can unintentionally prevent variables from being garbage collected because of retained references, leading to memory leaks.
3. Debugging Complexity:
- Variables can come from multiple levels of the scope chain
- When closures are deeply nested, understanding where a variable comes from can be difficult.
---

## React Concepts
### Higher-Order Component (HOC)
An advanced technique for reusing component logic.
- Creating a HOC
```javascript
const withEnhancement = (WrappedComponent) => {
    return (props) => {
        return <WrappedComponent {...props} newProp="Enhanced!" />;
    };
};
```
- Using that HOC
```javascript
/* MyComponent is a child component that is being wrapped in HOC created and then being assigned to a new variable EnhancedComponent that can then be used to display UI */
const EnhancedComponent = withEnhancement(MyComponent);
export default EnhancedComponent;
```

### Context API
Used to share global state without prop drilling.
- Create a context
```javascript
const MyContext = React.createContext();
```
- Wrap the children component in newly created provider
```javascript
export const AppProvider = ({ children }) => {
  const [user, setUser] = useState({ name: 'John Doe', age: 25 });
  return (
    <MyContext.Provider value={{ user, setUser }}>
    {/* children */}
    </MyContext.Provider>
  );
};
```
- Use the new context values inside the children
```javascript
const App = () => {
  return (
    <AppProvider>
        <UserProfile />
    </AppProvider>
  );
};

const UserProfile = () => {
  const { user, setUser } = useContext(AppContext);
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Name: {user.name}</Text>
      <Text style={styles.text}>Age: {user.age}</Text>
    </View>
  );
};
```

### React.memo vs memo vs useCallbacks
| Feature                | `React.memo`                                   | `useMemo`                                        | `useCallback`                                   |
|------------------------|-----------------------------------------------|-------------------------------------------------|-------------------------------------------------|
| **Purpose**            | Memoizes a component to avoid unnecessary re-renders when props haven't changed. | Memoizes the result of a computation to avoid recalculating on every render. | Memoizes a function definition to avoid re-creating it on every render. |
| **Type**               | Higher-Order Component (HOC)                  | Hook                                            | Hook                                            |
| **Usage**              | Wraps a functional component to optimize rendering. | Wraps a computation or value.                    | Wraps a function to keep its reference stable.  |
| **Arguments**          | `React.memo(Component, [compareFunction])`    | `useMemo(() => computeValue, [dependencies])`    | `useCallback(() => callbackFunction, [dependencies])` |
| **Return Value**       | A memoized version of the component.          | The memoized value of the computation.           | A memoized version of the callback function.    |
| **Primary Use Case**   | Prevents unnecessary re-renders of components based on unchanged props. | Prevents recalculating expensive computations on each render. | Prevents creating a new function on every render, useful when passing functions as props. |
| **Dependencies**       | No dependencies; compares props shallowly.    | Depends on a dependency array for recomputation. | Depends on a dependency array to determine when the function should be updated. |
| **Example**            | ```js<br>const MyComponent = React.memo(function MyComponent(props) {<br> return <Text>{props.name}</Text>;<br>});``` | ```js<br>const memoizedValue = useMemo(() => expensiveComputation(), [dependency]);``` | ```js<br>const memoizedCallback = useCallback(() => {<br> doSomething();<br>}, [dependency]);``` |
| **When to Use**        | Use when a component's re-rendering is unnecessary because its props haven't changed. | Use when a computation is expensive and should only run when certain dependencies change. | Use when passing functions to child components, and you want to avoid unnecessary re-creations of the function. |

### React.memo()
```javascript
const UserProfile = memo(({ name, age }) => {
  console.log('Rendering UserProfile');
  return (
    <View>
      <Text>Name: {name}</Text>
      <Text>Age: {age}</Text>
    </View>
  );
});
```
### useMemo
```javascript
 const expensiveCalculation = useMemo(() => {
    console.log('Running expensive calculation');
    return count * 2;
  }, [count]);
```

### useCallback
```javascript
const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]); // Function re-created only when 'count' changes
```

### React Portals
Render child components outside their parent in the DOM.
```javascript
function Modal({ message }) {
    return ReactDOM.createPortal(
        <div className="modal">
            <h1>{message}</h1>
        </div>,
        document.getElementById('modal-root')
    );
}
```

### useImperativeHandle
Expose methods/values from a child component to a parent component.

- Parent component
```javascript
const App = () => {
  const customButtonRef = useRef();
  const handleFocus = () => {
    customButtonRef.current.focus();
  };

  const handleReset = () => {
    customButtonRef.current.reset();
  };

  return (
    <>
      <CustomButton ref={customButtonRef} />
      <Button title="Focus on Button" onPress={handleFocus} />
      <Button title="Reset Button" onPress={handleReset} />
    </>
  );
};
export default App;
```
- Child component
```javascript
const CustomButton = forwardRef((props, ref) => {
  const buttonRef = useRef(null);
  useImperativeHandle(ref, () => ({
    focus: () => {
      buttonRef.current && buttonRef.current.focus();
    },
    reset: () => {
      console.log('Resetting the button');
    }
  }));

  return (
    <>
      <Button ref={buttonRef} title="Click Me" onPress={() => console.log('Button Clicked')} />
    </>
  );
});

export default CustomButton;
```


---

## Event Loop and Task Queue
### Synchronous tasks vs. Microtasks vs. Macrotasks
- **Synchronous**: All loops like while,do while and for are synchronous and are executed immediately.
- **Microtasks**: Promises, MutationObserver callbacks.
- **Macrotasks**: `setTimeout`, `setInterval`, DOM events.
- **Priority**: Synchronous > Microtasks > Macrotasks.

```javascript
console.log(1); // Printed first, this is a synchronous operation and will be executed immediately in the current call stack.

setTimeout(() => console.log(2), 0);// Printed last as this is a macro task

Promise.resolve().then(() => console.log(3)); // Microtask have higher priority than macrotasks, so it will execute after the current synchronous code

console.log(4); // Printed second, this is a synchronous operation and will be executed immediately in the current call stack.

// Final Output: 1, 4, 3, 2
```
---

## TypeScript
### Interface vs. Type
- **Interface**: Best for defining object/class structure. Supports declaration merging.
- **Type**: More versatile, can define primitives, unions, intersections, tuples.

### TypeScript Benefits in React Native
- Static typing reduces runtime errors.
- Refactoring is safer and consistent.
- Enhanced autocompletion and documentation support in IDEs.

---

## SSL Pinning (React Native)
A security measure to mitigate MITM attacks.
```javascript
import axios from 'axios';
import RNSslPinning from 'react-native-ssl-pinning';

const axiosInstance = axios.create({
    baseURL: 'https://example.com',
    httpsAgent: new RNSslPinning.Agent({
        certs: ['certificates/my-cert.cer'],
    }),
});
```

---

## Generic Function Example (TypeScript)
In TypeScript, generic functions allow you to write functions that can work with any type, while still enforcing type safety. Generics are a powerful feature to make your code reusable and flexible.
```typescript
function identity<T>(value: T): T {
    return value;
}
const numberValue = identity<number>(42);
const stringValue = identity<string>("Hello, Generics!");
```

---
### Swapping Variables Without Temp Variable
```javascript
let a = 5, b = 10;
[a, b] = [b, a];
```
### Call,Apply,Bind (Function Context Methods or Function Binding Methods in JavaScript)
```javascript
const person = {
  firstName: 'John',
  lastName: 'Doe'
};
```
1. **Call:** The call method allows you to call a function with a specified this value and individual arguments.
```javascript
function fullName(city, country) {
  console.log(`${this.firstName} ${this.lastName}, ${city}, ${country}`);
}
fullName.call(person, 'New York', 'USA'); // Output: John Doe, New York, USA
```
2. **Apply:** The apply method is similar to call, but it accepts the arguments as an array (or array-like object) instead of individual arguments.
```javascript
function fullName(city, country) {
  console.log(`${this.firstName} ${this.lastName}, ${city}, ${country}`);
}
fullName.apply(person, ['New York', 'USA']); // Output: John Doe, New York, USA
```
3. **Bind:** The bind method creates a new function that, when called, has its this value set to the provided value and prepends any provided arguments to the argument list.
```javascript
function fullName(city, country) {
  console.log(`${this.firstName} ${this.lastName}, ${city}, ${country}`);
}
const boundFullName = fullName.bind(person, 'New York');
boundFullName('USA'); // Output: John Doe, New York, USA
```

## Generator Functions
Generator functions are a special type of function in JavaScript that allows pausing and resuming its execution during runtime.
- next() is used to pass down value for yeild in an generator function.
- yield is used to pause a function waiting for a value from next()

```javascript
function* generatorExample() {
/*
Step 1.1:
Pauses here first waiting for a value for first yeild
*/
  const firstValue = yield 'First yield paused'; 
/*
Step 2.1:
Pauses in above, but because value is now present in next() its successfully executed and console is printed
*/
  console.log('First value received:', firstValue);
  return 'Generator complete!';
}

const gen = generatorExample();
/* Step 1:
Calls a generator function without any value
returns { value: 'First yield paused', done: false }
*/
console.log(gen.next()) 
/* Step 2:
This time a value is passed to the generator function
*/
console.log(gen.next('Value for first yield').value); 
```
### Increase performance in RN
- Use FlatList or SectionList for Long Lists
- Memoization Using React.memo, useMemo, and useCallback
- Avoid Inline Functions and Objects in JSX
- Optimize Images using react-native-fast-image
- Use InteractionManager for Heavy Tasks
- Use Lazy Loading and Code Splitting
- Use Hermes Engine

### Key Testing Types (Jest)
- Unit testing is a software testing technique that involves testing individual components or functions of an application in isolation to ensure they behave as expected.
```javascript
// utils/calculateSum.js
export const calculateSum = (a, b) => a + b;

// utils/calculateSum.test.js
import { calculateSum } from './calculateSum';

test('calculateSum adds numbers correctly', () => {
  expect(calculateSum(2, 3)).toBe(5);
});
```

- Component testing involves testing individual components of a user interface (UI) in isolation to ensure they function correctly.
```javascript
// components/Greeting.js
import React from 'react';
import { Text, View } from 'react-native';

const Greeting = ({ name }) => (
  <View>
    <Text>Hello, {name}!</Text>
  </View>
);

export default Greeting;
```

```javascript
// components/Greeting.test.js
import React from 'react';
import { render } from '@testing-library/react-native';
import Greeting from './Greeting';

test('renders Greeting component with name', () => {
  const { getByText } = render(<Greeting name="John" />);
  expect(getByText('Hello, John!')).toBeTruthy();
});
```

- Snapshot testing is a type of testing used to ensure that a component's UI does not change unexpectedly.
```javascript
// components/Button.js
import React from 'react';
import { Text, TouchableOpacity } from 'react-native';

const Button = ({ label, onPress }) => (
  <TouchableOpacity onPress={onPress}>
    <Text>{label}</Text>
  </TouchableOpacity>
);

export default Button;
```
```javascript
// components/Button.test.js
import React from 'react';
import renderer from 'react-test-renderer';
import Button from './Button';

test('Button component snapshot', () => {
  const tree = renderer.create(<Button label="Click me" onPress={() => {}} />).toJSON();
  expect(tree).toMatchSnapshot();
});
```

### Mocking Functions and API Calls
```javascript
// components/UserDetails.test.js
import React from 'react';
import { render, waitFor } from '@testing-library/react-native';
import UserDetails from './UserDetails';

jest.mock('./UserDetails', () => ({
  fetchUser: jest.fn(() => Promise.resolve({ name: 'John Doe' })),
}));

test('renders user details after fetching data', async () => {
  const { getByText } = render(<UserDetails />);
  await waitFor(() => expect(getByText('John Doe')).toBeTruthy());
});
```
# Copying by refrence
- Here the value of c changes if changes are made to d or c as they share same refrence in memory
```javascript
let c = { greeting: 'Hey!' };
let d;
d = c; // Both c and d point to the same object in memory
c.greeting = 'Hello';
console.log(d.greeting); // Outputs: 'Hello'
```
- If object is copied using {...} or rest operator only the top most values are copied independently all the nested values are copied by refrence
```javascript
let c = { greeting: 'Hey!', details: { age: 25 } };
let d;
d = { ...c }; // Shallow copy of c
c.greeting = 'Hello';
c.details.age = 30; // Changes the nested object
console.log(d.greeting); // Outputs: 'Hey!' (independent copy of top-level property)
console.log(d.details.age); // Outputs: 30 (nested object is still shared by reference)
```

### forEach vs Map
| **Feature**           | **`forEach`**                            | **`map`**                                |
|-----------------------|-----------------------------------------|-----------------------------------------|
| **Purpose**           | For side effects (e.g., logging)       | To transform and create a new array     |
| **Return Value**      | `undefined`                            | A new array                             |
| **Mutability**        | Can modify the original array or external state | Does not modify the original array      |
| **Chaining**          | Not chainable                          | Chainable                               |
| **Use Case**          | Performing an action per element       | Transforming an array                   |

## Object.assign() vs. Object.create()
| **Feature**              | **`Object.assign()`**                       | **`Object.create()`**                      |
|--------------------------|--------------------------------------------|-------------------------------------------|
| **Purpose**              | Copies properties from source to target   | Creates a new object with a specific prototype |
| **Mutates Data**         | **Yes** (modifies the target object)       | **No** (creates a new object)             |
| **Prototype Inheritance**| Does not handle prototype inheritance      | Creates an object with inherited prototype |
| **Return Value**         | The modified target object                 | A new object                              |

### What is an Error Boundary in React?
Error Boundaries are React components that catch JavaScript errors in their child component tree, log those errors, and display a fallback UI instead of crashing the entire application. They help prevent the app from breaking when an error occurs in a specific part of the UI.
Key Methods in Error Boundaries
static getDerivedStateFromError(error)
Updates the state so the next render will show a fallback UI.
componentDidCatch(error, info)
Logs error information.

### Basic Sum Function in TypeScript
```javascript
function sum(a: string | number, b: string | number): number {
  // Convert both parameters to numbers if they are strings
  const num1 = typeof a === 'string' ? parseFloat(a) : a;
  const num2 = typeof b === 'string' ? parseFloat(b) : b;
  return num1 + num2;
}
```

# Generic functions typescript
```javascript
function identity<T>(arg: T): T {
  return arg;
}
const result1 = identity(10);    // T inferred as number
const result2 = identity('Hello'); // T inferred as string
console.log(result1); // 10
console.log(result2); // 'Hello'
```
---

This README consolidates foundational and advanced JavaScript and React concepts for developers.

