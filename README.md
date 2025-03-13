# React

## why React
React.js is a JavaScript library for building user interfaces (UIs), primarily for single-page applications (SPAs). React allows developers to create reusable UI components and efficiently manage the state of web applications.

- ### Component-Based Architecture
React applications are built using reusable components, making development faster and more maintainable.

- ### Virtual DOM for Performance Optimization
React uses a Virtual DOM to efficiently update and render only the components that need changes, improving performance.

- ### Unidirectional Data Flow (One-Way Data Binding)
Data flows in one direction, making debugging easier and ensuring predictable application behavior

- ### State Management
React provides built-in state management (useState, useReducer) and integrates with tools like Redux, Zustand, and Recoil for complex state management.

- ### Hooks for Functional Components

Hooks like useEffect, useMemo, and useCallback allow functional components to handle side effects, optimize performance, and manage state efficiently.

## What is JSx

JSX is a syntax extension of JavaScript that allows writing HTML-like code inside JavaScript.
It makes UI development more intuitive and readable.
JSX is compiled into JavaScript by Babel.
JSX Example
```jsx
const element = <h1>Hello, React!</h1>;
```
JavaScript Equivalent:

```jsx
const element = React.createElement('h1', null, 'Hello, React!');
```

## What is Props

Props allow data to be passed from a parent component to a child component.
They are read-only (immutable).
Useful for dynamic content.

```JSX
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Usage
<Welcome name="John" />
```

## What is State

State is data that changes over time and determines how a component behaves.
Unlike props, state is mutable.
Used for dynamic updates like form inputs, user interactions, etc.

```JSX
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

## What is the Virtual DOM?
The Virtual DOM (VDOM) is a lightweight copy of the real DOM that React uses to optimize performance and update the UI efficiently.
### How Does the Virtual DOM Work? 
1️⃣ React creates a Virtual DOM (a JavaScript object that represents the real DOM).

2️⃣ When state changes, React updates the Virtual DOM instead of the real DOM directly.

3️⃣ React compares (diffs) the new Virtual DOM with the old one to find what actually changed.

4️⃣ React updates only the changed parts in the real DOM (not the entire page).

React uses a "Reconciliation" process to efficiently update the Virtual DOM. The core of this process is based on a diffing algorithm called the "Fiber Reconciliation Algorithm"

### How Does the Fiber Reconciliation Algorithm Work?
The algorithm follows these steps:

1️⃣ Creates a Virtual DOM tree

React first creates a Virtual DOM tree, which is a JavaScript representation of the actual DOM.

2️⃣ Compares (diffs) the new Virtual DOM with the previous one

React uses efficient heuristics to determine what has changed.
Instead of comparing the entire tree node-by-node, it assumes:
Elements with different types (e.g., <div> vs. <span>) are completely replaced.
Elements with the same type are updated in place.

3️⃣ Uses "Keys" to Optimize Lists When dealing with lists (```<ul> or <table>```), React uses unique keys to efficiently track items.
If keys are missing, React re-renders the entire list, which can be slow.




# **Complete Guide to React Hooks**  

React hooks let you use **state** and **lifecycle methods** in functional components, making them more powerful and easier to maintain.  

---

## **Basic Hooks**  

### **1. `useState` – Manages Component State**  
Stores and updates **local state** in a functional component.  

**Syntax:**  
```jsx
const [state, setState] = useState(initialValue);
```
**Example: Counter Component**  
```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**When to use?**  
- Managing **component-specific** data (like form inputs, counters).  
- When state **doesn’t need to be shared globally**.  

---

### **2. `useEffect` – Handles Side Effects (API Calls, Event Listeners, Timers)**  
Runs side effects **after rendering**.  

**Syntax:**  
```jsx
useEffect(() => {
  // Side effect logic
  return () => {
    // Cleanup (optional)
  };
}, [dependencies]);
```

**Example: Fetching API Data**  
```jsx
import { useEffect, useState } from "react";

function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => res.json())
      .then((data) => setUsers(data));
  }, []);

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

**When to use?**  
- Fetching data from APIs.  
- Adding/removing event listeners.  
- Setting timers/intervals.  

**Common Mistake:** Forgetting the dependency array (`[]`). This can lead to **infinite loops** if not handled correctly.  

---

### **3. `useContext` – Access Global State Without Prop Drilling**  
Provides **global state** to avoid passing props down multiple levels.  

**Example: Theme Context**  
```jsx
import { createContext, useContext } from "react";

const ThemeContext = createContext("light");

function ThemeComponent() {
  const theme = useContext(ThemeContext);
  return <p>Current theme: {theme}</p>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemeComponent />
    </ThemeContext.Provider>
  );
}
```

**When to use?**  
- When **state needs to be shared across multiple components**.  
- Managing **themes, authentication, language preferences**.  

---

## **Performance Optimization Hooks**  

### **4. `useMemo` – Optimizes Expensive Computations**  
**Prevents unnecessary recalculations of expensive functions.**  

```jsx
import { useMemo, useState } from "react";

