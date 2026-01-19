
## 1. Full Stack Architecture (MERN)

![Image](https://substackcdn.com/image/fetch/%24s_%21g3db%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4a38175b-11e8-40ae-879c-ab3ce2027089_2008x1252.png)

**Full stack architecture** defines how different layers of a web application are structured and interact with each other.

### MERN Stack Components

- **MongoDB**
    
    - NoSQL database
        
    - Stores data as documents (JSON-like format)
        
- **Express.js**
    
    - Backend framework
        
    - Manages routing and middleware
        
- **React**
    
    - Frontend UI library
        
    - Builds reusable components
        
- **Node.js**
    
    - JavaScript runtime for backend
        
    - Handles server-side logic
        

### Architecture Layers

1. **Frontend (Client Layer)**
    
    - React runs in the browser
        
    - Handles UI, events, and state
        
2. **Backend (Server Layer)**
    
    - Node.js + Express.js
        
    - Handles APIs, authentication, business logic
        
3. **Database Layer**
    
    - MongoDB stores application data
        
    - Uses collections and documents
        
4. **API / Communication Layer**
    
    - REST APIs
        
    - Uses HTTP methods and JSON
        

### Key Points

- Single language (JavaScript) across stack
    
- Modular and scalable
    
- Clear separation of concerns
    

---

## 2. MERN Workflow

![Image](https://www.bocasay.com/wp-content/uploads/2020/03/MERN-stack-1.png)

### Workflow Steps

1. User interacts with **React UI**
    
2. React sends HTTP request (`fetch` / `axios`)
    
3. Request hits **Express.js routes**
    
4. Middleware processes request
    
5. Backend communicates with **MongoDB**
    
6. MongoDB returns data
    
7. Backend sends JSON response
    
8. React updates UI using state
    

### Example

- User clicks “Submit”
    
- React sends POST request
    
- Express validates data
    
- MongoDB stores/retrieves record
    
- Response displayed on UI
    

### Advantages

- Reusable APIs
    
- Better scalability
    
- Clean client–server separation
    

---

## 3. Web Request–Response Cycle

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20250705152348042640/Request-and-Response-Cycle.webp)

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20210905091508/ImageOfHTTPRequestResponse-660x374.png)

### Cycle Stages

1. **Client Request**
    
    - Browser sends HTTP request
        
    - Methods: GET, POST, PUT, DELETE
        
2. **Server Receives Request**
    
    - Node.js listens on a port
        
    - Express routes handle requests
        
3. **Processing**
    
    - Middleware execution
        
    - Business logic
        
    - Database interaction
        
4. **Server Response**
    
    - Status code (200, 404, 500)
        
    - Headers and response body (JSON)
        
5. **Client Rendering**
    
    - React processes response
        
    - DOM updates dynamically
        

### Common HTTP Status Codes

- 200 – OK
    
- 201 – Created
    
- 400 – Bad Request
    
- 401 – Unauthorized
    
- 404 – Not Found
    
- 500 – Server Error
    

---

## 4. Dev Tools Overview

![Image](https://developer.chrome.com/static/docs/devtools/overview/image/elements-panel-546127ed29eac.png)

### Frontend Dev Tools

- **Browser DevTools**
    
    - Inspect HTML/CSS
        
    - Debug JavaScript
        
    - View network requests
        
- **React Developer Tools**
    
    - Inspect components
        
    - View props and state
        

### Backend Dev Tools

- **Postman
    
    - API testing
        
- **VS Code Debugger**
    
    - Breakpoints and step execution
        

### Database Tools

- **MongoDB Compass**
    
    - Visual database interface
        
    - Query and index management
        

### Version Control & Build Tools

- **Git & GitHub**
    
- **npm / yarn**
    
- **Vite / Webpack**
    

### Deployment Tools

- **Docker**
    
- **Cloud Platforms**
    
    - AWS, Vercel, Netlify, Render

---
### NPM Error (IN WINDOWS)

1. **Adds the PowerShell execution policy command**
    
2. **Creates a first simple Node + React “Hello World” app**
    
3. Explains how frontend and backend connect
    

---

## 1. PowerShell Execution Policy (Required on Windows)

If you’re on **Windows**, run this **once** in **PowerShell (Run as Administrator or normal user)**:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Why this is needed

- Allows running local scripts (like `npm`, `npx`)
    
- Required for React and Node tools to work properly
    
- Applies **only to your user account** (safe)
    

Verify:

```powershell
Get-ExecutionPolicy -Scope CurrentUser
```

Expected output:

```text
RemoteSigned
```

---

## 2. Prerequisites

Make sure these are installed:

- **Node.js** (includes npm)
    
- Code Editor: VS Code
    
- Browser: Chrome / Edge
    

Check installation:

```bash
node -v
npm -v
```

---

## 3. Create Project Structure

```text
hello-mern/
 ├── backend/
 └── frontend/
```

---

## 4. Backend: Node + Express (Hello World API)

### Step 1: Setup Backend

```bash
mkdir backend
cd backend
npm init -y
npm install express
```

### Step 2: Create `index.js`

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from Node Backend 👋");
});

app.listen(5000, () => {
  console.log("Server running on http://localhost:5000");
});
```

### Step 3: Run Backend

```bash
node index.js
```

Open browser:

```
http://localhost:5000
```

 You should see:

```
Hello from Node Backend 
```

---

## 5. Frontend: React Hello World

### Step 1: Create React App

Open a **new terminal**:

```bash
cd frontend
npm create vite@latest .
npm run dev
```

This uses **React**

Open browser:

```
http://localhost:3000
```

You’ll see the React welcome screen.

---

## 6. Connect React to Node Backend

### Step 1: Update `src/App.js`

```js
import { useEffect, useState } from "react";

function App() {
  const [message, setMessage] = useState("");

  useEffect(() => {
    fetch("http://localhost:5000")
      .then(res => res.text())
      .then(data => setMessage(data));
  }, []);

  return (
    <div style={{ padding: "40px", fontSize: "24px" }}>
      <h1>React Frontend</h1>
      <p>{message}</p>
    </div>
  );
}

export default App;
```

### Step 2: Run Both Servers

- Backend → `http://localhost:5000`
    
- Frontend → `http://localhost:3000`
    

Output on browser:

```
React Frontend
Hello from Node Backend 
```

---

## 7. How It Works (Visual Flow)


![Image](https://images-www.contentful.com/fo9twyrwpveg/sz24EGGpoxenPyGltVYQS/28c6fc5be6c6bc9544c0f2ccf4ce275a/image1.png)

### Flow Explanation

1. React loads in browser
    
2. React sends request to Node server
    
3. Express handles request
    
4. Response sent back
    
5. React displays message
    

---
