# MERN Stack Blog API - Backend Documentation

## Table of Contents
1. Project Overview
2. Folder Structure
3. Environment Configuration
4. File-by-File Implementation
5. API Endpoints Summary
6. Data Flow Diagram

---

## 1. Project Overview

### What is this Backend?
This is a **RESTful API** that powers a blog application. It handles:
- **User Authentication** (Signup/Login with JWT tokens)
- **Blog CRUD Operations** (Create, Read, Update, Delete posts)
- **Authorization** (Only authors can edit/delete their own posts)

### Technologies Used
| Technology | Purpose |
|------------|---------|
| **Node.js** | JavaScript runtime for server |
| **Express.js** | Web framework for routing and middleware |
| **MongoDB** | NoSQL database for storing data |
| **Mongoose** | ODM (Object Data Modeling) for MongoDB |
| **JWT** | JSON Web Tokens for authentication |
| **bcryptjs** | Password hashing for security |
| **cors** | Cross-Origin Resource Sharing |
| **dotenv** | Environment variable management |

---

## 2. Folder Structure

```text
backend/
├── .env                          # 🔐 Environment secrets (NEVER commit to Git)
├── package.json                  # 📦 Project dependencies and scripts
├── package-lock.json             # 🔒 Locked dependency versions
│
└── src/                          # 📁 Source code directory
    │
    ├── server.js                 # 🚀 ENTRY POINT - Starts DB & Server
    ├── app.js                    # ⚙️  EXPRESS CONFIG - Middleware & Routes
    │
    ├── models/                   # 📊 DATABASE SCHEMAS
    │   ├── User.js               #    - User schema (username, email, password)
    │   └── Blog.js               #    - Blog schema (title, content, author)
    │
    ├── controllers/              # 🧠 BUSINESS LOGIC
    │   ├── authController.js     #    - Signup & Login functions
    │   └── blogController.js     #    - CRUD operations for blogs
    │
    ├── middleware/               # 🛡️  SECURITY LAYER
    │   └── authMiddleware.js     #    - JWT verification
    │
    └── routes/                   # 🗺️  URL ENDPOINTS
        ├── auth.js               #    - /api/signup, /api/login
        └── blogs.js              #    - /api/blogs/*
```

### Visual Architecture
```text
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT REQUEST                           │
│                    (Frontend / Postman)                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         server.js                               │
│              (Connects to MongoDB, starts Express)              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                          app.js                                 │
│           (CORS, JSON parsing, Route mounting)                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         routes/                                 │
│              (Maps URLs to Controller functions)                │
└─────────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
         ┌──────────────────┐  ┌──────────────────┐
         │   middleware/    │  │   controllers/   │
         │ (Auth checking)  │  │ (Business logic) │
         └──────────────────┘  └──────────────────┘
                                        │
                                        ▼
                          ┌──────────────────────┐
                          │       models/        │
                          │  (Database schemas)  │
                          └──────────────────────┘
                                        │
                                        ▼
                          ┌──────────────────────┐
                          │       MongoDB        │
                          │     (Database)       │
                          └──────────────────────┘
```

---

## 3. Environment Configuration

### File: `.env`
```properties
# Server Port
PORT=5000

# MongoDB Connection String
MONGODB_URI=mongodb://localhost:27017/mern_blog_dev

# JWT Secret Key (used to sign tokens)
JWT_SECRET=your_super_secret_key_here_make_it_long_and_random
```

### Why Use `.env`?
| Reason | Explanation |
|--------|-------------|
| **Security** | Keeps passwords/secrets out of code |
| **Flexibility** | Different values for dev/production |
| **Best Practice** | Industry standard for configuration |

> ⚠️ **Important:** Add `.env` to your `.gitignore` file!

---

## 4. File-by-File Implementation

---

### 4.1 Entry Point: server.js

**Purpose:** This is where everything starts. It connects to the database and launches the server.

**File:** server.js
```javascript
// Load environment variables from .env file
require("dotenv").config();

// Import mongoose for database connection
const mongoose = require("mongoose");

// Import our configured Express app
const app = require("./app");

// Get values from environment or use defaults
const PORT = process.env.PORT || 5000;
const MONGODB_URI =
  process.env.MONGODB_URI || "mongodb://localhost:27017/mern_blog_dev";

// Connect to MongoDB first, THEN start the server
mongoose
  .connect(MONGODB_URI)
  .then(() => {
    console.log("Connected to mongodb");
    // Only start listening after DB is connected
    app.listen(PORT, () => console.log(`Server listening on ${PORT}`));
  })
  .catch((err) => {
    console.error("DB connection error", err);
    process.exit(1); // Exit with error code
  });
```

