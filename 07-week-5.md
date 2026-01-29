# Week 5 — Express.js & Project Structure

*Month 2: The Structured Web*

---

## Goals

- Understand why Express exists and what problems it solves
- Learn Express fundamentals: routing, middleware, error handling
- Organize code using MVC architecture
- Use environment variables for configuration
- Refactor your vanilla server to Express

---

## Part 1: Why Express Exists

### What to Learn

- What problems does Express solve?
- What was painful about raw Node.js http?
- What is a web framework?
- What is middleware?

### Problems Express Solves

| Raw Node.js Problem | Express Solution |
|---------------------|------------------|
| Manual URL parsing | Built-in route params |
| Manual body parsing | `express.json()` middleware |
| Repetitive response code | `res.json()`, `res.send()` |
| Manual routing logic | `app.get()`, `app.post()`, etc. |
| No middleware system | Middleware chain |

### 🤖 AI Learning Prompts

- "What problems did developers have with raw Node.js http that led to Express?"
- "Explain middleware in Express like I'm a beginner"
- "What is the request/response cycle in Express?"
- "What is MVC architecture and why use it?"
- "What is the difference between Express and other frameworks like Fastify or Koa?"

---

## Part 2: Express Fundamentals

### Setup

```bash
mkdir express-api
cd express-api
npm init -y
npm install express
```

### The Simplest Express Server

```javascript
const express = require('express');
const app = express();

// Middleware to parse JSON bodies
app.use(express.json());

// Route
app.get('/', (req, res) => {
  res.json({ message: 'Hello World!' });
});

// Start server
app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

### Routing

```javascript
// GET request
app.get('/tasks', (req, res) => {
  res.json({ tasks: [] });
});

// POST request
app.post('/tasks', (req, res) => {
  const { text } = req.body;
  // Create task...
  res.status(201).json(newTask);
});

// Route with parameter
app.get('/tasks/:id', (req, res) => {
  const { id } = req.params;
  // Find task by id...
  res.json(task);
});

// DELETE request
app.delete('/tasks/:id', (req, res) => {
  const { id } = req.params;
  // Delete task...
  res.json({ success: true });
});
```

### Request Object

```javascript
app.post('/tasks', (req, res) => {
  req.body;          // Parsed JSON body
  req.params;        // URL parameters (:id)
  req.query;         // Query string (?sort=date)
  req.headers;       // Request headers
  req.method;        // HTTP method
  req.path;          // URL path
});
```

### Response Object

```javascript
app.get('/example', (req, res) => {
  res.json({ data: 'value' });     // Send JSON
  res.send('Hello');               // Send text/html
  res.status(404).json({});        // Set status + send
  res.redirect('/other');          // Redirect
  res.sendStatus(204);             // Send just status
});
```

### 🤖 AI Learning Prompts

- "Walk me through setting up a basic Express app"
- "What does app.use() do in Express?"
- "Explain Express route parameters vs query parameters"
- "What's the difference between res.send() and res.json()?"

---

## Part 3: Middleware

### What is Middleware?

Middleware are functions that run between receiving a request and sending a response. They can:
- Execute code
- Modify request/response objects
- End the request-response cycle
- Call the next middleware

### Built-in Middleware

```javascript
// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies (forms)
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));
```

### Custom Middleware

```javascript
// Logger middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.path}`);
  next(); // MUST call next() to continue
};

// Apply to all routes
app.use(logger);

// Apply to specific routes
app.get('/tasks', logger, (req, res) => {
  // ...
});
```

### CORS Middleware

```bash
npm install cors
```

```javascript
const cors = require('cors');

// Allow all origins
app.use(cors());

// Or configure
app.use(cors({
  origin: 'http://localhost:5000',
  methods: ['GET', 'POST', 'PATCH', 'DELETE']
}));
```

### Middleware Order Matters!

```javascript
// ✅ Correct order
app.use(express.json());  // Parse body first
app.use(logger);          // Then log
app.get('/tasks', ...);   // Then routes

// ❌ Wrong order
app.get('/tasks', ...);   // Routes won't have parsed body
app.use(express.json());
```

### 🤖 AI Learning Prompts

- "Explain middleware in Express with examples"
- "Why do I need to call next() in middleware?"
- "How does middleware order affect my Express app?"
- "How do I create custom middleware?"

---

## Part 4: Error Handling

### Basic Error Handling

```javascript
app.get('/tasks/:id', (req, res) => {
  const task = tasks.find(t => t.id === parseInt(req.params.id));
  
  if (!task) {
    return res.status(404).json({ error: 'Task not found' });
  }
  
  res.json(task);
});
```

### Error Handling Middleware

```javascript
// Error handling middleware (4 parameters!)
const errorHandler = (err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: 'Something went wrong!' });
};

// Must be LAST
app.use(errorHandler);
```

### Throwing Errors

```javascript
app.get('/tasks/:id', (req, res, next) => {
  try {
    const task = findTask(req.params.id);
    if (!task) {
      const error = new Error('Task not found');
      error.status = 404;
      throw error;
    }
    res.json(task);
  } catch (error) {
    next(error); // Pass to error handler
  }
});

