# JavaScript Concepts and Examples

### Currying
- In below example curring become useful if finalPrice or tax remain same for multipe items. Then only extra function be used.
This become more important if the data (price/tax) are large obj they then dont need to be sent again and again, like we do in multi param fn.
```javascript
function finalPrice(price){
    return function(tax){
        return function(extraCharge){
            return price+tax+extraCharge
        }
    }
}
// Calculating price for item A
const priceA=finalPrice(10)
const taxA=priceA(10)
const extraA=taxA(10)
console.log("Price for A",extraA)
// Calculating price for item B
const priceB=finalPrice(25)
const taxB=priceB(5)
const extraB=taxB(7)
console.log("Price for B",extraB)
```

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
**slice(startIndex, endIndex)**:
The `slice()` method extracts a shallow copy of a portion of an array without modifying the original array.
the element at the end index is not included in the result
```
let x=["A","B","C","D","E","F","G"]
console.log(x.slice(0,3)) // Returns [ 'A', 'B', 'C' ]
```
### Splice
**splice(startIndex, deleteCount, itemToAdd1)**
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
```javascript
const promise1 = new Promise((resolve, reject) => setTimeout(resolve, 1000, "First"));
const promise2 = new Promise((resolve, reject) => setTimeout(resolve, 2000, "Second"));
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 1500, "Third"));
Promise.all([promise1, promise2, promise3])
  .then(results => {
    console.log(results); // ["First", "Second", "Third"]
  })
  .catch(error => {
    console.log(error); // If any promise rejects, the entire promise chain is rejected
  });
```
### Promise.allSettled
Waits for all promises to settle (resolve or reject) and returns an array of results.
```javascript
const promise1 = new Promise((resolve, reject) => setTimeout(resolve, 1000, "First"));
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 2000, "Error"));
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 1500, "Third"));

Promise.allSettled([promise1, promise2, promise3])
  .then(results => {
    console.log(results);
    // [
    //   { status: "fulfilled", value: "First" },
    //   { status: "rejected", reason: "Error" },
    //   { status: "fulfilled", value: "Third" }
    // ]
  });
```
### Promise.race
Promise.race takes an array of promises and returns a single promise that resolves or rejects as soon as any one of the input promises resolves or rejects.
```javascript
const promise1 = new Promise((resolve, reject) => setTimeout(resolve, 1000, "First"));
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 500, "Error"));
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 1500, "Third"));

Promise.race([promise1, promise2, promise3])
  .then(result => {
    console.log(result); // "Error" because promise2 rejects first
  })
  .catch(error => {
    console.log(error); // "Error"
  });
```

### Promise.any
Resolves as soon as the first promise fulfills or throws an `AggregateError` if all reject.
```javascript
const promise1 = new Promise((resolve, reject) => setTimeout(reject, 1000, "Error"));
const promise2 = new Promise((resolve, reject) => setTimeout(resolve, 500, "First"));
const promise3 = new Promise((resolve, reject) => setTimeout(resolve, 1500, "Third"));

Promise.any([promise1, promise2, promise3])
  .then(result => {
    console.log(result); // "First", because it's the first resolved promise
  })
  .catch(error => {
    console.log(error); // If all promises reject, this will catch the error
  });
```
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
```javascript
function example() {
  var x = 10; // Function-scoped
  let y = 20; // Block-scoped
  const z = 30; // Block-scoped

  if (true) {
    var a = 40; // Function-scoped
    let b = 50; // Block-scoped
    const c = 60; // Block-scoped
    console.log(a, b, c); // 40, 50, 60
  }

  console.log(a); // 40 (Accessible because `var` is function-scoped)
  // console.log(b); // ReferenceError: b is not defined (Block-scoped)
  // console.log(c); // ReferenceError: c is not defined (Block-scoped)
}

// Variables declared inside the function are NOT accessible outside:
example();
// console.log(x, y, z); // ReferenceError: x is not defined
```
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
- An advanced technique for reusing component logic.
- Adding common behavior (e.g., authentication, logging).
- Managing state or props.
- Handling side effects like data fetching.
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
### Higher Order Functions (HOFs):
- A higher-order function is a function that either:
Takes one or more functions as arguments, or
Returns a function as its result.