**Key Concepts to Explain:**
1. **`require("dotenv").config()`** - Loads `.env` file into `process.env`
2. **Promise chain** - `.then()` runs after successful connection
3. **Error handling** - `.catch()` handles connection failures
4. **`process.exit(1)`** - Stops the application if DB fails

---

### 4.2 Express Configuration: `app.js`

**Purpose:** Configures Express middleware and mounts all routes.

**File:** app.js
```javascript
const express = require("express");
const cors = require("cors");

// Import route files
const authRoutes = require("./routes/auth");
const blogRoutes = require("./routes/blogs");

// Create Express application
const app = express();

// ============== MIDDLEWARE ==============
// Middleware runs on EVERY request before reaching routes

// Enable CORS (Cross-Origin Resource Sharing)
// Allows frontend (port 5173) to communicate with backend (port 5000)
app.use(cors());

// Parse JSON request bodies
// Without this, req.body would be undefined
app.use(express.json());

// ============== ROUTES ==============
// Mount route handlers at specific paths

// Auth routes: /api/signup, /api/login
app.use("/api", authRoutes);

// Blog routes: /api/blogs, /api/blogs/:id
app.use("/api/blogs", blogRoutes);

// Root route for testing
app.get("/", (req, res) => {
  res.send("MERN Blog API is running!");
});

// Export the configured app
module.exports = app;
```

**Key Concepts to Explain:**
| Middleware | Purpose |
|------------|---------|
| `cors()` | Allows cross-origin requests (Frontend → Backend) |
| `express.json()` | Parses JSON in request body to `req.body` |
| `app.use("/api", routes)` | Mounts routes at a specific path |

---

### 4.3 Database Models

#### 4.3.1 User Model: `models/User.js`

**Purpose:** Defines the structure of a User document in MongoDB.

**File:** User.js
```javascript
const mongoose = require("mongoose");

// Define the schema (structure) for User documents
const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,  // This field is mandatory
  },
  email: {
    type: String,
    required: true,
    unique: true,    // No two users can have same email
  },
  password: {
    type: String,
    required: true,
    // Note: This stores the HASHED password, not plain text
    // Example: "$2a$10$N9qo8uLOickgx2ZMRZoMy..."
  },
});

// Create and export the model
// "User" → MongoDB will create a "users" collection
module.exports = mongoose.model("User", userSchema);
```

**MongoDB Document Example:**
```json
{
  "_id": "507f1f77bcf86cd799439011",
  "username": "john_doe",
  "email": "john@example.com",
  "password": "$2a$10$N9qo8uLOickgx2ZMRZoMy...",
  "__v": 0
}
```

---

#### 4.3.2 Blog Model: `models/Blog.js`

**Purpose:** Defines the structure of a Blog document with a reference to the author.

**File:** Blog.js
```javascript
const mongoose = require("mongoose");

const blogSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: true,
    },
    content: {
      type: String,
      required: true,
    },
    author: {
      // This stores the User's ObjectId (reference)
      type: mongoose.Schema.Types.ObjectId,
      // "ref" tells Mongoose which model to use for population
      ref: "User",
      required: true,
    },
  },
  {
    // Automatically add createdAt and updatedAt fields
    timestamps: true,
  }
);

module.exports = mongoose.model("Blog", blogSchema);
```

**MongoDB Document Example:**
```json
{
  "_id": "507f1f77bcf86cd799439022",
  "title": "My First Blog Post",
  "content": "This is the content of my blog...",
  "author": "507f1f77bcf86cd799439011",
  "createdAt": "2026-01-28T10:30:00.000Z",
  "updatedAt": "2026-01-28T10:30:00.000Z",
  "__v": 0
}
```

**Key Concept - References:**
```text
┌─────────────────┐         ┌─────────────────┐
│      Blog       │         │      User       │
├─────────────────┤         ├─────────────────┤
│ _id             │         │ _id             │◄──┐
│ title           │         │ username        │   │
│ content         │         │ email           │   │
│ author ─────────┼────────►│ password        │   │
│ createdAt       │  refs   └─────────────────┘   │
│ updatedAt       │                               │
└─────────────────┘                               │
        │                                         │
        └─────────────────────────────────────────┘
              author field stores User's _id
```

