# MERN Stack Blog - Frontend Documentation

## Table of Contents
1. Project Overview
2. Folder Structure
3. Configuration Files
4. File-by-File Implementation
5. Component Hierarchy
6. Data Flow Diagram

---

## 1. Project Overview

### What is this Frontend?
This is a **Single Page Application (SPA)** built with React that provides the user interface for our blog application. It handles:
- **User Interface** (Forms, Cards, Navigation)
- **Routing** (Navigate between pages without page reload)
- **State Management** (useState, useEffect hooks)
- **API Communication** (Axios for HTTP requests)
- **Authentication State** (LocalStorage for token persistence)

### Technologies Used
| Technology | Purpose |
|------------|---------|
| **React 18** | UI library for building components |
| **Vite** | Fast build tool and dev server |
| **React Router DOM** | Client-side routing |
| **Axios** | HTTP client for API calls |
| **Tailwind CSS** | Utility-first CSS framework |
| **LocalStorage** | Browser storage for auth tokens |

---

## 2. Folder Structure

```text
frontend/
├── index.html                    # 📄 HTML entry point
├── package.json                  # 📦 Dependencies and scripts
├── vite.config.js                # ⚡ Vite configuration
├── tailwind.config.js            # 🎨 Tailwind CSS configuration
├── postcss.config.js             # 🔧 PostCSS configuration
├── .env                          # 🔐 Environment variables
│
└── src/                          # 📁 Source code
    │
    ├── main.jsx                  # 🚀 ENTRY POINT - React initialization
    ├── App.jsx                   # 🗺️  ROUTING - Page navigation
    ├── index.css                 # 🎨 Global styles (Tailwind imports)
    │
    ├── api/                      # 🌐 API CONFIGURATION
    │   └── axios.js              #    - Axios instance with interceptors
    │
    ├── components/               # 🧩 REUSABLE UI COMPONENTS
    │   ├── Nav.jsx               #    - Navigation bar
    │   └── BlogCard.jsx          #    - Single blog post card
    │
    └── pages/                    # 📄 PAGE COMPONENTS (Views)
        ├── Home.jsx              #    - Blog listing page
        ├── Signup.jsx            #    - User registration page
        ├── Login.jsx             #    - User login page
        └── CreateEdit.jsx        #    - Create/Edit blog page
```

### Visual Architecture
```text
┌─────────────────────────────────────────────────────────────────┐
│                         index.html                              │
│                    (Root HTML template)                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         main.jsx                                │
│              (React initialization & Router setup)              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                          App.jsx                                │
│                    (Route definitions)                          │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                      <Nav />                             │   │
│  └─────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                     <Routes>                             │   │
│  │   "/" → Home.jsx                                         │   │
│  │   "/login" → Login.jsx                                   │   │
│  │   "/signup" → Signup.jsx                                 │   │
│  │   "/create" → CreateEdit.jsx                             │   │
│  │   "/edit/:id" → CreateEdit.jsx                           │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        api/axios.js                             │
│              (HTTP requests to Backend API)                     │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Backend API (Port 5000)                      │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3. Configuration Files

### 3.1 Package Configuration: package.json

**Purpose:** Defines project dependencies, scripts, and metadata.

**File:** package.json
```json
{
  "name": "mern-blog-frontend",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "axios": "^1.6.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.2.0",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.32",
    "tailwindcss": "^3.3.6",
    "vite": "^5.0.0"
  }
}
```

**Dependencies Explained:**
| Package | Purpose |
|---------|---------|
| `react` | Core React library |
| `react-dom` | React DOM rendering |
| `react-router-dom` | Client-side routing |
| `axios` | HTTP client for API calls |
| `tailwindcss` | Utility CSS framework |
| `vite` | Fast development server & bundler |

---

### 3.2 Vite Configuration: `vite.config.js`

**Purpose:** Configures the Vite build tool and dev server.

**File:** vite.config.js
```javascript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,  // Frontend runs on this port
    proxy: {
      // Optional: Proxy API requests to backend
      '/api': {
        target: 'http://localhost:5000',
        changeOrigin: true,
      }
    }
  }
})
```

---

### 3.3 Tailwind Configuration: `tailwind.config.js`

**Purpose:** Configures Tailwind CSS for the project.

**File:** tailwind.config.js
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  // Tell Tailwind which files to scan for class names
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // Custom theme extensions go here
    },
  },
  plugins: [],
}
```

---

### 3.4 PostCSS Configuration: `postcss.config.js`

**Purpose:** Enables Tailwind CSS processing.

**File:** postcss.config.js
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

---

### 3.5 Environment Variables: `.env`

