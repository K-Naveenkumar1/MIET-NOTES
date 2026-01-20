
---

## 1. Difference between JavaScript and React

### JavaScript (JS)

- A **programming language**
    
- Used to add logic, events, and DOM manipulation to web pages
    
- You manually select and update HTML elements
    
- Example tools: `document.getElementById`, `innerHTML`, `addEventListener`
    

### React

- A **JavaScript library** (built using JavaScript)
    
- Used for building **user interfaces**
    
- Uses a **component-based architecture**
    
- Updates the UI automatically when **state changes**
    
- Uses **Virtual DOM** for efficient updates
    


---

## 2. Example 1: Updating Text on Button Click

###  Using JavaScript (`innerHTML`)

```html
<p id="text">Hello</p>
<button onclick="changeText()">Click</button>

<script>
function changeText() {
  document.getElementById("text").innerHTML = "Hello World";
}
</script>
```

🔹 **What’s happening**

- We manually find the element
    
- We manually update the DOM using `innerHTML`
    

---

###  Using React

```jsx
import { useState } from "react";

function App() {
  const [text, setText] = useState("Hello");

  return (
    <div>
      <p>{text}</p>
      <button onClick={() => setText("Hello World")}>
        Click
      </button>
    </div>
  );
}

export default App;
```

🔹 **What’s happening**

- UI is controlled by **state**
    
- No direct DOM manipulation
    
- React automatically updates the UI
    

---

## 3. Example 2: Adding Items to a List

### JavaScript (`innerHTML`)

```html
<ul id="list"></ul>
<button onclick="addItem()">Add Item</button>

<script>
function addItem() {
  const list = document.getElementById("list");
  list.innerHTML += "<li>New Item</li>";
}
</script>
```

Problems:

- Rebuilding HTML strings
    
- Easy to introduce bugs
    
- Hard to manage large apps
    

---

### React

```jsx
import { useState } from "react";

function App() {
  const [items, setItems] = useState([]);

  return (
    <div>
      <button onClick={() => setItems([...items, "New Item"])}>
        Add Item
      </button>

      <ul>
        {items.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}
```

✔ Clean  
✔ Scalable  
✔ UI stays in sync with data

---

## 4. Example 3: Form Input Handling

###  JavaScript

```html
<input id="name" type="text" />
<p id="output"></p>

<script>
document.getElementById("name").addEventListener("input", function () {
  document.getElementById("output").innerHTML = this.value;
});
</script>
```

---

### React

```jsx
import { useState } from "react";

function App() {
  const [name, setName] = useState("");

  return (
    <div>
      <input
        type="text"
        onChange={(e) => setName(e.target.value)}
      />
      <p>{name}</p>
    </div>
  );
}
```

---

## 5. Key Differences Summary Table

|Feature|JavaScript|React|
|---|---|---|
|Type|Language|Library|
|DOM updates|Manual (`innerHTML`)|Automatic|
|Code style|Imperative|Declarative|
|Reusability|Low|High (components)|
|Performance|Slower for large apps|Faster (Virtual DOM)|
|State handling|Manual|Built-in|

---

## 6. One-Line Interview Answer

> **JavaScript manipulates the DOM directly using methods like `innerHTML`, while React uses state and components to automatically update the UI in a more efficient and scalable way.**

---
### Async Await & Promises

---

## 1️ What is a Promise?

A **Promise** represents a value that will be available **later** (or fail).

It has **3 states**:

- `pending`
    
- `fulfilled`
    
- `rejected`
    

### Basic Promise example

```js
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Data loaded");
  }, 1000);
});

fetchData
  .then(result => console.log(result))
  .catch(error => console.error(error));
```

---

## `then()` and `catch()`
Here’s a ￼￼clear, practical explanation of ￼￼async￼￼, ￼￼await￼￼, and Promises in JavaScript￼￼, with ￼￼React-specific examples￼￼ 👇
Promises are handled using:

- `.then()` → success
    
- `.catch()` → error
    
- `.finally()` → always runs
    

```js
fetch("https://api.example.com/users")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error(err))
  .finally(() => console.log("Done"));
```

---

## `async` / `await` (Cleaner Way)

`async/await` is **syntactic sugar** over Promises.  
It makes async code look **synchronous**.

### Same example with async/await

```js
async function getUsers() {
  try {
    const response = await fetch("https://api.example.com/users");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

🔹 `async` → makes a function return a Promise  
🔹 `await` → pauses execution until the Promise resolves

---

##  Error Handling

### With Promises

```js
fetchData()
  .then(res => console.log(res))
  .catch(err => console.error(err));
```

### With async/await

```js
try {
  const res = await fetchData();
  console.log(res);
} catch (err) {
  console.error(err);
}
```

 `try/catch` is usually **cleaner**

---

## Async/Await in React (VERY IMPORTANT)

### Example: Fetching data in `useEffect`

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const res = await fetch("https://api.example.com/users");
        const data = await res.json();
        setUsers(data);
      } catch (error) {
        console.error(error);
      }
    }

    fetchUsers();
  }, []);

  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

 You **cannot** make `useEffect` itself async:

```js
// Wrong
useEffect(async () => {})
```

---

##  Handling Loading & Error State in React

```jsx
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  async function fetchData() {
    try {
      setLoading(true);
      const res = await fetch(url);
      const data = await res.json();
      setData(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  }

  fetchData();
}, []);
```

---

##  Multiple Promises (`Promise.all`)

```js
const [users, posts] = await Promise.all([
  fetch("/users").then(r => r.json()),
  fetch("/posts").then(r => r.json())
]);
```

 Runs in parallel → faster

---