- Array methods like map, filter, and reduce.
- Function composition or currying.
- Custom utility functions (e.g., debounce, throttle).
```javascript
// Currying
const multiplyBy = (factor) => (number) => number * factor;
const double = multiplyBy(2);
console.log(double(4)); // 8
// Map,Filter, Reduce
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Chain: Filter out even numbers, double them, then sum them
const result = numbers
  .filter(num => num % 2 === 0)  // Get even numbers
  .map(num => num * 2)           // Double each even number
  .reduce((acc, num) => acc + num, 0); // Sum them up

console.log(result);  // 60 (i.e., (2*2) + (4*2) + (6*2) + (8*2) + (10*2))
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
  const { user, setUser } = useContext(MyContext);
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

### Pure components
- A pure component re-renders only when its props or state change.
- Created using **React.PureComponent** in class based compoenent and **React.memo** in a functional based component
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
The Event Loop is a mechanism that enables JavaScript to handle asynchronous tasks, ensuring non-blocking execution of code.
- JavaScript is single-threaded, meaning it can execute only one task at a time.
- The Event Loop monitors the Call Stack and the Task Queue (or Microtask Queue) to determine what should run next.
- It continuously checks if the Call Stack is empty, and if so, it pushes tasks from the Task Queue or Microtask Queue onto the Call Stack for execution.
  
### Synchronous(Callstack) tasks vs. Microtasks vs. Macrotasks
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
### Redux and React's Context API 
| Feature/Aspect              | Redux                                            | Context API                                       |
|-----------------------------|--------------------------------------------------|--------------------------------------------------|
| **Purpose**                 | State management library for predictable and centralized app state. | Tool for passing data deeply through the component tree without props drilling. |
| **Learning Curve**          | Steeper learning curve due to additional concepts like actions, reducers, middleware, etc. | Easier to learn and implement with minimal boilerplate. |
| **Scalability**             | Suitable for large-scale applications with complex state. | Better for small to medium-sized applications with simpler state. |
| **Performance**             | Optimized with middlewares like `redux-thunk` and `redux-saga`. Can be fine-tuned with selective subscriptions. | Prone to performance issues when used for frequently changing state, as all consumers re-render. |
| **Setup**                   | Requires installing Redux, React-Redux, and potentially middlewares. | Built into React, no external libraries required. |
| **Middleware Support**      | Advanced middleware (e.g., `redux-thunk`, `redux-saga`) for handling side effects. | No built-in middleware; side effects are managed manually or with custom solutions. |
| **DevTools Support**        | Excellent DevTools for state inspection, time-travel debugging, and performance monitoring. | Limited to React DevTools for debugging context changes. |
| **Boilerplate Code**        | Requires more boilerplate: actions, reducers, and a store setup. | Minimal boilerplate; simply create a context and use it. |
| **State Structure**         | Centralized global state; all state logic is in one place. | Decentralized state, defined as separate contexts. |
| **Third-Party Ecosystem**   | Rich ecosystem with libraries for side effects, persistence, and more. | Limited ecosystem; primarily just React-based. |
| **Flexibility**             | Highly customizable; supports advanced patterns like middleware, selectors, etc. | Limited to React's functionality and API. |
| **When to Use**             | Large apps with complex, shared, and frequently changing state. | Small apps or when you need to avoid prop drilling for simpler state. |

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
//call(thisArg, arg1, arg2, /* …, */ argN)
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

### Replace Stack React Navigation
```javascript
import { StackActions } from '@react-navigation/native';
navigation.dispatch(StackActions.replace('NewScreen', { /* params */ }));
```
### Hermes
- Hermes is a lightweight, high-performance JavaScript engine designed by Meta (formerly Facebook) specifically for React Native apps
**1. Improves Startup Time:**
Hermes uses Ahead of Time compilation(AOT), which compiles the JS bundle during build time instead of app runtime which the traditional JS engine(JavaScriptCore) do. JavaScriptCore used Just in Time compilation.

**2. Reduces Memory Usage & Smaller App Size and Increases performance:**
Hermes minimizes memory usage & reduce app size by precompiling JS bundle into bytecode. Bytecode is smaller in size than traditional JS source code.

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
### Running test cases with JEST
- **jest**
Runs all the test files in your project. By default, Jest looks for files with .test.js or .spec.js extensions in the __tests__ folder or files with these extensions anywhere in your project.

- **jest --watch**
Watches your files for changes and automatically reruns the tests when a file is saved. Useful during development to get instant feedback on your tests.

- **jest --onlyFailures**
Reruns only the tests that failed in the previous run. Useful for quickly debugging failures after running the full test suite.

- **jest --coverage**
Generates a test coverage report, showing which parts of your code are covered by tests. Outputs detailed information about the lines, functions, and branches covered.

- **jest --updateSnapshot**
Updates Jest snapshots for tests that use snapshot testing. Use this when your component output changes and you’re confident the new output is correct.

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
### Custom hooks
Custom hooks always start with the prefix use, and they can use other built-in hooks like useState, useEffect, useContext, etc., within them.
- Code Reusability: Avoid duplicating logic across components by centralizing common functionality.
- Improved Readability: Keeps components cleaner and easier to read by moving complex logic out of the component.
- Separation of Concerns: Better organizes code by separating stateful logic from component rendering.
- Testing: Custom hooks make it easier to test logic independently of the components.

### Avoid un-necessary re-renders in custom hooks
- Use useMemo for Memoizing Values
- Use useCallback for Memoizing Functions

### **`useEffect` vs. `useLayoutEffect`**

| Feature/Aspect            | **`useEffect`**                                                                 | **`useLayoutEffect`**                                                                                      |
|----------------------------|--------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| **Execution Timing**       | Runs **asynchronously** after the browser has painted the UI.                  | Runs **synchronously** after all DOM mutations but **before** the browser paints the UI.                  |
| **Use Case**               | Best for non-visual side effects like API calls, logging, or setting up subscriptions. | Best for side effects that affect the **layout or appearance** of the DOM (e.g., measuring DOM nodes or adjusting styles). |
| **Impact on UI**           | Does **not block the rendering** of the UI.                                    | Can **block rendering** until the side effect completes, as it runs synchronously.                        |
| **Performance**            | More efficient for most cases as it doesn’t delay rendering.                  | May hurt performance if used unnecessarily because it delays the paint phase.                             |
| **React Hook Type**        | Runs after the DOM is updated and the browser has completed rendering.         | Runs **before** the browser renders the next frame, ensuring DOM consistency before user sees the changes. |
| **Typical Usage**          | - Fetching data.<br>- Subscribing/unsubscribing to events.<br>- Logging.        | - Measuring DOM size/position.<br>- Adding inline styles based on measurements.<br>- Synchronizing animations. |
| **Example Scenario**       | Use for fetching data from an API.                                             | Use for recalculating layout or synchronizing animations.                                                 |

### Hermes engine
- Improves App Startup Time
Hermes reduces app startup times by using an optimized bytecode format, which allows the JavaScript to load and execute faster compared to traditional JavaScript engines like V8 or JavaScriptCore.

- Reduces Memory Usage
It uses an optimized memory management system, which significantly lowers memory consumption, making it ideal for apps running on low-end devices.

- Ahead-of-Time (AOT) Compilation
JavaScript code is compiled into bytecode before the app is bundled. This eliminates the need for Just-in-Time (JIT) compilation at runtime, reducing overhead.

- Smaller JavaScript Bundle Size
Hermes compiles JavaScript code into an efficient bytecode format that is smaller than raw JavaScript, reducing the overall size of the app package.

- Optimized Garbage Collection
Hermes uses a memory-efficient garbage collection strategy designed for React Native apps, which improves runtime performance and responsiveness.

- Cross-Platform Support
Hermes is available for both Android and iOS platforms, making it a viable choice for improving performance across all devices.

### Redux working
- Dispatch Action
```javascript
const dispatch=useDispatch()
dispatch({ type: 'INCREMENT', payload: 1 });
```
- Action Type: The dispatched action contains a type (required) and a payload (optional). The type determines the intent of the action.
```javascript
{ type: 'INCREMENT', payload: 1 }
```
- Middleware:Middleware (like Thunk or Logger) intercepts actions before they reach the reducer.
- Reducer:The reducer receives the current state and the action, and it returns the updated state based on the action type.
```javascript
const counterReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + action.payload };
    default:
      return state;
  }
};
```
- Update Store: The store updates its state with the new state returned by the reducer.
- Re-render Components

  ### Render Hijacking HOC
Render hijacking occurs when a component's rendering logic is intercepted, allowing you to modify the component's behavior without directly modifying the component itself.

```javascript
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;
// HOC to manipulate props and hijack render
const withGreetingManipulation = (WrappedComponent) => {
  return (props) => {
    // Manipulating the props before passing them down
    const newProps = {
      ...props,
      name: props.name ? props.name.toUpperCase() : 'Anonymous', // Hijack the 'name' prop and modify it
    };
    
    // Render hijacking by modifying the component's rendering logic
    return <WrappedComponent {...newProps} />;
  };
};