function ExpensiveCalculation({ number }) {
  const squaredNumber = useMemo(() => {
    console.log("Calculating...");
    return number * number;
  }, [number]);

  return <p>Squared: {squaredNumber}</p>;
}
```

**When to use?**  
- Avoiding unnecessary re-renders for **heavy calculations**.  

---

### **5. `useCallback` – Memoizes Functions**  
**Prevents re-creating functions on every render.**  

```jsx
import { useCallback, useState } from "react";

function Child({ onClick }) {
  return <button onClick={onClick}>Click me</button>;
}

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Button clicked");
  }, []);

  return <Child onClick={handleClick} />;
}
```

**When to use?**  
- Passing functions to **child components** to prevent unnecessary re-renders.  

---

## **Summary of Hooks & Their Use Cases**  

| Hook | Use Case |
|------|---------|
| `useState` | Local state management |
| `useEffect` | Side effects (API calls, subscriptions) |
| `useContext` | Global state without prop drilling |
| `useMemo` | Memoizing expensive calculations |
| `useCallback` | Memoizing functions |
| `useRef` | Accessing DOM elements, storing mutable values |
| `useReducer` | Complex state management |
| `useLayoutEffect` | Synchronous DOM updates |
| `useImperativeHandle` | Custom ref handling |

## **React Life Cycle**
In functional components, React provides the useEffect hook to handle different lifecycle phases like mounting, updating, and unmounting.

- ###  Mounting (Component Creation)
This happens when the component first appears in the DOM.
 Ideal for: API calls, event listeners, setting up subscriptions.
```jsx
import { useEffect } from "react";

const MyComponent = () => {
  useEffect(() => {
    console.log("Component Mounted: Fetching data...");
    
    // Fetch data or perform any one-time action here

  }, []); // Empty dependency array → Runs only ONCE when the component mounts

  return <h1>Hello, World!</h1>;
};
```
### what happens here
- Runs only once when the component mounts
- The empty dependency array [] ensures it does NOT run on updates.

- ### Updating (Component Re-rendering)
Happens when state or props change.
Ideal for: Fetching new data when props/state change.

```jsx
import { useState, useEffect } from "react";

const MyComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Component Updated: Count is now", count);
  }, [count]); // Runs when `count` changes

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```
### what happens here
- useEffect runs every time count changes.
- This is similar to componentDidUpdate in class components.

### Unmounting (Component Removal)
Happens when the component is removed from the DOM.
Ideal for: Cleaning up event listeners, intervals, or subscriptions.

```jsx
import { useEffect } from "react";

const MyComponent = () => {
  useEffect(() => {
    console.log("Component Mounted");

    return () => {
      console.log("Component Unmounted: Cleanup here!");
    };
  }, []); // Runs once, cleanup runs when component unmounts

  return <h1>Hello</h1>;
};
```
### what happens here
- return () => {} inside useEffect acts as the cleanup function.
- It runs before the component is removed from the DOM.
- Used for removing event listeners or stopping intervals.

## **Summary of Hooks & Their Use Cases**  

| Lifecycle | Functional Component |
|------|---------|
| `Mounting` | ```useEffect(() => {...}, [])``` |
| `Updating` | ```useEffect(() => {...}, [dependencies])``` |
| `Unmounting` | ```useEffect(() => { return () => {...} }, [])``` |


## Better Example Of Unmounting

```jsx
import { useState, useEffect, useCallback } from "react";

const TimerComponent = () => {
  const [time, setTime] = useState(0);
  const [isRunning, setIsRunning] = useState(false);

  useEffect(() => {
    let interval;

    if (isRunning) {
      interval = setInterval(() => {
        setTime((prevTime) => prevTime + 1);
      }, 1000);
    }

    return () => clearInterval(interval); // Cleanup on unmount or stop
  }, [isRunning]); // Runs effect when `isRunning` changes

  const startTimer = useCallback(() => {
    setIsRunning(true);
  }, []);

  const stopTimer = useCallback(() => {
    setIsRunning(false);
  }, []);

  const resetTimer = useCallback(() => {
    setIsRunning(false);
    setTime(0);
  }, []);

  return (
    <div style={{ textAlign: "center", padding: "20px" }}>
      <h2>Timer: {time} seconds</h2>
      <button onClick={startTimer} disabled={isRunning}>Start</button>
      <button onClick={stopTimer} disabled={!isRunning}>Stop</button>
      <button onClick={resetTimer}>Reset</button>
    </div>
  );
};

export default TimerComponent;
```

