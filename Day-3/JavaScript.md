
# JavaScript Basics (FOUNDATION)

##  What is JavaScript?

JavaScript is a **programming language** that:

- Makes websites **interactive**
    
- Runs **inside the browser**
    
- Also runs on the **server** (Node.js)
    
### Example (Without JS)

A button does nothing.

### Example (With JS)

A button:

- Shows message
    
- Sends data
    
- Changes color
    
- Calls an API
    
 **MERN uses JavaScript everywhere**

- React → frontend
    
- Node + Express → backend
    
- MongoDB → data handling
    

---

## How JavaScript Works (Simple)

JavaScript:

- Runs **line by line**
    
- Is **single-threaded**
    
- Executes **top to bottom**
    

```js
console.log("Hello");
console.log("World");
```

Output:

```
Hello
World
```

---

##  Variables (Very Important)

Variables store data.

### ❌ Bad (Old)

```js
var age = 25;
```

### Good (Modern)

```js
let age = 25;      // can change
const name = "Sam"; // cannot change
```

### Rules

- Use `const` by default
    
- Use `let` if value changes
    
- Never use `var`
    

---

## 4️⃣ Data Types (Beginner Friendly)

```js
const name = "Alex";   // string
const age = 25;       // number
const isAdmin = true; // boolean
let city;             // undefined
let salary = null;    // null
```

👉 In MERN:

- Data comes from forms
    
- Data goes to APIs
    
- Data stored in MongoDB
    

---

# Operators & Conditions (LOGIC BUILDING)

## 1️⃣ Operators (Explained Simply)

### Arithmetic

```js
10 + 5   // 15
10 - 5   // 5
10 * 5   // 50
10 / 5   // 2
```

### Comparison (VERY IMPORTANT)

```js
5 === 5     // true
5 === "5"   // false
```

👉 Always use `===` in MERN

### Logical

```js
true && false // false
true || false // true
!true         // false
```

---

## Conditions (Decision Making)

```js
if (age >= 18) {
  console.log("Allowed");
} else {
  console.log("Not allowed");
}
```

👉 Used in:

- Login systems
    
- Permissions
    
- Validation
    

---

##  Real MERN Example

```js
if (user && user.isAdmin) {
  access = "Granted";
}
```

---

#  Loops & Functions (CORE PROGRAMMING)

## Loops (Repeating Work)

### Example

```js
for (let i = 1; i <= 5; i++) {
  console.log(i);
}
```

Used in MERN:

- Display lists in React
    
- Loop API data
    
- Process MongoDB results
    

---

## Functions (MOST IMPORTANT)

Functions are reusable blocks of code.

```js
function greet() {
  console.log("Hello");
}
greet();
```

---

### Arrow Functions (Used in MERN)

```js
const greet = () => {
  console.log("Hello");
};
```

---

### With Parameters

```js
const add = (a, b) => {
  return a + b;
};
```

 Used in:

- Controllers
    
- React components
    
- Utilities
    

---

# Arrays & Objects (DATA HANDLING)

## Objects (Real-World Data)

```js
const user = {
  name: "Alex",
  email: "a@test.com",
  isAdmin: true
};
```

 MongoDB stores data like this

---

## Arrays (Multiple Data)

```js
const users = [
  { name: "A" },
  { name: "B" }
];
```

---

## Important Array Methods (🔥 MERN CORE)

### map()

```js
users.map(user => user.name);
```

### filter()

```js
users.filter(user => user.isAdmin);
```

### find()

```js
users.find(user => user.name === "A");
```

👉 Used in:

- React rendering
    
- API responses
    
- Backend logic
    

---

##  Destructuring (React MUST)

```js
const { name, email } = user;
```

---

# Asynchronous JavaScript (CRITICAL FOR MERN)

## What is Async JavaScript?

Some tasks take time:

- API calls
    
- Database queries
    
- File uploads

JavaScript **does not wait**

---

## setTimeout Example

```js
setTimeout(() => {
  console.log("Hello");
}, 2000);
```

---

## Promises (Easy Explanation)

A Promise means:

> “I will give you data later”

```js
fetch(url)
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.log(err));
```

---

## async / await (BEST PRACTICE)

```js
const getUsers = async () => {
  const res = await fetch(url);
  const data = await res.json();
  console.log(data);
};
```

 Used in:

- Express routes
    
- React `useEffect`
    
- MongoDB queries
    

---

#  DOM + Modern JavaScript

##  DOM (Before React)

```js
document.querySelector("button")
  .addEventListener("click", () => {
    alert("Clicked");
  });
```

 Helps understand how React works internally

---

## Spread Operator (VERY IMPORTANT)

```js
const newUser = { ...user, role: "admin" };
```

Used in:

- React state updates
    
- Redux
    
- MongoDB updates
    

---

##  Modules (React & Node)

```js
export default App;
import App from "./App";
```

---

# JavaScript in MERN (BIG PICTURE)

## JavaScript in Node.js

- No browser
    
- No DOM
    
- Server logic
    

---

##  Express Backend Example

```js
app.get("/users", async (req, res) => {
  const users = await User.find();
  res.json(users);
});
```

---

##  React Frontend Example

```js
useEffect(() => {
  fetchUsers();
}, []);
```

---

##  MERN Flow (VERY IMPORTANT)

1. React sends request
    
2. Express handles logic
    
3. MongoDB returns data
    
4. React updates UI
    