---

### 4.4 Middleware: `middleware/authMiddleware.js`

**Purpose:** Protects routes by verifying JWT tokens. Acts as a security guard.

**File:** authMiddleware.js
```javascript
const jwt = require("jsonwebtoken");

const authMiddleware = (req, res, next) => {
  // Step 1: Get the Authorization header
  // Format: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  const authHeader = req.headers.authorization;

  // Step 2: Check if header exists
  if (!authHeader) {
    return res.status(401).json({ message: "No token provided" });
  }

  // Step 3: Extract the token (remove "Bearer " prefix)
  const parts = authHeader.split(" ");
  if (parts.length !== 2 || parts[0] !== "Bearer") {
    return res.status(401).json({ message: "Token format invalid" });
  }
  const token = parts[1];

  try {
    // Step 4: Verify the token using our secret key
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Step 5: Attach user ID to request object
    // Now controllers can access req.userId
    req.userId = decoded.userId;
    
    // Step 6: Proceed to the next middleware/controller
    next();
  } catch (error) {
    // Token is invalid or expired
    return res.status(401).json({ message: "Invalid or expired token" });
  }
};

module.exports = authMiddleware;
```

**How Middleware Works (Visual):**
```text
Request with Token
        │
        ▼
┌───────────────────┐
│   authMiddleware  │
├───────────────────┤
│ 1. Check header   │──── No token? ──► 401 Unauthorized
│ 2. Extract token  │
│ 3. Verify JWT     │──── Invalid? ────► 401 Unauthorized
│ 4. Attach userId  │
│ 5. Call next()    │
└───────────────────┘
        │
        ▼
   Controller
   (req.userId available)
```

---

### 4.5 Controllers

#### 4.5.1 Auth Controller: `controllers/authController.js`

**Purpose:** Handles user registration and login logic.

**File:** authController.js
```javascript
const User = require("../models/User");
const bcrypt = require("bcrypt");
const jwt = require("jsonwebtoken");

// ==================== SIGNUP ====================
exports.signup = async (req, res) => {
  try {
    // Step 1: Extract data from request body
    const { username, email, password } = req.body;

    // Step 2: Check if user already exists
    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return res.status(400).json({ message: "Email already registered" });
    }

    // Step 3: Hash the password (NEVER store plain text passwords!)
    // bcrypt.hash(password, saltRounds)
    // Salt rounds = 10 means 2^10 iterations (good balance of security/speed)
    const hashedPassword = await bcrypt.hash(password, 10);

    // Step 4: Create new user in database
    const user = await User.create({
      username,
      email,
      password: hashedPassword,
    });

    // Step 5: Send success response
    res.status(201).json({
      message: "User created successfully",
      user: { id: user._id, username: user.username },
    });
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};

// ==================== LOGIN ====================
exports.login = async (req, res) => {
  try {
    // Step 1: Extract credentials from request
    const { email, password } = req.body;

    // Step 2: Find user by email
    const user = await User.findOne({ email });
    if (!user) {
      return res.status(400).json({ message: "Invalid credentials" });
    }

    // Step 3: Compare password with stored hash
    // bcrypt.compare() handles the hashing internally
    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) {
      return res.status(400).json({ message: "Invalid credentials" });
    }

    // Step 4: Generate JWT token
    // jwt.sign(payload, secret, options)
    const token = jwt.sign(
      { userId: user._id },           // Payload (data stored in token)
      process.env.JWT_SECRET,          // Secret key for signing
      { expiresIn: "24h" }             // Token expires in 24 hours
    );

    // Step 5: Send token and user info
    res.json({
      token,
      user: {
        id: user._id,
        username: user.username,
        email: user.email,
      },
    });
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};
```

**Password Hashing Flow:**
```text
SIGNUP:
User enters: "mypassword123"
        │
        ▼
bcrypt.hash("mypassword123", 10)
        │
        ▼
Stored in DB: "$2a$10$N9qo8uLOickgx2ZMRZoMyeK3..."

LOGIN:
User enters: "mypassword123"
        │
        ▼
bcrypt.compare("mypassword123", "$2a$10$N9qo8uLOickgx2ZMRZoMyeK3...")
        │
        ▼
Returns: true (passwords match!)
```