**Purpose:** Stores configuration that may change between environments.

**File:** `frontend/.env`
```properties
VITE_API_URL=http://localhost:5000/api
```

> **Note:** In Vite, environment variables must be prefixed with `VITE_` to be exposed to the client.

---

## 4. File-by-File Implementation

---

### 4.1 HTML Entry Point: `index.html`

**Purpose:** The single HTML file that hosts the React application.

**File:** index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>MERN Blog</title>
  </head>
  <body>
    <!-- React app mounts here -->
    <div id="root"></div>
    
    <!-- Vite injects the script automatically -->
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

**Key Concepts:**
- `<div id="root">` - React mounts the entire app here
- `type="module"` - Enables ES6 module imports

---

### 4.2 Global Styles: `src/index.css`

**Purpose:** Imports Tailwind CSS base styles.

**File:** index.css
```css
/* Tailwind CSS Directives */
@tailwind base;       /* Reset & base styles */
@tailwind components; /* Component classes */
@tailwind utilities;  /* Utility classes */

/* Custom Global Styles (Optional) */
body {
  font-family: 'Inter', system-ui, sans-serif;
}
```

---

### 4.3 React Entry Point: `src/main.jsx`

**Purpose:** Initializes React and sets up the Router.

**File:** main.jsx
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App.jsx'
import './index.css'

// Create root and render app
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    {/* BrowserRouter enables client-side routing */}
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
)
```

**Key Concepts:**
| Concept | Explanation |
|---------|-------------|
| `ReactDOM.createRoot()` | React 18 way to create root |
| `<React.StrictMode>` | Enables additional development warnings |
| `<BrowserRouter>` | Wraps app to enable routing |

**Visual Flow:**
```text
index.html
    │
    └── <div id="root">
              │
              └── main.jsx
                    │
                    └── <BrowserRouter>
                              │
                              └── <App />
```

---

### 4.4 API Configuration: `src/api/axios.js`

**Purpose:** Creates a configured Axios instance with automatic token injection.

**File:** axios.js
```javascript
import axios from "axios";

// Create axios instance with base configuration
const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || "http://localhost:5000/api",
  headers: {
    "Content-Type": "application/json",
  },
});

