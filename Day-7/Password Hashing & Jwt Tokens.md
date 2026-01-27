
## 1️. Password Hashing (VERY IMPORTANT)

 **Never store plain text passwords** in the database.

### Why hashing?

- If DB is hacked, passwords stay safe
    
- Hashing is **one-way** (you can’t get original password back)
    

### Common library

```js
bcrypt
```

### Flow

1. User enters password
    
2. Password is **hashed**
    
3. Hashed password is stored in DB
    
4. During login → compare entered password with stored hash
    

### Example (Register)

```js
const bcrypt = require("bcrypt");

const hashedPassword = await bcrypt.hash(password, 10);
// 10 = salt rounds

user.password = hashedPassword;
await user.save();
```

### Example (Login)

```js
const isMatch = await bcrypt.compare(password, user.password);

if (!isMatch) {
  return res.status(401).json({ message: "Invalid credentials" });
}
```
 **Key idea:**  
You never decrypt passwords — you only _compare hashes_.

---

## 2️. JWT (JSON Web Token)

JWT is used for **authentication** (keeping user logged in).

### What JWT does

- Server gives a **token** after login
    
- Client stores the token (usually localStorage or cookie)
    
- Token is sent with every protected request
    
- Server verifies token → allows access
    

---

### JWT Structure

```
HEADER.PAYLOAD.SIGNATURE
```

- **Payload** → user info (id, email, role)
    
- **Signature** → created using secret key
    

---

### Install JWT

```bash
npm install jsonwebtoken
```

---

### Generate Token (after login)

```js
const jwt = require("jsonwebtoken");

const token = jwt.sign(
  { id: user._id },
  process.env.JWT_SECRET,
  { expiresIn: "1d" }
);
```

Send it to client:

```js
res.json({ token });
```

---

## 3️. Protecting Routes (Middleware)

This is where JWT really shines ✨

### Auth Middleware

```js
const jwt = require("jsonwebtoken");

const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) {
    return res.status(401).json({ message: "No token" });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ message: "Invalid token" });
  }
};
```

### Protected Route

```js
app.get("/profile", authMiddleware, (req, res) => {
  res.json({ message: "Welcome", user: req.user });
});
```

---

## 4️. Full Auth Flow (Easy to Remember)

 **Register**

- Hash password → save user
    

 **Login**

- Compare password
    
- Generate JWT
    
- Send token
    

 **Client**

- Stores token
    
- Sends token in headers
    

**Server**

- Verifies token
    
- Gives access
    