// Enhancing the Greeting component with the HOC
const EnhancedGreeting = withGreetingManipulation(Greeting);

// Using the enhanced component
const App = () => {
  return <EnhancedGreeting name="Lydia" />;
};

export default App;
```
### useReducer
useReducer is a React hook used for managing state in components, especially when the state logic is more complex or needs to be updated based on multiple actions.
```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```
# Features Comparison

| Feature                          | Axios                                  | Fetch                                   |
|-----------------------------------|----------------------------------------|-----------------------------------------|
| **Browser Support**               | Works in all modern browsers, including Internet Explorer | Works in modern browsers, requires a polyfill for IE |
| **Request Interceptors**          | Yes                                    | No                                      |
| **Response Interceptors**         | Yes                                    | No                                      |
| **Automatic JSON Parsing**        | Yes                                    | No, needs `.json()` to parse           |
| **Timeouts**                       | Yes, built-in timeout support         | No, needs manual implementation        |
| **Error Handling**                | Rejects on HTTP error codes           | Rejects on network errors, need manual check for HTTP errors |
| **File Uploads**                  | Native support for `FormData`         | Manual implementation                  |
| **Request Cancellation**          | Yes, via `CancelToken`                 | Yes, via `AbortController`             |
| **API**                           | Can be used with `POST`, `GET`, etc.   | Can be used with `POST`, `GET`, etc.    |

### Creating object in JS
- Using Object Literal
```javascript
const person = {
  name: "John",
  age: 30,
  greet: function () {
    console.log(`Hello, my name is ${this.name}`);
  }
};
console.log(person.name); // Output: John
person.greet();           // Output: Hello, my name is John
```
-  Using Object.create()
```javascript
const prototypeObject = {
  greet: function () {
    console.log(`Hello, my name is ${this.name}`);
  }
};
const person = Object.create(prototypeObject);
person.name = "Jane";
person.age = 25;
person.greet(); // Output: Hello, my name is Jane
```
- Using a Constructor Function
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.greet = function () {
    console.log(`Hello, my name is ${this.name}`);
  };
}
const person1 = new Person("Alice", 28);
const person2 = new Person("Bob", 35);
person1.greet(); // Output: Hello, my name is Alice
person2.greet(); // Output: Hello, my name is Bob
```
- Using ES6 Classes
```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}
const person1 = new Person("Charlie", 40);
const person2 = new Person("Diana", 32);
person1.greet(); // Output: Hello, my name is Charlie
person2.greet(); // Output: Hello, my name is Diana
```
- Using Object Constructor
```javascript
const person = new Object();
person.name = "Eve";
person.age = 22;
person.greet = function () {
  console.log(`Hello, my name is ${this.name}`);
};
console.log(person.name); // Output: Eve
person.greet();           // Output: Hello, my name is Eve
```
- Using JSON.parse
```javascript
const person = JSON.parse('{"name": "Hank", "age": 20}');
console.log(person.name); // Output: Hank
console.log(person.age);  // Output: 20
```
### Set v/s Weak Set
- ***Set***:
    A Set ensures that all values are unique. If you try to add a duplicate value, it will be ignored.