// ============== REQUEST INTERCEPTOR ==============
// Runs BEFORE every request is sent
api.interceptors.request.use(
  (config) => {
    // Get token from localStorage
    const token = localStorage.getItem("token");
    
    // If token exists, add it to Authorization header
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// ============== RESPONSE INTERCEPTOR ==============
// Runs AFTER every response is received
api.interceptors.response.use(
  (response) => {
    // Return successful response as-is
    return response;
  },
  (error) => {
    // Handle 401 Unauthorized errors
    if (error.response?.status === 401) {
      // Token expired or invalid - clear storage and redirect
      localStorage.removeItem("token");
      localStorage.removeItem("user");
      window.location.href = "/login";
    }
    return Promise.reject(error);
  }
);

export default api;
```

**Interceptor Flow Diagram:**
```text
┌─────────────────────────────────────────────────────────────────┐
│                      Component makes request                     │
│                api.get("/blogs") or api.post("/blogs")          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    REQUEST INTERCEPTOR                          │
│                                                                 │
│   1. Check localStorage for token                               │
│   2. If token exists, add: Authorization: Bearer <token>        │
│   3. Forward request to server                                  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Backend API (Port 5000)                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   RESPONSE INTERCEPTOR                          │
│                                                                 │
│   Success (200): Return data to component                       │
│   Error (401): Clear token, redirect to /login                  │
│   Other errors: Return error to component                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Component receives data                     │
│                    .then(res => setBlogs(res.data))             │
└─────────────────────────────────────────────────────────────────┘
```

**Why Use Interceptors?**
| Without Interceptors | With Interceptors |
|---------------------|-------------------|
| Add token manually to every request | Token added automatically |
| Handle 401 in every component | Centralized error handling |
| Duplicate code everywhere | Single source of truth |

---

### 4.5 App Component & Routing: `src/App.jsx`

**Purpose:** Defines all routes and the main layout structure.

**File:** App.jsx
```jsx
import { Routes, Route } from "react-router-dom";

// Components
import Nav from "./components/Nav";

// Pages
import Home from "./pages/Home";
import Signup from "./pages/Signup";
import Login from "./pages/Login";
import CreateEdit from "./pages/CreateEdit";

function App() {
  return (
    <div className="min-h-screen bg-gray-100">
      {/* Navigation bar - appears on all pages */}
      <Nav />
      
      {/* Main content area */}
      <main className="container mx-auto px-4 py-8">
        {/* Route definitions */}
        <Routes>
          {/* Public Routes */}
          <Route path="/" element={<Home />} />
          <Route path="/signup" element={<Signup />} />
          <Route path="/login" element={<Login />} />
          
          {/* Protected Routes (handled in components) */}
          <Route path="/create" element={<CreateEdit />} />
          
          {/* Dynamic Route - :id is a URL parameter */}
          <Route path="/edit/:id" element={<CreateEdit />} />
          
          {/* 404 Catch-all Route */}
          <Route path="*" element={<div>Page Not Found</div>} />
        </Routes>
      </main>
    </div>
  );
}

export default App;
```

**Routing Concepts:**
```text
URL: /                  → Renders: <Home />
URL: /login             → Renders: <Login />
URL: /signup            → Renders: <Signup />
URL: /create            → Renders: <CreateEdit /> (create mode)
URL: /edit/abc123       → Renders: <CreateEdit /> (edit mode, id="abc123")
URL: /anything-else     → Renders: "Page Not Found"
```

**Dynamic Route Parameter:**
```text
Route: /edit/:id
URL:   /edit/507f1f77bcf86cd799439011
                 └─────────┬─────────┘
                           │
                    useParams() returns:
                    { id: "507f1f77bcf86cd799439011" }
```

---

### 4.6 Navigation Component: `src/components/Nav.jsx`

**Purpose:** Displays navigation links and handles logout. Conditionally renders based on authentication state.

**File:** Nav.jsx
```jsx
import { Link, useNavigate } from "react-router-dom";

const Nav = () => {
  const navigate = useNavigate();
  
  // Check if user is logged in by checking for token
  const token = localStorage.getItem("token");
  const user = JSON.parse(localStorage.getItem("user") || "null");

  // Handle logout
  const handleLogout = () => {
    // Clear authentication data
    localStorage.removeItem("token");
    localStorage.removeItem("user");
    // Redirect to login page
    navigate("/login");
  };

  return (
    <nav className="bg-blue-600 text-white shadow-lg">
      <div className="container mx-auto px-4">
        <div className="flex justify-between items-center h-16">
          
          {/* Logo/Brand */}
          <Link to="/" className="text-xl font-bold">
            📝 MERN Blog
          </Link>

          {/* Navigation Links */}
          <div className="flex items-center space-x-4">
            <Link to="/" className="hover:text-blue-200">
              Home
            </Link>

            {/* Conditional Rendering based on auth state */}
            {token ? (
              // User is LOGGED IN - show these links
              <>
                <span className="text-blue-200">
                  Hello, {user?.username}
                </span>
                <Link
                  to="/create"
                  className="bg-green-500 px-4 py-2 rounded hover:bg-green-600"
                >
                  + New Post
                </Link>
                <button
                  onClick={handleLogout}
                  className="bg-red-500 px-4 py-2 rounded hover:bg-red-600"
                >
                  Logout
                </button>
              </>
            ) : (
              // User is NOT LOGGED IN - show these links
              <>
                <Link
                  to="/login"
                  className="hover:text-blue-200"
                >
                  Login
                </Link>
                <Link
                  to="/signup"
                  className="bg-white text-blue-600 px-4 py-2 rounded hover:bg-gray-100"
                >
                  Sign Up
                </Link>
              </>
            )}
          </div>
        </div>
      </div>
    </nav>
  );
};

export default Nav;
```

**Conditional Rendering Flow:**
```text
┌─────────────────────────────────────────────────────────────────┐
│                         Nav Component                           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │ token exists?   │
                    └─────────────────┘
                     /              \
                   Yes               No
                   /                  \
                  ▼                    ▼
     ┌─────────────────────┐  ┌─────────────────────┐
     │  Show:              │  │  Show:              │
     │  - Username         │  │  - Login link       │
     │  - New Post button  │  │  - Signup link      │
     │  - Logout button    │  │                     │
     └─────────────────────┘  └─────────────────────┘
```

**Key Hooks Used:**
| Hook | Purpose |
|------|---------|
| `useNavigate()` | Programmatic navigation (redirect after logout) |
| `Link` | Declarative navigation (clickable links) |

---

### 4.7 Blog Card Component: `src/components/BlogCard.jsx`

**Purpose:** Displays a single blog post with conditional edit/delete buttons for the author.

**File:** BlogCard.jsx
```jsx
import { Link, useNavigate } from "react-router-dom";
import api from "../api/axios";

const BlogCard = ({ blog, onDelete }) => {
  const navigate = useNavigate();
  
  // Get current user from localStorage
  const user = JSON.parse(localStorage.getItem("user") || "null");
  
  // Check if current user is the author of this blog
  // blog.author can be an object (populated) or string (ID)
  const authorId = blog.author?._id || blog.author;
  const isAuthor = user && user.id === authorId;

  // Format date for display
  const formatDate = (dateString) => {
    return new Date(dateString).toLocaleDateString("en-US", {
      year: "numeric",
      month: "long",
      day: "numeric",
    });
  };

  // Handle delete button click
  const handleDelete = async () => {
    // Confirm before deleting
    if (!window.confirm("Are you sure you want to delete this post?")) {
      return;
    }

    try {
      await api.delete(`/blogs/${blog._id}`);
      // Notify parent component to refresh the list
      if (onDelete) {
        onDelete(blog._id);
      }
    } catch (error) {
      console.error("Error deleting blog:", error);
      alert("Failed to delete blog");
    }
  };

  return (
    <div className="bg-white rounded-lg shadow-md overflow-hidden hover:shadow-lg transition-shadow">
      {/* Card Header */}
      <div className="p-6">
        {/* Title */}
        <h2 className="text-xl font-bold text-gray-800 mb-2">
          {blog.title}
        </h2>

        {/* Author & Date */}
        <div className="flex items-center text-sm text-gray-500 mb-4">
          <span className="font-medium text-blue-600">
            {blog.author?.username || "Unknown"}
          </span>
          <span className="mx-2">•</span>
          <span>{formatDate(blog.createdAt)}</span>
        </div>

        {/* Content Preview */}
        <p className="text-gray-600 line-clamp-3">
          {blog.content.length > 150
            ? `${blog.content.substring(0, 150)}...`
            : blog.content}
        </p>
      </div>

      {/* Card Footer - Action Buttons */}
      <div className="px-6 py-4 bg-gray-50 flex justify-between items-center">
        {/* Read More Link */}
        <Link
          to={`/blog/${blog._id}`}
          className="text-blue-600 hover:text-blue-800 font-medium"
        >
          Read More →
        </Link>

        {/* Edit/Delete - Only shown to author */}
        {isAuthor && (
          <div className="flex space-x-2">
            <button
              onClick={() => navigate(`/edit/${blog._id}`)}
              className="px-3 py-1 bg-yellow-500 text-white rounded hover:bg-yellow-600 text-sm"
            >
              ✏️ Edit
            </button>
            <button
              onClick={handleDelete}
              className="px-3 py-1 bg-red-500 text-white rounded hover:bg-red-600 text-sm"
            >
              🗑️ Delete
            </button>
          </div>
        )}
      </div>
    </div>
  );
};

export default BlogCard;
```

**Props Explained:**
```text
<BlogCard 
  blog={blogObject}      // The blog data to display
  onDelete={handleDelete} // Callback when blog is deleted
/>
```

**Ownership Check Logic:**
```text
┌─────────────────────────────────────────────────────────────────┐
│                         BlogCard                                │
└─────────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┴─────────────────────┐
        │                                           │
        ▼                                           ▼
┌───────────────────┐                    ┌───────────────────┐
│ localStorage.user │                    │    blog.author    │
│   id: "abc123"    │                    │    _id: "abc123"  │
└───────────────────┘                    └───────────────────┘
        │                                           │
        └─────────────────┬─────────────────────────┘
                          ▼
                  ┌───────────────┐
                  │ user.id ===   │
                  │ blog.author?  │
                  └───────────────┘
                    /          \
                  Yes           No
                  /              \
                 ▼                ▼
        ┌─────────────┐    ┌─────────────┐
        │ Show Edit & │    │ Hide Edit & │
        │ Delete btns │    │ Delete btns │
        └─────────────┘    └─────────────┘
```

---

### 4.8 Home Page: `src/pages/Home.jsx`

**Purpose:** Displays all blog posts fetched from the API.

**File:** Home.jsx
```jsx
import { useState, useEffect } from "react";
import api from "../api/axios";
import BlogCard from "../components/BlogCard";

const Home = () => {
  // State for storing blogs
  const [blogs, setBlogs] = useState([]);
  // State for loading indicator
  const [loading, setLoading] = useState(true);
  // State for error messages
  const [error, setError] = useState(null);

  // Fetch blogs when component mounts
  useEffect(() => {
    fetchBlogs();
  }, []); // Empty dependency array = run once on mount

  // Function to fetch all blogs
  const fetchBlogs = async () => {
    try {
      setLoading(true);
      const response = await api.get("/blogs");
      setBlogs(response.data);
      setError(null);
    } catch (err) {
      setError("Failed to load blogs. Please try again.");
      console.error("Error fetching blogs:", err);
    } finally {
      setLoading(false);
    }
  };

  // Handler for when a blog is deleted
  const handleDelete = (deletedId) => {
    // Remove the deleted blog from state
    setBlogs(blogs.filter((blog) => blog._id !== deletedId));
  };

  // Render loading state
  if (loading) {
    return (
      <div className="flex justify-center items-center h-64">
        <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"></div>
      </div>
    );
  }

  // Render error state
  if (error) {
    return (
      <div className="text-center py-8">
        <p className="text-red-600">{error}</p>
        <button
          onClick={fetchBlogs}
          className="mt-4 px-4 py-2 bg-blue-600 text-white rounded"
        >
          Try Again
        </button>
      </div>
    );
  }

  return (
    <div>
      {/* Page Header */}
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-800">Latest Blog Posts</h1>
        <p className="text-gray-600 mt-2">
          Discover stories, ideas, and insights from our community
        </p>
      </div>

      {/* Blog Grid */}
      {blogs.length === 0 ? (
        <div className="text-center py-12 bg-white rounded-lg">
          <p className="text-gray-500 text-lg">No blog posts yet.</p>
          <p className="text-gray-400">Be the first to create one!</p>
        </div>
      ) : (
        <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
          {blogs.map((blog) => (
            <BlogCard
              key={blog._id}
              blog={blog}
              onDelete={handleDelete}
            />
          ))}
        </div>
      )}
    </div>
  );
};

export default Home;
```

**Component Lifecycle:**
```text
1. Component Mounts
        │
        ▼
2. useEffect runs (empty dependency [])
        │
        ▼
3. fetchBlogs() called
        │
        ├── setLoading(true)
        │
        ▼
4. API request: GET /api/blogs
        │
        ▼
5. Response received
        │
        ├── setBlogs(response.data)
        ├── setLoading(false)
        │
        ▼
6. Component re-renders with data
```

**State Management:**
| State | Type | Purpose |
|-------|------|---------|
| `blogs` | Array | Stores list of blog posts |
| `loading` | Boolean | Shows loading spinner |
| `error` | String/null | Displays error message |

---

### 4.9 Signup Page: `src/pages/Signup.jsx`

**Purpose:** Handles user registration with form validation.

**File:** Signup.jsx
```jsx
import { useState } from "react";
import { Link, useNavigate } from "react-router-dom";
import api from "../api/axios";

const Signup = () => {
  const navigate = useNavigate();
  
  // Form state
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    password: "",
    confirmPassword: "",
  });
  
  // UI state
  const [error, setError] = useState("");
  const [loading, setLoading] = useState(false);

  // Handle input changes
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({
      ...formData,      // Keep existing values
      [name]: value,    // Update changed field
    });
  };

  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault(); // Prevent page reload
    setError("");

    // Validation
    if (formData.password !== formData.confirmPassword) {
      setError("Passwords do not match");
      return;
    }

    if (formData.password.length < 6) {
      setError("Password must be at least 6 characters");
      return;
    }

    try {
      setLoading(true);
      
      // Send signup request
      await api.post("/auth/signup", {
        username: formData.username,
        email: formData.email,
        password: formData.password,
      });

      // Redirect to login on success
      navigate("/login", { 
        state: { message: "Account created! Please login." } 
      });
    } catch (err) {
      setError(err.response?.data?.message || "Signup failed");
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="max-w-md mx-auto">
      <div className="bg-white rounded-lg shadow-md p-8">
        {/* Header */}
        <h1 className="text-2xl font-bold text-center mb-6">Create Account</h1>

        {/* Error Message */}
        {error && (
          <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
            {error}
          </div>
        )}

        {/* Signup Form */}
        <form onSubmit={handleSubmit}>
          {/* Username Field */}
          <div className="mb-4">
            <label className="block text-gray-700 font-medium mb-2">
              Username
            </label>
            <input
              type="text"
              name="username"
              value={formData.username}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your username"
            />
          </div>

          {/* Email Field */}
          <div className="mb-4">
            <label className="block text-gray-700 font-medium mb-2">
              Email
            </label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your email"
            />
          </div>

          {/* Password Field */}
          <div className="mb-4">
            <label className="block text-gray-700 font-medium mb-2">
              Password
            </label>
            <input
              type="password"
              name="password"
              value={formData.password}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your password"
            />
          </div>

          {/* Confirm Password Field */}
          <div className="mb-6">
            <label className="block text-gray-700 font-medium mb-2">
              Confirm Password
            </label>
            <input
              type="password"
              name="confirmPassword"
              value={formData.confirmPassword}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Confirm your password"
            />
          </div>

          {/* Submit Button */}
          <button
            type="submit"
            disabled={loading}
            className="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50"
          >
            {loading ? "Creating Account..." : "Sign Up"}
          </button>
        </form>

        {/* Login Link */}
        <p className="text-center mt-6 text-gray-600">
          Already have an account?{" "}
          <Link to="/login" className="text-blue-600 hover:underline">
            Login here
          </Link>
        </p>
      </div>
    </div>
  );
};

