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

### Deep Copy
Creates a completely independent copy of the original object/array.
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
```
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
| Feature                | `React.memo`                  | `memo`                        | `useCallback`                 |
|------------------------|-------------------------------|--------------------------------|--------------------------------|
| **Purpose**            | Optimizes functional components by memoizing their rendered output to avoid unnecessary re-renders. | Shorthand for `React.memo` when importing React. | Caches a function definition to prevent re-creating it on every render. |
| **Type**               | Higher-Order Component (HOC)  | Same as `React.memo`           | Hook                           |
| **Usage**              | Wraps a functional component. | Same as `React.memo`.          | Wraps a callback function.     |
| **Arguments**          | `React.memo(Component, [compareFunction])` | `memo(Component, [compareFunction])` | `useCallback(callback, [dependencies])` |
| **Return Value**       | A memoized version of the component. | Same as `React.memo`.          | A memoized version of the callback function. |
| **Primary Use Case**   | Avoids re-rendering a component when its props haven't changed. | Same as `React.memo`.          | Prevents passing newly created functions to child components on every render. |
| **Dependencies**       | No dependencies required; checks props shallowly by default. | Same as `React.memo`.          | Relies on dependencies array to determine when to update the callback. |
| **Example**            | ```js<br>const MyComponent = React.memo(function MyComponent(props) {<br> return <Text>{props.name}</Text>;<br>});``` | ```js<br>import { memo } from 'react';<br>const MyComponent = memo((props) => {<br> return <Text>{props.name}</Text>;<br>});``` | ```js<br>const memoizedCallback = useCallback(() => {<br> doSomething();<br>}, [dependency]);``` |
| **When to Use**        | Use when components are re-rendering unnecessarily due to unchanged props. | Same as `React.memo`.          | Use when passing callbacks to child components that depend on stable references. |


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

### useMemo vs. useCallback
- **useMemo**: Memoizes a computed value.
- **useCallback**: Memoizes a function.

---

## Event Loop and Task Queue
### Microtasks vs. Macrotasks
- **Microtasks**: Promises, MutationObserver callbacks.
- **Macrotasks**: `setTimeout`, `setInterval`, DOM events.
- **Priority**: Microtasks > Macrotasks.

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
```typescript
function identity<T>(value: T): T {
    return value;
}
const numberValue = identity<number>(42);
const stringValue = identity<string>("Hello, Generics!");
```

---

## Additional Examples
### Swapping Variables Without Temp Variable
```javascript
let a = 5, b = 10;
[a, b] = [b, a];
```

### Event Loop Example
```javascript
console.log(1);
setTimeout(() => console.log(2), 0);
Promise.resolve().then(() => console.log(3));
console.log(4);
// Output: 1, 4, 3, 2
```

---

This README consolidates foundational and advanced JavaScript and React concepts for developers.