```javascript
const mySet = new Set();
mySet.add(1);
mySet.add(2);
console.log(mySet.has(1)); // true
mySet.delete(2);
console.log(mySet.size); // 1
mySet.clear(); //delete all items
```
- ***Weak set***:A WeakSet can only store objects (not primitive values).
```javascript
const weakSet = new WeakSet();
const obj1 = { name: "Bob" };
const obj2 = { name: "Eve" };
weakSet.add(obj1);
console.log(weakSet.has(obj1)); // true
weakSet.delete(obj1);
console.log(weakSet.has(obj1)); // false
```

### Set vs Array
- ***Set***:
1. A Set only stores unique values. Duplicate values are automatically ignored.
2. Use the size property to get the number of elements in a Set.
3. No concept of index; you cannot access elements by index in a Set.
- ***Array***:
1. An Array can store duplicate values.
2. Use the length property to get the number of elements in an Array.
3. Elements can be accessed by their index (e.g., arr[0]).

| **Feature**       | **Rest Operator (`...`)**                    | **Spread Operator (`...`)**                 |
|--------------------|---------------------------------------------|---------------------------------------------|
| **Purpose**        | Combines items into a single collection.    | Expands items into individual elements.     |
| **Use Case**       | Function parameters, destructuring.         | Function calls, array/object literals.      |
| **Primary Context**| `function fn(...args) {}`                   | `const newArr = [...oldArr];`               |
| **Key Behavior**   | Gathers remaining values into an array or object. | Spreads values from arrays or objects.      |
| **Example: Function Parameters** | `function sum(...nums) {}`             | `Math.max(...nums);`                        |
| **Example: Array Destructuring** | `[first, ...rest] = [1, 2, 3];`        | `const combined = [...arr1, ...arr2];`      |
| **Example: Object** | Not applicable.                             | `const clone = { ...original };`            |

### Primitive Types: Pass-by-Value
When you pass a primitive type (e.g., string, number, boolean, etc.) to a function, JavaScript passes a copy of the value. Changes made to the parameter inside the function do not affect the original variable.
```javascript
let x = 10;
function modifyValue(y) {
  y = 20; // This changes the copy of x, not the original x
}
modifyValue(x);
console.log(x); // Output: 10 (x is unchanged)
```
### Non-Primitive Types: Pass-by-Reference-Like Behavior
For objects, arrays, and functions (non-primitive types), JavaScript passes a reference to the value in memory. This means the function gets access to the same object in memory, so modifying the object inside the function affects the original object.
However, if you reassign the parameter to a new object, the original object remains unchanged because the parameter now points to a new reference.

```javascript
let obj = { name: "John" };
function modifyObject(o) {
  o.name = "Doe"; // Modifies the original object
}
modifyObject(obj);
console.log(obj.name); // Output: "Doe" (the original object is changed)
```