export default Signup;
```

**Form Data Flow:**
```text
User types in input
        │
        ▼
onChange event fires
        │
        ▼
handleChange(e) called
        │
        ▼
setFormData({...formData, [name]: value})
        │
        ▼
State updates, input shows new value
```

---

### 4.10 Login Page: `src/pages/Login.jsx`

**Purpose:** Handles user authentication and token storage.

**File:** Login.jsx
```jsx
import { useState } from "react";
import { Link, useNavigate, useLocation } from "react-router-dom";
import api from "../api/axios";

const Login = () => {
  const navigate = useNavigate();
  const location = useLocation();
  
  // Form state
  const [formData, setFormData] = useState({
    email: "",
    password: "",
  });
  
  // UI state
  const [error, setError] = useState("");
  const [loading, setLoading] = useState(false);
  
  // Success message from signup redirect
  const successMessage = location.state?.message;

  // Handle input changes
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value,
    });
  };

  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    setError("");

    try {
      setLoading(true);
      
      // Send login request
      const response = await api.post("/auth/login", formData);
      
      // Extract token and user from response
      const { token, user } = response.data;
      
      // Store in localStorage for persistence
      localStorage.setItem("token", token);
      localStorage.setItem("user", JSON.stringify(user));
      
      // Redirect to home page
      navigate("/");
      
      // Force page reload to update Nav component
      window.location.reload();
    } catch (err) {
      setError(err.response?.data?.message || "Login failed");
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="max-w-md mx-auto">
      <div className="bg-white rounded-lg shadow-md p-8">
        {/* Header */}
        <h1 className="text-2xl font-bold text-center mb-6">Welcome Back</h1>

        {/* Success Message (from signup) */}
        {successMessage && (
          <div className="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded mb-4">
            {successMessage}
          </div>
        )}

        {/* Error Message */}
        {error && (
          <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
            {error}
          </div>
        )}

        {/* Login Form */}
        <form onSubmit={handleSubmit}>
          {/* Email Field */}
          <div className="mb-4">
            <label className="block text-gray-700 font-medium mb-2">
              Email
            </label>
            <input
              type="email"
              name="email"
              value={formData.email}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your email"
            />
          </div>

          {/* Password Field */}
          <div className="mb-6">
            <label className="block text-gray-700 font-medium mb-2">
              Password
            </label>
            <input
              type="password"
              name="password"
              value={formData.password}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter your password"
            />
          </div>

          {/* Submit Button */}
          <button
            type="submit"
            disabled={loading}
            className="w-full bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50"
          >
            {loading ? "Logging in..." : "Login"}
          </button>
        </form>

        {/* Signup Link */}
        <p className="text-center mt-6 text-gray-600">
          Don't have an account?{" "}
          <Link to="/signup" className="text-blue-600 hover:underline">
            Sign up here
          </Link>
        </p>
      </div>
    </div>
  );
};

