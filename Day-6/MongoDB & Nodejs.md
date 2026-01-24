
## 1️. NoSQL Basics

**NoSQL** databases store data in flexible formats (not fixed tables like SQL).

**Key ideas:**

- Schema-flexible (fields can vary per record)
    
- Designed for scalability & performance
    
- Common types: document, key-value, graph, column-based
    

Most popular NoSQL DB for web apps: **MongoDB**

---

## 2️. Collections & Documents

In MongoDB (document-based NoSQL):

- **Database** → contains collections
    
- **Collection** → group of documents (like a table)
    
- **Document** → single record stored as JSON-like data (BSON)
    

**Example document:**

```json
{
  "name": "Alex",
  "email": "alex@example.com",
  "age": 25
}
```

 Documents in the same collection **don’t need identical fields**

---

## 3️. CRUD Operations

CRUD = the four basic database actions:

| Operation  | Meaning     | MongoDB Example       |
| ---------- | ----------- | --------------------- |
| **Create** | Insert data | `insertOne()`         |
| **Read**   | Fetch data  | `find()`, `findOne()` |
| **Update** | Modify data | `updateOne()`         |
| **Delete** | Remove data | `deleteOne()`         |

**Example (MongoDB):**

```js
db.users.insertOne({ name: "Alex", age: 25 })
db.users.find({ age: 25 })
db.users.updateOne({ name: "Alex" }, { $set: { age: 26 } })
db.users.deleteOne({ name: "Alex" })
```

---

## 4️. Mongoose Schemas

**Mongoose** is an ODM (Object Data Modeling) library for Node.js that adds structure to MongoDB.

**Why use Mongoose?**

- Enforces schemas
    
- Built-in validation
    
- Cleaner code with models
    

**Schema Example:**

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true },
  age: Number
});

module.exports = mongoose.model("User", userSchema);
```


```js
User.create({ name: "Alex", email: "alex@example.com" });
```

---

## 1. What is Node.js

**Node.js** is a JavaScript runtime that allows you to run JavaScript **outside the browser**, mainly on the server.

It is built on:

- **V8 Engine** (from Google Chrome)
    
- Event-driven, non-blocking I/O model
    

Used for:

- APIs
    
- Real-time apps
    
- Backend services
    

---

## 2. Node.js Architecture (High Level)

Node.js works on a **single-threaded, asynchronous** model.

Key components:

- Event Loop
    
- Callback Queue
    
- Non-blocking APIs
    

This allows Node.js to handle many requests efficiently.

---

## 3. Modules in Node.js

Node.js uses a **module system** to organize code.

### Types of Modules

1. Built-in modules
    
2. User-defined modules
    
3. Third-party modules (npm)
    

---

### Example: Built-in Module (fs)

```js
const fs = require("fs");

fs.writeFileSync("data.txt", "Hello Node.js");
```

This writes data to a file using Node.js.

---

## 4. Creating a Simple Server (http module)

```js
const http = require("http");

const server = http.createServer((req, res) => {
  res.write("Hello from Node.js Server");
  res.end();
});

server.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

This shows:

- How Node.js handles HTTP requests
    
- Server runs without a browser
    

---

## 5. Asynchronous Programming

Node.js uses **non-blocking asynchronous execution**.

### Synchronous Example

```js
const fs = require("fs");

const data = fs.readFileSync("data.txt");
console.log(data.toString());
```

Blocks execution until file is read.

---

### Asynchronous Example

```js
const fs = require("fs");

fs.readFile("data.txt", (err, data) => {
  if (err) throw err;
  console.log(data.toString());
});

console.log("Reading file...");
```

Node.js continues execution while file is being read.

---

## 6. Event Loop (Core Concept)

The **event loop** handles:

- Timers
    
- I/O callbacks
    
- Promises
    
- Async operations
    

Example:

```js
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
```

Output:

```
Start
End
Timeout
```

This demonstrates non-blocking behavior.

---

## 7. NPM (Node Package Manager)

Used to install and manage packages.

Commands:

```
npm init
npm install express
```

---

## 8. Express.js Basics (Minimal Example)

Express is a framework built on Node.js to simplify server creation.

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello Express");
});

app.listen(3000, () => {
  console.log("Server started");
});
```

---

## 9. Handling JSON and Middleware

```js
app.use(express.json());

app.post("/user", (req, res) => {
  res.json(req.body);
});
```

Middleware processes requests before sending responses.

---

## 10. Real-World Mini Example (API Logic)

```js
app.get("/users", (req, res) => {
  const users = [
    { name: "Alex", age: 25 },
    { name: "Sam", age: 30 }
  ];
  res.json(users);
});
```


This is the **standard REST API approach**.

---

## Project Structure

```
project/
│
├── config/
│   └── db.js
│
├── db/
│   └── user.js
│
├── src/
│   └── server.js
│
├── package.json
```

---

## 1. MongoDB Configuration

### `config/db.js`

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect("mongodb://127.0.0.1:27017/crudDB");
    console.log("MongoDB connected");
  } catch (error) {
    console.error("MongoDB connection failed", error);
  }
};

module.exports = connectDB;
```

Uses **MongoDB** connection.

---

## 2. User Schema

### `db/user.js`

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  age: Number
});

const User = mongoose.model("User", userSchema);

module.exports = User;
```

Uses **Mongoose**.

---

## 3. CRUD Using HTTP Methods

### `src/server.js`

```js
const express = require("express");
const connectDB = require("../config/db");
const User = require("../db/user");

const app = express();

// middleware
app.use(express.json());

app.post("/users", async (req, res) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

app.get("/users", async (req, res) => {
  const users = await User.find();
  res.json(users);
});


app.put("/users/:id", async (req, res) => {
  const updatedUser = await User.findByIdAndUpdate(
    req.params.id,
    req.body,
    { new: true }
  );
  res.json(updatedUser);
});


app.delete("/users/:id", async (req, res) => {
  await User.findByIdAndDelete(req.params.id);
  res.json({ message: "User deleted" });
});

// start server
app.listen(3000, () => {
  // connect database
  connectDB();
  console.log("Server running on port 3000");
});
```

---

## How to Test (Very Important)

Use **Postman  / curl**

### POST (Create User)

```
POST http://localhost:3000/users
```

Body:

```json
{
  "name": "Alex",
  "age": 25
}
```

---

### GET (Read Users)

```
GET http://localhost:3000/users
```

---

### PUT (Update User)

```
PUT http://localhost:3000/users/<USER_ID>
```

Body:

```json
{
  "age": 26
}
```

---

### DELETE (Remove User)

```
DELETE http://localhost:3000/users/<USER_ID>
```

---

## CRUD Mapping

|HTTP Method|CRUD|Description|
|---|---|---|
|POST|Create|Insert new document|
|GET|Read|Fetch documents|
|PUT|Update|Modify document|
|DELETE|Delete|Remove document|

---