## TypeScript
### Interface vs. Type
- **Interface**: In TypeScript, an interface is a structure that defines the shape of an object. It allows you to specify the types of properties and methods that an object should have, without providing the actual implementation.
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  description?: string; // Optional property
}
//Usage
const user: User = {
  id: 1,
  name: "John Doe",
  email: "john.doe@example.com",
};
console.log(user.name); // John Doe
```
- **Type**: More versatile, can define primitives, unions, intersections, tuples.

### Differences Between Interfaces and Types
- Interfaces can be extended using extends, while types use intersection (&)
```typescript
//Interface
interface Person {
  name: string;
  age: number;
}
interface Employee extends Person {
  employeeId: number;
}
const employee: Employee = {
  name: "Alice",
  age: 25,
  employeeId: 101,
};
console.log(employee); // { name: 'Alice', age: 25, employeeId: 101 }
```
```typescript
type Person = {
  name: string;
  age: number;
};
type Employee = Person & {
  employeeId: number;
};
const employee: Employee = {
  name: "Bob",
  age: 30,
  employeeId: 102,
};
console.log(employee); // { name: 'Bob', age: 30, employeeId: 102 }
```
- Interfaces allow declaration merging (multiple declarations of the same interface are merged automatically), but types do not.
```typescript
//Interface
interface User {
  id: number;
}
interface User {
  name: string;
}
const user: User = {
  id: 1,
  name: "Charlie",
};
console.log(user); // { id: 1, name: 'Charlie' }
```
```typescript
//Type
type User = {
  id: number;
};
// Error: Duplicate identifier 'User'.
// type User = {
//   name: string;
// };
```
- type can be used for unions, tuples, and primitives, whereas interface is specifically for defining object shapes.
```typescript
//Type
type Status = "active" | "inactive" | "pending"; //Union types with multiple values
const currentStatus: Status = "active";
type Point = [number, number];
const coordinates: Point = [10, 20]; //A Tuple is a fixed-size array where each element has a specific type. Unlike regular arrays, tuples define the type of each element at specific positions.
console.log(currentStatus); // active
console.log(coordinates);   // [10, 20]
```

### Getter & Setter methods in TS
```typescript
class Person {
  private _name: string = "";
  // Getter
  get name(): string {
    return this._name;
  }
  // Setter
  set name(newName: string) {
    this._name = newName;
  }
}
const person = new Person();
person.name = "Alice"; // Calls the setter
console.log(person.name); // Calls the getter -> "Alice"
// person.name = "Al"; // Error: Name must be at least 3 characters long
```
### How to implement class constants in TypeScript?
In TypeScript, you can implement class constants by using the static keyword. Static properties or constants belong to the class itself rather than to an instance of the class.
```typescript
class MathConstants {
  static readonly PI = 3.14159; // Class constant
  static readonly E = 2.71828;  // Class constant

  static getPiSquared(): number {
    return this.PI * this.PI; // Access static constant within the class
  }
}
// Accessing constants
console.log(MathConstants.PI); // 3.14159
console.log(MathConstants.E);  // 2.71828
// Using the static method
console.log(MathConstants.getPiSquared()); // 9.869587728099999
```
### TypeScript Benefits in React Native
- Static typing reduces runtime errors.
- Refactoring is safer and consistent.
- Enhanced autocompletion and documentation support in IDEs.

### What is the difference between any, unknown, and never types in TypeScript?
- **any**: The any type is the most permissive type in TypeScript. It effectively disables type checking, allowing any value to be assigned to a variable, and it can be used anywhere.
- **unknown**: The unknown type is a safer alternative to any. While unknown can hold any type of value (like any), it requires explicit type checking**(type narrowing)** before performing operations on the value. This ensures that you validate the type before interacting with it.
```typescript
let value: unknown = 42;
value = "string"; // No error, can change type freely

if (typeof value === "string") {
  console.log(value.toUpperCase()); // No error, because the type is checked
} else {
  console.log(value.toUpperCase()); // Error: Property 'toUpperCase' does not exist on type 'unknown'
}
```
- **never**: he never type represents a value that never occurs. It is used for functions that will never return a value (e.g., functions that throw an error or enter an infinite loop).
```typescript
function throwError(message: string): never {
  throw new Error(message); // This function never returns
}

function infiniteLoop(): never {
  while (true) {} // Infinite loop, never ends
}
```
### How do you define and use enums in TypeScript?
```typescript
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right  // 3
}
let move: Direction = Direction.Up;
console.log(move);  // Output: 0
```
### readonly in ts
- In TypeScript, the readonly modifier is used to mark properties of objects or array elements as immutable, meaning their values cannot be changed after they are initialized.
```typescript
interface Person {
  readonly name: string;
  readonly age: number;
}

const person: Person = { name: "John", age: 30 };

// Trying to modify the readonly properties will result in a compile-time error
person.name = "Alice";  // Error: Cannot assign to 'name' because it is a read-only property.
person.age = 35;        // Error: Cannot assign to 'age' because it is a read-only property.
```
### How can you make all properties of an interface optional?
```typescript
interface Person {
  name: string;
  age: number;
  address: string;
}

const person: Partial<Person> = {
  name: "John" // Only one property is required
};
```
### What is the as keyword used for in TypeScript?
- The as keyword in TypeScript is used for type assertions. It allows you to tell the TypeScript compiler to treat a value as a specific type, overriding its inferred or declared type.
```typescript
let someValue: any = "Hello, TypeScript!";
// Assert that `someValue` is a string
let strLength: number = (someValue as string).length;
console.log(strLength);  // Output: 18
```

### What are intersection types, and how do they differ from inheritance?
- An intersection type in TypeScript allows you to combine multiple types into one. A value of an intersection type will have all the properties and methods of each of the types involved in the intersection. It’s like saying “a value must satisfy all of these types.”
```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee {
  jobTitle: string;
  company: string;
}

type EmployeeWithDetails = Person & Employee;