export default Login;
```

**Authentication Flow:**
```text
┌─────────────────────────────────────────────────────────────────┐
│                      User submits login form                     │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│               api.post("/auth/login", {email, password})        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Backend validates credentials                 │
└─────────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
        ┌──────────┐                    ┌──────────┐
        │ Success  │                    │  Error   │
        └──────────┘                    └──────────┘
              │                               │
              ▼                               ▼
┌─────────────────────────┐       ┌─────────────────────────┐
│ Store in localStorage:  │       │ Display error message   │
│ - token                 │       └─────────────────────────┘
│ - user object           │
└─────────────────────────┘
              │
              ▼
┌─────────────────────────┐
│ Redirect to Home (/)    │
│ Nav shows logged-in UI  │
└─────────────────────────┘
```

**LocalStorage Usage:**
```javascript
// Storing data (Login success)
localStorage.setItem("token", "eyJhbGciOiJIUzI1NiIs...");
localStorage.setItem("user", JSON.stringify({id: "abc", username: "john"}));

// Reading data (in components)
const token = localStorage.getItem("token");
const user = JSON.parse(localStorage.getItem("user"));

// Clearing data (Logout)
localStorage.removeItem("token");
localStorage.removeItem("user");
```

---

### 4.11 Create/Edit Page: `src/pages/CreateEdit.jsx`

**Purpose:** Dual-purpose form for creating new blogs and editing existing ones.

**File:** CreateEdit.jsx
```jsx
import { useState, useEffect } from "react";
import { useNavigate, useParams } from "react-router-dom";
import api from "../api/axios";

