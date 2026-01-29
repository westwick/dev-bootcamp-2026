# Week 7 — Users & Authentication

*Month 2: The Structured Web*

---

## Goals

- Understand authentication vs authorization
- Learn password hashing with bcrypt
- Implement JWT (JSON Web Token) authentication
- Build a complete auth system
- Protect routes with middleware

---

## Part 1: Authentication Concepts

### What to Learn

- What is authentication vs authorization?
- What is stateful vs stateless authentication?
- What are sessions? What are tokens?
- Why not store plain passwords?

### Authentication vs Authorization

| Authentication | Authorization |
|----------------|---------------|
| Who are you? | What can you do? |
| Verifying identity | Verifying permissions |
| Login process | Access control |
| "You are John" | "John can edit this" |

### Stateful vs Stateless Auth

| Sessions (Stateful) | Tokens (Stateless) |
|--------------------|-------------------|
| Server stores session | Server doesn't store anything |
| Session ID in cookie | Token contains user info |
| Harder to scale | Easy to scale |
| Easy to revoke | Harder to revoke |

### 🤖 AI Learning Prompts

- "Explain authentication vs authorization with examples"
- "What is stateful vs stateless authentication?"
- "What are the pros and cons of sessions vs JWT?"
- "Why can't I just store passwords as plain text?"

---

## Part 2: Password Hashing

### What to Learn

- What is hashing?
- Why can't hashes be reversed?
- What is a salt?
- Why use bcrypt?

### Never Store Plain Passwords!

```
❌ Database breach = All passwords exposed
   user@email.com : password123
   
✅ Database breach = Only hashes exposed  
   user@email.com : $2b$10$N9qo8uLOickgx2ZMRZoMy...
```

### Hashing with bcrypt

```bash
npm install bcrypt
```

```javascript
const bcrypt = require('bcrypt');

// Hash a password (when registering)
const saltRounds = 10;
const passwordHash = await bcrypt.hash('userpassword', saltRounds);
// Result: $2b$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy

// Verify a password (when logging in)
const isValid = await bcrypt.compare('userpassword', passwordHash);
// true or false
```

### What is Salting?

A salt is random data added before hashing:
- Same password → different hashes (per user)
- Prevents rainbow table attacks
- bcrypt automatically handles salting

### 🤖 AI Learning Prompts

- "Explain password hashing for beginners"
- "What is a salt in password hashing?"
- "Why use bcrypt instead of SHA256 or MD5?"
- "What's the cost factor in bcrypt?"

---

## Part 3: JWT (JSON Web Tokens)

### What to Learn

- What is a JWT?
- What are the three parts?
- What goes in the payload?
- How do you verify a JWT?

### JWT Structure

```
header.payload.signature

eyJhbGciOiJIUzI1NiIs...  (header: algorithm + type)
.eyJ1c2VySWQiOjEsImVt...  (payload: your data)
.SflKxwRJSMeKKF2QT4fw...  (signature: verification)
```

### Creating JWTs

```bash
npm install jsonwebtoken
```

```javascript
const jwt = require('jsonwebtoken');

// Create a token
const token = jwt.sign(
  { userId: user.id, email: user.email },  // Payload
  process.env.JWT_SECRET,                   // Secret key
  { expiresIn: '7d' }                       // Options
);

// Verify a token
try {
  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  console.log(decoded.userId);  // Access payload data
} catch (error) {
  console.log('Invalid token');
}
```

### What to Put in JWT Payload

```javascript
// ✅ Good - Non-sensitive identification
{ userId: 123, email: "user@example.com", role: "user" }

// ❌ Bad - Sensitive data
{ password: "...", creditCard: "...", ssn: "..." }
```

### 🤖 AI Learning Prompts

- "Explain JWT tokens for beginners"
- "What are the three parts of a JWT?"
- "What should and shouldn't go in a JWT payload?"
- "How does JWT verification work?"
- "Why do JWTs have an expiration time?"

---

## Part 4: Building the Auth System

### Database: Users Table

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Registration Endpoint

**src/controllers/authController.js**
```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const pool = require('../db');

exports.register = async (req, res, next) => {
  try {
    const { email, password } = req.body;
    
    // Validate input
    if (!email || !password) {
      return res.status(400).json({ 
        error: 'Email and password required' 
      });
    }
    
    if (password.length < 6) {
      return res.status(400).json({ 
        error: 'Password must be at least 6 characters' 
      });
    }
    
    // Check if user exists
    const existing = await pool.query(
      'SELECT id FROM users WHERE email = $1',
      [email]
    );
    
    if (existing.rows.length > 0) {
      return res.status(409).json({ 
        error: 'Email already registered' 
      });
    }
    
    // Hash password
    const passwordHash = await bcrypt.hash(password, 10);
    
    // Create user
    const result = await pool.query(
      `INSERT INTO users (email, password_hash) 
       VALUES ($1, $2) 
       RETURNING id, email, created_at`,
      [email, passwordHash]
    );
    
    const user = result.rows[0];
    
    // Create token
    const token = jwt.sign(
      { userId: user.id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );
    
    res.status(201).json({ user, token });
    
  } catch (error) {
    next(error);
  }
};
```