const employee: EmployeeWithDetails = {
  name: "Alice",
  age: 30,
  jobTitle: "Software Developer",
  company: "Tech Corp",
};
```
| **Aspect**                 | **Intersection Types**                            | **Inheritance**                                    |
|----------------------------|---------------------------------------------------|---------------------------------------------------|
| **Purpose**                 | Combine multiple types into one type.            | Create a hierarchical relationship between classes or interfaces. |
| **Usage**                   | Used for combining the properties of different types. | Used to create subclasses that inherit properties and methods from a superclass. |
| **Composition**             | A type has **all** properties from multiple types. | A subclass inherits properties and methods from a superclass. |
| **Type System**             | Works at the type level, doesn't require classes. | Works with classes and objects, creating a class hierarchy. |
| **Relationship**            | No hierarchical relationship.                    | Models an "is-a" relationship (e.g., `Dog` is an `Animal`). |
| **Flexibility**             | Can combine any types, including objects, interfaces, and even primitives. | Tightly coupled with object-oriented programming and class structures. |

### What is a function overload in TypeScript, and how does it work?
```typescript
// Overload signatures
function calculate(a: number, b: number): number;
function calculate(a: string, b: string): string;
// Implementation signature
function calculate(a: number | string, b: number | string): number | string {
  if (typeof a === "number" && typeof b === "number") {
    return a + b;  // If both are numbers, return sum
  } else if (typeof a === "string" && typeof b === "string") {
    return a + " " + b;  // If both are strings, return concatenation
  }
  throw new Error("Invalid arguments");
}
console.log(calculate(5, 10));        // Output: 15
console.log(calculate("Hello", "World")); // Output: "Hello World"
```
### What is type narrowing, and how is it done in TypeScript?
Type narrowing in TypeScript is the process of refining the type of a variable within a specific scope or block of code
- Using typeof Operator
- Using instanceof Operator
```typescript
function printValue(value: string | number | boolean) {
  if (typeof value === "string") {
    // Narrowed to string
    console.log(`String value: ${value.toUpperCase()}`);
  } else if (typeof value === "number") {
    // Narrowed to number
    console.log(`Number value: ${value + 10}`);
  } else {
    // Narrowed to boolean
    console.log(`Boolean value: ${value ? "True" : "False"}`);
  }
}
// Test cases
printValue("hello"); // String value: HELLO
printValue(42);      // Number value: 52
printValue(true);    // Boolean value: True
```

## Event Propagation: Bubbling vs. Capturing

| Feature               | **Event Bubbling**                                   | **Event Capturing**                              |
|-----------------------|-----------------------------------------------------|-------------------------------------------------|
| **Definition**        | Event propagates from the target element upwards to its ancestors. | Event propagates from the root element down to the target element. |
| **Direction**         | Target → Parent → Ancestors → Document               | Document → Ancestors → Parent → Target          |
| **Default Behavior**  | Enabled by default for event listeners.              | Must explicitly enable by passing `true` as the third parameter in `addEventListener`. |
| **Use Case**          | Commonly used for event delegation, handling child events in the parent. | Useful when you need to intercept events before they reach the target. |
| **Stopping Propagation** | Use `event.stopPropagation()` to stop bubbling.    | Use `event.stopPropagation()` to stop capturing. |

### Example Code for Event Propagation

```javascript
// Event Bubbling (Default)
document.getElementById('child').addEventListener('click', () => {
    console.log('Child clicked!');
});
document.getElementById('parent').addEventListener('click', () => {
    console.log('Parent clicked!');
});

// Event Capturing
document.getElementById('child').addEventListener('click', () => {
    console.log('Child clicked during capturing!');
}, true); // 'true' enables capturing phase
document.getElementById('parent').addEventListener('click', () => {
    console.log('Parent clicked during capturing!');
}, true);
```
### Function Composition
Function Composition is the process of combining two or more functions to produce a new function or perform a series of operations. It allows you to build complex operations by combining simpler ones.
- Functions are executed from right to left (similar to mathematical composition).
```javascript
const add = (x) => x + 2;
const multiply = (x) => x * 3;
// Compose two functions
const compose = (f, g) => {
  return (x) => {
    return f(g(x));
  };
};
const addThenMultiply = compose(multiply, add);
console.log(addThenMultiply(5)); // (5 + 2) * 3 = 21
```
### Overriding hooks
```javascript
import { useState } from "react";

function useLoggedState(initialValue) {
  const [state, setState] = useState(initialValue);

  const setLoggedState = (value) => {
    console.log(`State changing from ${state} to ${value}`);
    setState(value);
  };

  return [state, setLoggedState];
}