const CreateEdit = () => {
  const navigate = useNavigate();
  const { id } = useParams(); // Get ID from URL if editing
  
  // Determine mode based on presence of ID
  const isEditMode = Boolean(id);
  
  // Form state
  const [formData, setFormData] = useState({
    title: "",
    content: "",
  });
  
  // UI state
  const [error, setError] = useState("");
  const [loading, setLoading] = useState(false);
  const [fetching, setFetching] = useState(isEditMode);

  // Check authentication on mount
  useEffect(() => {
    const token = localStorage.getItem("token");
    if (!token) {
      navigate("/login");
      return;
    }

    // If editing, fetch existing blog data
    if (isEditMode) {
      fetchBlog();
    }
  }, [id, navigate, isEditMode]);

  // Fetch blog data for editing
  const fetchBlog = async () => {
    try {
      const response = await api.get(`/blogs/${id}`);
      const { title, content } = response.data;
      setFormData({ title, content });
    } catch (err) {
      setError("Failed to load blog");
      console.error(err);
    } finally {
      setFetching(false);
    }
  };

  // Handle input changes
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value,
    });
  };

  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    setError("");

    // Validation
    if (!formData.title.trim() || !formData.content.trim()) {
      setError("Title and content are required");
      return;
    }

    try {
      setLoading(true);

      if (isEditMode) {
        // UPDATE existing blog
        await api.put(`/blogs/${id}`, formData);
      } else {
        // CREATE new blog
        await api.post("/blogs", formData);
      }

      // Redirect to home on success
      navigate("/");
    } catch (err) {
      setError(err.response?.data?.message || "Failed to save blog");
    } finally {
      setLoading(false);
    }
  };

  // Show loading while fetching blog for edit
  if (fetching) {
    return (
      <div className="flex justify-center items-center h-64">
        <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600"></div>
      </div>
    );
  }

  return (
    <div className="max-w-2xl mx-auto">
      <div className="bg-white rounded-lg shadow-md p-8">
        {/* Header - changes based on mode */}
        <h1 className="text-2xl font-bold mb-6">
          {isEditMode ? "✏️ Edit Blog Post" : "📝 Create New Blog Post"}
        </h1>

        {/* Error Message */}
        {error && (
          <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
            {error}
          </div>
        )}

        {/* Blog Form */}
        <form onSubmit={handleSubmit}>
          {/* Title Field */}
          <div className="mb-4">
            <label className="block text-gray-700 font-medium mb-2">
              Title
            </label>
            <input
              type="text"
              name="title"
              value={formData.title}
              onChange={handleChange}
              required
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              placeholder="Enter blog title"
            />
          </div>

          {/* Content Field */}
          <div className="mb-6">
            <label className="block text-gray-700 font-medium mb-2">
              Content
            </label>
            <textarea
              name="content"
              value={formData.content}
              onChange={handleChange}
              required
              rows="10"
              className="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 resize-y"
              placeholder="Write your blog content here..."
            />
          </div>

          {/* Action Buttons */}
          <div className="flex gap-4">
            <button
              type="submit"
              disabled={loading}
              className="flex-1 bg-blue-600 text-white py-2 rounded-lg hover:bg-blue-700 disabled:opacity-50"
            >
              {loading
                ? "Saving..."
                : isEditMode
                ? "Update Post"
                : "Publish Post"}
            </button>
            <button
              type="button"
              onClick={() => navigate("/")}
              className="px-6 py-2 border border-gray-300 rounded-lg hover:bg-gray-100"
            >
              Cancel
            </button>
          </div>
        </form>
      </div>
    </div>
  );
};