### Login Endpoint

```javascript
exports.login = async (req, res, next) => {
  try {
    const { email, password } = req.body;
    
    // Validate input
    if (!email || !password) {
      return res.status(400).json({ 
        error: 'Email and password required' 
      });
    }
    
    // Find user
    const result = await pool.query(
      'SELECT * FROM users WHERE email = $1',
      [email]
    );
    
    if (result.rows.length === 0) {
      return res.status(401).json({ 
        error: 'Invalid email or password' 
      });
    }
    
    const user = result.rows[0];
    
    // Verify password
    const isValid = await bcrypt.compare(password, user.password_hash);
    
    if (!isValid) {
      return res.status(401).json({ 
        error: 'Invalid email or password' 
      });
    }
    
    // Create token
    const token = jwt.sign(
      { userId: user.id, email: user.email },
      process.env.JWT_SECRET,
      { expiresIn: '7d' }
    );
    
    // Don't send password_hash!
    res.json({ 
      user: { id: user.id, email: user.email },
      token 
    });
    
  } catch (error) {
    next(error);
  }
};
```

### Auth Routes

**src/routes/authRoutes.js**
```javascript
const express = require('express');
const router = express.Router();
const authController = require('../controllers/authController');

router.post('/register', authController.register);
router.post('/login', authController.login);

module.exports = router;
```

---

## Part 5: Protecting Routes

### Auth Middleware

**src/middleware/auth.js**
```javascript
const jwt = require('jsonwebtoken');

const auth = (req, res, next) => {
  try {
    // Get token from header
    const authHeader = req.headers.authorization;
    
    if (!authHeader) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    // Format: "Bearer <token>"
    const token = authHeader.split(' ')[1];
    
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Add user info to request
    req.user = decoded;
    
    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};

module.exports = auth;
```

### Using Auth Middleware

```javascript
const auth = require('../middleware/auth');

// Protect all task routes
router.use(auth);

// Or protect specific routes
router.get('/tasks', auth, taskController.getAll);
router.post('/tasks', auth, taskController.create);

// Public routes (no auth)
router.get('/health', (req, res) => res.json({ status: 'ok' }));
```

### Accessing User in Controllers

```javascript
exports.getAll = async (req, res, next) => {
  try {
    // req.user was set by auth middleware
    const { userId } = req.user;
    
    // Only get tasks for this user
    const result = await pool.query(
      'SELECT * FROM tasks WHERE user_id = $1 ORDER BY created_at DESC',
      [userId]
    );
    
    res.json({ tasks: result.rows });
  } catch (error) {
    next(error);
  }
};
```

### 🤖 AI Learning Prompts

- "How do I verify a JWT in Express middleware?"
- "How do I get the user ID from the verified token?"
- "How do I apply auth middleware to only certain routes?"
- "What HTTP status code for authentication vs authorization failures?"

---

## Part 6: Associate Tasks with Users

### Update Tasks Table

```sql
-- Add user_id column
ALTER TABLE tasks ADD COLUMN user_id INTEGER REFERENCES users(id);

-- Or recreate with foreign key
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  text VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT false,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Update Task Controllers

```javascript
exports.create = async (req, res, next) => {
  try {
    const { text } = req.body;
    const { userId } = req.user;  // From auth middleware
    
    const result = await pool.query(
      'INSERT INTO tasks (text, user_id) VALUES ($1, $2) RETURNING *',
      [text, userId]
    );
    
    res.status(201).json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};

exports.getAll = async (req, res, next) => {
  try {
    const { userId } = req.user;
    
    const result = await pool.query(
      'SELECT * FROM tasks WHERE user_id = $1 ORDER BY created_at DESC',
      [userId]
    );
    
    res.json({ tasks: result.rows });
  } catch (error) {
    next(error);
  }
};
```

---

## ✅ Week 7 Milestone Checklist

- [ ] I understand authentication vs authorization
- [ ] I understand why we hash passwords
- [ ] I can hash passwords with bcrypt
- [ ] I understand JWTs and how they work
- [ ] I can create and verify JWTs
- [ ] I have registration and login endpoints
- [ ] I can protect routes with auth middleware
- [ ] Tasks are associated with users
- [ ] Users can only see their own tasks

---

## Next Up

[Week 8: Data Relationships & Advanced Git →](10-week-8.md)