**JWT Token Structure:**
```text
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI1MDdmMWY3N2JjZjg2Y2Q3OTk0MzkwMTEiLCJpYXQiOjE3MDY0MzM4MDAsImV4cCI6MTcwNjUyMDIwMH0.X4bL9ZT_UQfN8tVR2Ws5kM...

┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│     HEADER      │  │     PAYLOAD     │  │    SIGNATURE    │
├─────────────────┤  ├─────────────────┤  ├─────────────────┤
│ {               │  │ {               │  │  Created using  │
│   "alg":"HS256",│  │   "userId":     │  │  header +       │
│   "typ":"JWT"   │  │   "507f1f77..", │  │  payload +      │
│ }               │  │   "iat": ...,   │  │  JWT_SECRET     │
│                 │  │   "exp": ...    │  │                 │
│                 │  │ }               │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘
```

---

#### 4.5.2 Blog Controller: `controllers/blogController.js`

**Purpose:** Handles all blog-related CRUD operations.

**File:** blogController.js
```javascript
const Blog = require("../models/Blog");

// ==================== GET ALL BLOGS (Public) ====================
exports.getBlogs = async (req, res) => {
  try {
    // Find all blogs and populate author field with username
    // .populate() replaces the author ObjectId with actual user data
    const blogs = await Blog.find()
      .populate("author", "username")  // Only get username from User
      .sort({ createdAt: -1 });        // Newest first

    res.json(blogs);
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};

// ==================== GET SINGLE BLOG (Public) ====================
exports.getBlogById = async (req, res) => {
  try {
    const blog = await Blog.findById(req.params.id)
      .populate("author", "username");

    if (!blog) {
      return res.status(404).json({ message: "Blog not found" });
    }

    res.json(blog);
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};

// ==================== CREATE BLOG (Protected) ====================
exports.createBlog = async (req, res) => {
  try {
    const { title, content } = req.body;

    // req.userId comes from authMiddleware
    const blog = await Blog.create({
      title,
      content,
      author: req.userId,  // Set current user as author
    });

    // Populate author info before sending response
    await blog.populate("author", "username");

    res.status(201).json(blog);
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};

// ==================== UPDATE BLOG (Protected + Owner Only) ====================
exports.updateBlog = async (req, res) => {
  try {
    const { title, content } = req.body;

    // Find the blog first
    const blog = await Blog.findById(req.params.id);
    if (!blog) {
      return res.status(404).json({ message: "Blog not found" });
    }

    // Check if current user is the author
    // blog.author is ObjectId, req.userId is string
    // .toString() converts ObjectId to string for comparison
    if (blog.author.toString() !== req.userId) {
      return res.status(403).json({ message: "Not authorized to edit this blog" });
    }

    // Update the blog
    blog.title = title || blog.title;
    blog.content = content || blog.content;
    await blog.save();

    await blog.populate("author", "username");
    res.json(blog);
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};

// ==================== DELETE BLOG (Protected + Owner Only) ====================
exports.deleteBlog = async (req, res) => {
  try {
    const blog = await Blog.findById(req.params.id);
    if (!blog) {
      return res.status(404).json({ message: "Blog not found" });
    }

    // Check ownership
    if (blog.author.toString() !== req.userId) {
      return res.status(403).json({ message: "Not authorized to delete this blog" });
    }

    await blog.deleteOne();
    res.json({ message: "Blog deleted successfully" });
  } catch (error) {
    res.status(500).json({ message: "Server error", error: error.message });
  }
};
```

**Populate Explained:**
```text
WITHOUT .populate("author", "username"):
{
  "_id": "abc123",
  "title": "My Blog",
  "author": "507f1f77bcf86cd799439011"  ← Just the ID
}

WITH .populate("author", "username"):
{
  "_id": "abc123",
  "title": "My Blog",
  "author": {
    "_id": "507f1f77bcf86cd799439011",
    "username": "john_doe"               ← Actual user data!
  }
}
```

---

### 4.6 Routes

#### 4.6.1 Auth Routes: `routes/auth.js`

**Purpose:** Defines authentication endpoints.

**File:** auth.js
```javascript
const express = require("express");
const { signup, login } = require("../controllers/authController");

const router = express.Router();

// POST /api/signup - Register new user
router.post("/signup", signup);

// POST /api/login - Login existing user
router.post("/login", login);

module.exports = router;
```

---

#### 4.6.2 Blog Routes: `routes/blogs.js`

**Purpose:** Defines blog CRUD endpoints with authentication.