export default CreateEdit;
```

**Dual-Mode Logic:**
```text
URL: /create
        │
        ▼
useParams() → { id: undefined }
        │
        ▼
isEditMode = Boolean(undefined) = false
        │
        ▼
CREATE MODE
- Empty form
- POST /api/blogs on submit


URL: /edit/507f1f77bcf86cd799439011
        │
        ▼
useParams() → { id: "507f1f77bcf86cd799439011" }
        │
        ▼
isEditMode = Boolean("507f...") = true
        │
        ▼
EDIT MODE
- Fetch existing blog data
- Pre-fill form
- PUT /api/blogs/:id on submit
```

**Component Behavior Chart:**
| Aspect | Create Mode | Edit Mode |
|--------|-------------|-----------|
| URL | `/create` | `/edit/:id` |
| `isEditMode` | `false` | `true` |
| Initial Form | Empty | Pre-filled |
| API Call | `POST /blogs` | `PUT /blogs/:id` |
| Header Text | "Create New Blog" | "Edit Blog Post" |
| Button Text | "Publish Post" | "Update Post" |

---

## 5. Component Hierarchy

```text
┌─────────────────────────────────────────────────────────────────┐
│                           App.jsx                               │
│                     (Root Component)                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                       Nav.jsx                            │   │
│  │                  (Always Visible)                        │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                       <Routes>                           │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │                   Home.jsx                       │    │   │
│  │  │  ┌─────────────────────────────────────────┐    │    │   │
│  │  │  │              BlogCard.jsx               │    │    │   │
│  │  │  │              BlogCard.jsx               │    │    │   │
│  │  │  │              BlogCard.jsx               │    │    │   │
│  │  │  └─────────────────────────────────────────┘    │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  │                          OR                              │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │                  Login.jsx                       │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  │                          OR                              │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │                  Signup.jsx                      │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  │                          OR                              │   │
│  │  ┌─────────────────────────────────────────────────┐    │   │
│  │  │                CreateEdit.jsx                    │    │   │
│  │  └─────────────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 6. Data Flow Diagram