// Usage
function Counter() {
  const [count, setCount] = useLoggedState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### Key Threads in React Native:
- Main/UI Thread
- JavaScript Thread

### MVC (Model-View-Controller)
- Model: Manages the data and business logic.
- View: Represents the UI and displays the data from the Model.
- Controller: Handles user input, processes it, and updates the Model and/or View.

### Array vs Tuple
# TypeScript: Tuples vs Arrays

| Feature          | Tuple                                        | Array                                        |
|------------------|----------------------------------------------|----------------------------------------------|
| **Length**       | Fixed length (predefined)                   | Variable length (dynamic size)               |
| **Element Type** | Can contain different types (heterogeneous)  | All elements must be of the same type (homogeneous) |
| **Access**       | Elements are accessed by index, types are enforced | Elements are accessed by index, types are the same |
| **Use Case**     | Used for fixed structures with mixed types, like records or key-value pairs | Used for collections of similar data (e.g., numbers, strings) |
| **Mutability**   | Elements can be reassigned, but the number of elements is fixed | Elements and size can be modified |
| **Syntax**       | `[type1, type2, ...]`                        | `type[]`                                     |

## Examples

### Tuple Example
```typescript
let person: [string, number] = ['John', 25];
// Correct usage
let coordinates: [number, number] = [40.7128, 74.0060];
// Incorrect usage: will throw an error
person = [30, 'Jane']; // Error: Type 'number' is not assignable to type 'string'
```
### How does TypeScript handle enums?
TypeScript handles enums by providing a way to define a set of named constants. Enums are a special type in TypeScript that allows you to assign names to numeric or string values.
```typescript
enum CommonExample {
  Up = 1,
  Active = 'ACTIVE',
  Left,
  Right
}
console.log(CommonExample.Up); // 1
console.log(CommonExample.Active); // "ACTIVE"
console.log(CommonExample.Left); // 2
console.log(CommonExample.Right); // 3
```

### Scenarios
- JS Thread blocking
**App Startup:**

The app initializes and renders the FlatList with the initial dataset (data state with 20 items).
The FlatList is responsive and scrolls smoothly because both the UI thread and JavaScript thread are working as expected.

**Pressing the "Block JS Thread" Button:**

When the "Block JS Thread" button is pressed, the blockJavaScriptThread function runs a synchronous while loop that blocks the JavaScript thread for 3 seconds.

*Key Issue:*
The JavaScript thread is now completely occupied by the while loop and cannot process any other tasks (e.g., handling state updates, triggering callbacks, or sending instructions to the UI thread).

**Attempting to Scroll the FlatList During Blocking:**

The UI thread detects the scroll gesture, but it needs to interact with the JavaScript thread to:
Process the scroll event logic.
Trigger the onEndReached callback to load more data when the user scrolls near the end of the list.

*Problem:*

Since the JavaScript thread is busy with the blockJavaScriptThread function:
The FlatList's scroll gestures are delayed or freeze entirely.
The onEndReached callback cannot execute, so no new data is fetched.

**Impact on UI Updates:**

While the JavaScript thread is blocked, it cannot:
Process state updates (e.g., setLoading(true) in loadMoreData).
Send updates to the UI thread to render new items.
As a result, the app becomes unresponsive until the blocking operation is completed.

**JavaScript Thread is Freed After Blocking:**

Once the while loop in blockJavaScriptThread completes, the JavaScript thread becomes free to handle other tasks.
At this point:
Any pending scroll gestures or onEndReached events are processed.
The app returns to normal responsiveness.

**Triggering onEndReached to Load More Data:**

If the user scrolls again after the JavaScript thread is unblocked, the onEndReached callback is triggered.
New data is fetched, and the FlatList updates its state and renders the additional items.

# Understanding Time Complexity in JavaScript
### Constant Time - O(1)
The operation does not depend on the input size.
```javascript
function getFirstElement(arr) {
  return arr[0]; // Executes in constant time
}
```

### Linear Time – O(n)
The execution time grows directly proportional to the input size.
```javascript
function printArray(arr) {
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // Executes once for each element
  }
}
```

### Quadratic Time – O(n²)
Nested loops result in n * n iterations.
```javascript
function printPairs(arr) {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      console.log(arr[i], arr[j]); // Executes n * n times
    }
  }
}
```

### Logarithmic Time – O(log n)
The search space is halved with each iteration.
```javascript
function binarySearch(arr, target) {
  let start = 0, end = arr.length - 1;
  while (start <= end) {
    let mid = Math.floor((start + end) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) start = mid + 1;
    else end = mid - 1;
  }
  return -1;
}
```
### Exponential Time – O(2ⁿ)
The execution time doubles with each additional input.
```javascript
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2); // Recursively calls itself twice
}
```

### Graph QL
**Graph Q: v/s REST**
- Graph QL fixes the problem of Over-fetching/Under-fetching, by returning only those data that was provided when executing a query.
- Graph QL provides the option to get different type of data from a single table as compared to REST, that might require multiple apis to be called for that same data.
    - One exapmle can be fetching User data & User posts data using same query
- Graph QL allows Easier Aggregation of Related Data, this can be understood with this example:
    - Suppose we need to fetch the User post that requires user id, in REST this would reqire us to first call User details api to get **USER ID**, then call User post api with the previously received ID. In Graph QL we can simply specify the query to fetch all the relevent details in single API.
- Graph QL allows Better Handling of Versioning, REST api would require versioning.
- Graph QL provides Real-Time Data with Subscriptions, REST does not have any such in built function.
```graphql
subscription {
  commentAdded(postId: 1) {
    text
    author {
      name
    }
  }
}
```

**How do you make a GraphQL request from a React Native or React app?**
- Using fetch
```graphql
const response = await fetch('https://api.example.com/graphql', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ query }),
  });
```
- Using Apollo Client
```graphql
import { ApolloClient, InMemoryCache, ApolloProvider } from '@apollo/client';
const client = new ApolloClient({
  uri: 'https://api.example.com/graphql',
  cache: new InMemoryCache(),
});
const App = () => (
  <ApolloProvider client={client}>
    {/* Other components */}
  </ApolloProvider>
);

// Querying data
const { loading, error, data } = useQuery(GET_USER);
// Mutations
const [addUser, { data, loading, error }] = useMutation(ADD_USER);
// Subscriptions
const { data, loading } = useSubscription(COMMENT_ADDED);
```

**What is a GraphQL fragment, and how can it help reduce redundancy in frontend code?**
- Define fragment
```graphql
// Declare common fragement
const USER_FIELDS = gql`
  fragment UserFields on User {
    // Here on User means UserFields is a fragment of User schema, this schema will be defined by backend
    id
    name
    email
    profilePicture
  }
`;

// Use fragment
const GET_USER = gql`
  query {
    user(id: 1) {
      ...UserFields
    }
  }
  ${USER_FIELDS} // Include the fragment
`;
```
**Cache Policies in Apollo Client**
- **cache-first (default)**: Return data from the cache if available, otherwise, fetch from the network.
- **cache-and-network**: Return data from the cache first, but also fetch the latest data from the network.
- **network-only**: Always fetch the data from the network and ignore the cache.
- **no-cache**: Do not store the result in the cache.
- **cache-only**: Only return data from the cache, and never fetch from the network.

```graphql
// Initialize Apollo Client with cache configuration
const client = new ApolloClient({
  uri: 'https://your-graphql-api.com/graphql',
  cache: new InMemoryCache(),  // Enable the normalized cache
});

const { data, loading, error } = useQuery(GET_USERS, {
  fetchPolicy: 'cache-and-network', // Return cached data first, but also fetch from network
});
```

**Realtime update in Grph QL**
```graphql
// Create WebSocket link for subscription
const wsLink = new WebSocketLink({
  uri: 'wss://your-graphql-api.com/graphql', // WebSocket URL for your server
  options: {
    reconnect: true, // Reconnect if connection is lost
  },
});

// Create Apollo Client with WebSocket link and InMemoryCache
const client = new ApolloClient({
  link: wsLink,
  cache: new InMemoryCache(),
});
// Usage with cleanup
const { data, loading, error, unsubscribe } = useSubscription(NEW_MESSAGE_SUBSCRIPTION);

```


### IBM
- Maximum storage that html5 provides is **5 MB**
- **<mark>** is used to hightlight text in HTML
- In a form, **event.preventDefault()** is commonly used to prevent the form from submitting when the user clicks the submit button or presses Enter.
- **<span>** is not a semantic element
- The correct CSS selector that selects all elements of type E that have the attribute attr that ends with a given value is **E[attr$="value"]**
- The function that converts an element's color to a shade of gray for use by the filter property in CSS is **grayscale()**
- Which of the following media query describe output style portrait if height is greater than or equal to width, landscape if the opposite? **orientation**
- The correct syntax to redirect a URL using JavaScript is **window.location.href = 'https://www.example.com';**

### Resource Links
https://www.interviewbit.com/javascript-interview-questions/#different-methods-can-you-make-an-object
https://github.com/lydiahallie/javascript-questions

### Remote Job Companies
1. https://www.linkedin.com/company/aexonic-technologies/posts/?feedView=all
2. https://www.linkedin.com/company/metafic/
3. https://www.linkedin.com/company/globallogic/life/e41c9913-a1fc-4b0b-b080-3691f3ff721d/
4. https://www.linkedin.com/company/quantum-sharq-innovative-solutions/
5. https://www.linkedin.com/company/cwc-mohali/
6. https://www.linkedin.com/company/porch/life/a1521592-3b8b-4397-9541-5b2a319eade0/
7. https://www.linkedin.com/company/deltaclass-technology-solutions-limited/
8. https://www.linkedin.com/company/thomson-reuters/life/india/
9.  https://www.linkedin.com/company/3pillar/life/29c30451-4deb-4a8e-bd49-0628a7b9100f/

### Onsite Job Companies
1. https://www.linkedin.com/company/kpmg/posts/?feedView=all
2. https://www.linkedin.com/company/globallogic/life/e41c9913-a1fc-4b0b-b080-3691f3ff721d/
3. https://www.linkedin.com/company/okta-inc-/life/careers/
4. https://www.linkedin.com/company/ouroglobal/
5. https://www.linkedin.com/company/nooncom/jobs/ (https://gist.github.com/ahmedam55)
6. https://www.linkedin.com/company/intellecthq/?originalSubdomain=sg
7. https://www.linkedin.com/company/metlife/life/
---

This README consolidates foundational and advanced JavaScript and React concepts for developers.

