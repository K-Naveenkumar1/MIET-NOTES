
# 1️. What is React?

### Definition

**React** is a **JavaScript library** used to build **interactive user interfaces**, especially **single-page applications**.

React focuses only on the **UI (View layer)**.

---

### Why React Was Created

Problems with normal JavaScript:

- Manual DOM manipulation
    
- UI bugs when data changes
    
- Difficult to scale large apps
    

React solves this by:

- Automatically updating the UI
    
- Using components
    
- Keeping UI in sync with data
    

---

### Core Idea (VERY IMPORTANT)

```
UI = function of state
```

When **state changes**, UI updates automatically.

---

# 2️. Setting Up a React App

### Create App

```bash
npx create-react-app my-app
cd my-app
npm start
```

---

### Folder Structure

```
src/
 ├── App.js
 ├── index.js
 └── components/
```

---

### index.js

```js
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

 React starts rendering from `App`

---

# 3️. JSX (JavaScript XML)

### What is JSX?

JSX allows us to write **HTML inside JavaScript**.

---

### Example

```jsx
const element = <h1>Hello React</h1>;
```

---

### JSX Rules

✅ One parent element  
✅ Use `className` instead of `class`  
✅ JavaScript inside `{}`

---

### Example

```jsx
function App() {
  return (
    <div>
      <h1>Hello</h1>
      <p>Welcome to React</p>
    </div>
  );
}
```

---

# 4️. Components

### What is a Component?

A **component** is a **function that returns JSX**.

---

### Example

```jsx
function Header() {
  return <h1>My Website</h1>;
}
```

---

### Using Component

```jsx
function App() {
  return (
    <div>
      <Header />
    </div>
  );
}
```

---

### Why Components?

- Reusable
    
- Readable
    
- Maintainable
    

---

# 5️. Props (Passing Data)

### What are Props?

Props are **data passed to components**, like function arguments.

---

### Example

```jsx
function Greeting(props) {
  return <h2>Hello {props.name}</h2>;
}
```

---

### Using Props

```jsx
function App() {
  return (
    <div>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </div>
  );
}
```


---

### Props with Destructuring

```jsx
function Greeting({ name }) {
  return <h2>Hello {name}</h2>;
}
```

---

# 6️. State (useState Hook)

### What is State?

State is **data that changes over time**.

---

### useState Syntax

```js
const [value, setValue] = useState(initialValue);
```

---

### Counter Example

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </div>
  );
}
```

---

### ❌ Wrong Way (Never Do This)

```js
count = count + 1;
```

State must be updated using `setCount`.

---

# 7️. Event Handling

### React Events

React uses **camelCase events**.

---

### Example

```jsx
function ClickMe() {
  function handleClick() {
    alert("Button clicked!");
  }

  return <button onClick={handleClick}>Click</button>;
}
```

---

# 8️. Conditional Rendering

### Example

```jsx
function LoginStatus() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      {isLoggedIn ? <h2>Welcome</h2> : <h2>Please Login</h2>}
      <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
        Toggle
      </button>
    </div>
  );
}
```

---

# 9️. Lists & Keys

### Rendering Lists

```jsx
function FruitList() {
  const fruits = ["Apple", "Banana", "Orange"];

  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```

---

### Why Keys?

Keys help React **identify elements uniquely**.

---

# 10. Forms & Controlled Inputs

### Controlled Input

```jsx
function NameForm() {
  const [name, setName] = useState("");

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <p>{name}</p>
    </div>
  );
}
```

---

# 1️1️. useEffect (Side Effects)

### What is useEffect?

Used for:

- API calls
    
- Timers
    
- DOM updates
    
- Side effects
    

---

### Example (Component Mount)

```jsx
import { useEffect } from "react";

useEffect(() => {
  console.log("Component mounted");
}, []);
```


---

### Example (State Change)

```jsx
useEffect(() => {
  document.title = count;
}, [count]);
```


---

# 1️2️. Lifting State Up

### Problem

Two components need the same data.

---

### Solution

Move state to **common parent**.

---

### Example

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <Display count={count} />
      <Button setCount={setCount} count={count} />
    </>
  );
}
```

---

# 1️3️. Mini Project: Todo App

### Code

```jsx
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [task, setTask] = useState("");

  function addTodo() {
    setTodos([...todos, task]);
    setTask("");
  }

  return (
    <div>
      <input
        value={task}
        onChange={(e) => setTask(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>

      <ul>
        {todos.map((t, i) => (
          <li key={i}>{t}</li>
        ))}
      </ul>
    </div>
  );
}
```

### ✅ Output

Typing `Study React` and clicking Add:

```
• Study React
```