### Complete Frontend-Backend Flow

```text
┌─────────────────────────────────────────────────────────────────┐
│                         USER ACTION                             │
│              (Click button, Submit form, Load page)             │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      React Component                            │
│                                                                 │
│   const [data, setData] = useState([]);                         │
│   const [loading, setLoading] = useState(false);                │
│                                                                 │
│   const fetchData = async () => {                               │
│     setLoading(true);                                           │
│     const response = await api.get("/blogs");  ─────────┐       │
│     setData(response.data);                             │       │
│     setLoading(false);                                  │       │
│   }                                                     │       │
└─────────────────────────────────────────────────────────│───────┘
                                                          │
                                                          ▼
┌─────────────────────────────────────────────────────────────────┐
│                      api/axios.js                               │
│                                                                 │
│   Request Interceptor:                                          │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │ config.headers.Authorization = `Bearer ${token}`        │  │
│   └─────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTP Request
                              │ GET http://localhost:5000/api/blogs
                              │ Headers: { Authorization: "Bearer eyJ..." }
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Backend API (Port 5000)                      │
│                                                                 │
│   1. authMiddleware verifies token                              │
│   2. blogController.getBlogs() queries MongoDB                  │
│   3. Returns JSON response                                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ HTTP Response
                              │ { blogs: [...] }
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      api/axios.js                               │
│                                                                 │
│   Response Interceptor:                                         │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │ if (error.status === 401) redirect to /login            │  │
│   └─────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      React Component                            │
│                                                                 │
│   setData(response.data);  // Update state                      │
│   Component re-renders with new data                            │
│                                                                 │
│   return (                                                      │
│     <div>                                                       │
│       {data.map(blog => <BlogCard blog={blog} />)}              │
│     </div>                                                      │
│   );                                                            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         DOM Updated                             │
│                   (User sees the blogs)                         │
└─────────────────────────────────────────────────────────────────┘
```

---

## 7. Quick Reference Commands

```bash
# Navigate to frontend directory
cd frontend

# Install dependencies
npm install

# Start development server (port 5173)
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## 8. Key React Concepts Summary

| Concept | Hook/Method | Usage |
|---------|-------------|-------|
| **State** | `useState()` | Store component data |
| **Side Effects** | `useEffect()` | API calls, subscriptions |
| **Navigation** | `useNavigate()` | Programmatic routing |
| **URL Params** | `useParams()` | Get dynamic route values |
| **Location** | `useLocation()` | Access current URL info |
| **Links** | `<Link to="/">` | Declarative navigation |

---

## 9. Authentication State Management

```text
┌─────────────────────────────────────────────────────────────────┐
│                      localStorage                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."           │
│                                                                 │
│   "user": '{"id":"507f...","username":"john_doe"}'              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
        │                                               │
        │ Read on every request                         │ Read in components
        ▼                                               ▼
┌─────────────────────┐                    ┌─────────────────────┐
│    axios.js         │                    │    Nav.jsx          │
│  (Add to headers)   │                    │  (Show/hide links)  │
└─────────────────────┘                    └─────────────────────┘
                                                        │
                                                        ▼
                                           ┌─────────────────────┐
                                           │   BlogCard.jsx      │
                                           │ (Show edit/delete)  │
                                           └─────────────────────┘
```