**File:** blogs.js
```javascript
const express = require("express");
const {
  getBlogs,
  getBlogById,
  createBlog,
  updateBlog,
  deleteBlog,
} = require("../controllers/blogController");
const auth = require("../middleware/authMiddleware");

const router = express.Router();

// ============== PUBLIC ROUTES ==============
// No authentication required

// GET /api/blogs - Get all blogs
router.get("/", getBlogs);

// GET /api/blogs/:id - Get single blog by ID
router.get("/:id", getBlogById);

// ============== PROTECTED ROUTES ==============
// Authentication required (auth middleware)

// POST /api/blogs - Create new blog
router.post("/", auth, createBlog);

// PUT /api/blogs/:id - Update blog
router.put("/:id", auth, updateBlog);

// DELETE /api/blogs/:id - Delete blog
router.delete("/:id", auth, deleteBlog);

module.exports = router;
```

**Route Protection Pattern:**
```text
PUBLIC:    router.get("/", getBlogs)
                    │
                    ▼
               Controller

PROTECTED: router.post("/", auth, createBlog)
                    │       │
                    ▼       │
               Middleware ──┘
                    │
                    ▼ (if token valid)
               Controller
```

---

## 5. API Endpoints Summary

### Authentication Endpoints
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/signup` | Register new user | ❌ No |
| POST | `/api/login` | Login user | ❌ No |

### Blog Endpoints
| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/blogs` | Get all blogs | ❌ No |
| GET | `/api/blogs/:id` | Get single blog | ❌ No |
| POST | `/api/blogs` | Create blog | ✅ Yes |
| PUT | `/api/blogs/:id` | Update blog | ✅ Yes (Owner) |
| DELETE | `/api/blogs/:id` | Delete blog | ✅ Yes (Owner) |

### Request/Response Examples

**Signup Request:**
```http
POST /api/signup
Content-Type: application/json

{
  "username": "john_doe",
  "email": "john@example.com",
  "password": "securepassword123"
}
```

**Login Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "507f1f77bcf86cd799439011",
    "username": "john_doe",
    "email": "john@example.com"
  }
}
```

**Create Blog Request:**
```http
POST /api/blogs
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

{
  "title": "My First Blog",
  "content": "This is the content of my first blog post."
}
```

---

## 6. Data Flow Diagram

### Complete Request Lifecycle

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                           FRONTEND (React)                               │
│                                                                          │
│   User clicks "Create Blog" → axios.post("/api/blogs", {title, content}) │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ HTTP Request with JWT
                                    ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                           server.js                                      │
│                    (Listening on Port 5000)                              │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                            app.js                                        │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐          │
│  │     cors()      │→ │  express.json() │→ │  Route Matcher  │          │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘          │
└──────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ Matches: POST /api/blogs
                                    ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                     routes/blogs.js                                      │
│                                                                          │
│   router.post("/", auth, createBlog)                                     │
│                      │                                                   │
└──────────────────────│───────────────────────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                  middleware/authMiddleware.js                            │
│                                                                          │
│   1. Extract token from Authorization header                             │
│   2. Verify token with JWT_SECRET                                        │
│   3. Attach req.userId = decoded.userId                                  │
│   4. Call next()                                                         │
└──────────────────────────────────────────────────────────────────────────┘
                       │
                       │ Token valid, proceed
                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│               controllers/blogController.js                              │
│                                                                          │
│   exports.createBlog = async (req, res) => {                             │
│     const blog = await Blog.create({                                     │
│       title: req.body.title,                                             │
│       content: req.body.content,                                         │
│       author: req.userId   ← From middleware                             │
│     });                                                                  │
│     res.status(201).json(blog);                                          │
│   }                                                                      │
└──────────────────────────────────────────────────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                     models/Blog.js                                       │
│                                                                          │
│   Mongoose validates data against schema                                 │
│   MongoDB creates new document                                           │
└──────────────────────────────────────────────────────────────────────────┘
                       │
                       │ New blog document
                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                        MongoDB                                           │
│                                                                          │
│   blogs collection: { _id, title, content, author, createdAt, ... }      │
└──────────────────────────────────────────────────────────────────────────┘
                       │
                       │ Response flows back
                       ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                        FRONTEND                                          │
│                                                                          │
│   .then(response => setBlog(response.data))                              │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference Commands

```bash
# Install dependencies
cd backend
npm install

# Start development server (with auto-reload)
npm run dev

# Start production server
npm start

# Test API with curl
curl http://localhost:5000/api/blogs
```

---