// Error handler
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({ 
    error: err.message 
  });
});
```

### 404 Handler

```javascript
// After all routes, before error handler
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});
```

### 🤖 AI Learning Prompts

- "How does error handling work in Express?"
- "What's special about error handling middleware?"
- "How do I create a catch-all 404 handler in Express?"
- "What's the difference between throwing an error and calling next(error)?"

---

## Part 5: Project Structure

### Why Organize Code?

- Easier to find things
- Easier to test
- Easier for teams
- Separation of concerns

### Recommended Structure

```
project/
├── src/
│   ├── routes/
│   │   └── taskRoutes.js
│   ├── controllers/
│   │   └── taskController.js
│   ├── models/
│   │   └── task.js
│   ├── middleware/
│   │   └── errorHandler.js
│   └── app.js
├── server.js
├── package.json
└── .env
```

### Separating Routes

**src/routes/taskRoutes.js**
```javascript
const express = require('express');
const router = express.Router();
const taskController = require('../controllers/taskController');

router.get('/', taskController.getAll);
router.post('/', taskController.create);
router.get('/:id', taskController.getOne);
router.patch('/:id', taskController.update);
router.delete('/:id', taskController.delete);

module.exports = router;
```

**src/app.js**
```javascript
const express = require('express');
const cors = require('cors');
const taskRoutes = require('./routes/taskRoutes');
const errorHandler = require('./middleware/errorHandler');

const app = express();

app.use(cors());
app.use(express.json());

app.use('/tasks', taskRoutes);

app.use(errorHandler);

module.exports = app;
```

### Controllers

**src/controllers/taskController.js**
```javascript
let tasks = [];
let nextId = 1;

exports.getAll = (req, res) => {
  res.json({ tasks });
};

exports.create = (req, res) => {
  const task = {
    id: nextId++,
    text: req.body.text,
    completed: false,
    createdAt: new Date()
  };
  tasks.push(task);
  res.status(201).json(task);
};

exports.getOne = (req, res) => {
  const task = tasks.find(t => t.id === parseInt(req.params.id));
  if (!task) {
    return res.status(404).json({ error: 'Task not found' });
  }
  res.json(task);
};

exports.update = (req, res) => {
  const task = tasks.find(t => t.id === parseInt(req.params.id));
  if (!task) {
    return res.status(404).json({ error: 'Task not found' });
  }
  if (req.body.completed !== undefined) {
    task.completed = req.body.completed;
  }
  if (req.body.text !== undefined) {
    task.text = req.body.text;
  }
  res.json(task);
};

exports.delete = (req, res) => {
  const index = tasks.findIndex(t => t.id === parseInt(req.params.id));
  if (index === -1) {
    return res.status(404).json({ error: 'Task not found' });
  }
  tasks.splice(index, 1);
  res.json({ success: true });
};
```

### 🤖 AI Learning Prompts

- "What is a good folder structure for an Express app?"
- "What is separation of concerns and why does it matter?"
- "What's the difference between a route and a controller?"
- "Should I put business logic in controllers or models?"

---

## Part 6: Environment Variables

### What to Learn

- What are environment variables?
- Why not hardcode values?
- What is a .env file?
- What should NOT go in code?

### Setup with dotenv

```bash
npm install dotenv
```

**.env** (in project root)
```
PORT=3000
NODE_ENV=development
```

**server.js**
```javascript
require('dotenv').config(); // Load .env file

const app = require('./src/app');

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### What Goes in .env?

| Yes (in .env) | No (in code) |
|--------------|--------------|
| Port numbers | Route paths |
| Database URLs | Business logic |
| API keys | HTML templates |
| JWT secrets | Middleware functions |

### Important: Add to .gitignore!

**.gitignore**
```
node_modules/
.env
```

Create a template:
**.env.example**
```
PORT=3000
NODE_ENV=development
DATABASE_URL=
JWT_SECRET=
```

### 🤖 AI Learning Prompts

- "Explain environment variables for beginners"
- "What should go in a .env file?"
- "Why should I add .env to .gitignore?"
- "How do I use different configs for development vs production?"

---

## Part 7: Refactor Your API to Express

### Project Task

Take your Month 1 vanilla Node.js server and refactor it to Express:

1. Install Express and cors
2. Replace http module with Express
3. Organize into routes and controllers
4. Add proper error handling
5. Use environment variables

### Before (Vanilla Node.js)

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/tasks') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ tasks }));
  }
  // ... lots more code
});
```

### After (Express)

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.get('/tasks', (req, res) => {
  res.json({ tasks });
});
```

---

## ✅ Week 5 Milestone Checklist

- [ ] I understand why Express exists and what problems it solves
- [ ] I can set up an Express application from scratch
- [ ] I understand middleware and how to create custom middleware
- [ ] I can organize code into routes and controllers
- [ ] I understand Express error handling
- [ ] I have refactored my API to use Express
- [ ] I understand environment variables and use .env
- [ ] My .env is in .gitignore

---

## Next Up

[Week 6: Databases & SQL →](08-week-6.md)
