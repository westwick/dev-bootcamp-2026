# Week 3 — Build a Server FROM SCRATCH

*Month 1: The Raw Web*

---

## Goals

- Understand what a server is and does
- Learn Node.js fundamentals
- Build an HTTP server using only built-in modules
- Create a REST API from scratch
- Understand the request/response cycle deeply

---

## Part 1: What is a Server?

### What to Learn

- What is a server? (It's just a computer program!)
- What is client-server architecture?
- What is a request and what is a response?
- What are HTTP methods? (GET, POST, PUT, DELETE, PATCH)
- What is a port?
- What is localhost?

### Key Concepts

| Term | Description |
|------|-------------|
| Server | A program that listens for requests and sends responses |
| Client | The program making requests (browser, mobile app, etc.) |
| Port | A number that identifies a specific process (like 3000, 8080) |
| localhost | Your own computer (127.0.0.1) |
| Request | Data sent TO the server |
| Response | Data sent FROM the server |

### HTTP Methods

| Method | Purpose | Example |
|--------|---------|---------|
| GET | Retrieve data | Get all tasks |
| POST | Create new data | Add a new task |
| PUT | Replace data entirely | Replace a task |
| PATCH | Update part of data | Mark task complete |
| DELETE | Remove data | Delete a task |

### 🤖 AI Learning Prompts

- "Explain what a server is like I'm 10 years old"
- "What happens step by step when a browser makes a request to a server?"
- "What's the difference between GET and POST?"
- "What is localhost and why is it 127.0.0.1?"
- "What is a port and why are there different port numbers?"

---

## Part 2: Node.js Fundamentals

### What to Learn

- What is Node.js? Why does it exist?
- What is the difference between browser JavaScript and Node.js?
- What is npm? (Node Package Manager)
- What is package.json?
- What are modules? What is require/import?

### Installing Node.js

1. Download from nodejs.org (LTS version)
2. Verify installation:
```bash
node --version
npm --version
```

### Running JavaScript with Node

```bash
# Run a file
node filename.js

# Start interactive mode (REPL)
node
```

### npm Basics

```bash
# Create package.json (use -y for defaults)
npm init
npm init -y

# Install a package
npm install packagename

# Install all packages from package.json
npm install

# Install as dev dependency
npm install --save-dev packagename
```

### Understanding package.json

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My first Node project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "eslint": "^8.0.0"
  }
}
```

### Important Files

| File/Folder | Purpose | Git? |
|-------------|---------|------|
| `package.json` | Project manifest | ✅ Commit |
| `package-lock.json` | Exact versions | ✅ Commit |
| `node_modules/` | Installed packages | ❌ .gitignore |

### 🤖 AI Learning Prompts

- "What is Node.js and why was it created?"
- "What's the difference between running JS in a browser vs Node.js?"
- "Explain npm and package.json for beginners"
- "Why do I add node_modules to .gitignore?"
- "What is the difference between dependencies and devDependencies?"

---

## Part 3: Build an HTTP Server (No Express!)

### What to Learn

- How to create a basic HTTP server with Node's built-in `http` module
- How to read the request URL and method
- How to send a response
- How to set response headers
- How to handle different routes manually

### The Simplest Server

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  // Log what we received
  console.log(`${request.method} ${request.url}`);
  
  // Send a response
  response.writeHead(200, { 'Content-Type': 'text/plain' });
  response.end('Hello World!');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000');
});
```

### Understanding the Request Object

```javascript
const server = http.createServer((req, res) => {
  console.log(req.method);  // GET, POST, etc.
  console.log(req.url);     // /tasks, /tasks/123, etc.
  console.log(req.headers); // Object with all headers
});
```

### Sending Different Responses

```javascript
// Plain text
response.writeHead(200, { 'Content-Type': 'text/plain' });
response.end('Hello World!');

// JSON
response.writeHead(200, { 'Content-Type': 'application/json' });
response.end(JSON.stringify({ message: 'Hello!' }));

// HTML
response.writeHead(200, { 'Content-Type': 'text/html' });
response.end('<h1>Hello World!</h1>');

// Error
response.writeHead(404, { 'Content-Type': 'application/json' });
response.end(JSON.stringify({ error: 'Not found' }));
```

### 🤖 AI Learning Prompts

- "Explain the Node.js http module for beginners"
- "What is the request object and what information does it contain?"
- "What is the response object and how do I use it?"
- "What are HTTP headers and why do I set Content-Type?"
- "How do I handle different URL paths without Express?"

---

## Part 4: Routing Manually

### What to Learn

- How to route requests to different handlers
- How to extract parts of the URL
- How to parse query parameters
- How to handle unknown routes (404)

### Basic Router Pattern

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  const { method, url } = req;
  
  // Set default headers for JSON
  res.setHeader('Content-Type', 'application/json');
  
  // Route: GET /
  if (method === 'GET' && url === '/') {
    res.writeHead(200);
    res.end(JSON.stringify({ message: 'Welcome to the API' }));
    return;
  }
  
  // Route: GET /tasks
  if (method === 'GET' && url === '/tasks') {
    res.writeHead(200);
    res.end(JSON.stringify({ tasks: [] }));
    return;
  }
  
  // Route not found
  res.writeHead(404);
  res.end(JSON.stringify({ error: 'Not found' }));
});
```

### Extracting URL Parameters

```javascript
// For URL like /tasks/123
const url = req.url; // '/tasks/123'
const parts = url.split('/'); // ['', 'tasks', '123']
const id = parts[2]; // '123'

// Or use URL API
const parsedUrl = new URL(req.url, `http://${req.headers.host}`);
const pathname = parsedUrl.pathname; // '/tasks/123'
const searchParams = parsedUrl.searchParams; // URLSearchParams
```

---

## Part 5: Handling Request Bodies

### What to Learn

- Why request bodies come as streams
- How to collect and parse body data
- How to handle JSON bodies

### Reading the Request Body

```javascript
const server = http.createServer((req, res) => {
  if (req.method === 'POST' && req.url === '/tasks') {
    let body = '';
    
    // Collect data chunks
    req.on('data', (chunk) => {
      body += chunk;
    });
    
    // All data received
    req.on('end', () => {
      try {
        const data = JSON.parse(body);
        console.log('Received:', data);
        
        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ success: true, data }));
      } catch (error) {
        res.writeHead(400, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ error: 'Invalid JSON' }));
      }
    });
    
    return;
  }
  
  // ... other routes
});
```

### 🤖 AI Learning Prompts

- "Why does the request body come as a stream in Node.js?"
- "How do I read the request body in Node.js http server?"
- "How do I parse JSON from a request body?"
- "How do I send JSON as a response?"
- "What HTTP status code should I use when creating a resource?"

---

## Part 6: Build — Task API Server

### Project Requirements

Build a REST API server with these endpoints:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/tasks` | Returns all tasks as JSON |
| POST | `/tasks` | Creates a new task from JSON body |
| DELETE | `/tasks/:id` | Deletes a task by ID |

Tasks should be stored in a JavaScript array (in-memory).

### Data Structure

```javascript
let tasks = [
  { id: 1, text: 'Learn Node.js', completed: false },
  { id: 2, text: 'Build an API', completed: false }
];

let nextId = 3;
```

### Technical Requirements

- [ ] Use only Node.js built-in `http` module
- [ ] Parse URL and method to route requests
- [ ] Parse JSON request bodies
- [ ] Send JSON responses
- [ ] Proper status codes (200, 201, 404, etc.)
- [ ] Handle errors gracefully

### Testing Your API

**Using curl (terminal):**
```bash
# GET all tasks
curl http://localhost:3000/tasks

# POST a new task
curl -X POST http://localhost:3000/tasks \
  -H "Content-Type: application/json" \
  -d '{"text": "Learn Node"}'

# DELETE a task
curl -X DELETE http://localhost:3000/tasks/1
```

**Using Postman or Insomnia:**
- Download Postman or Insomnia
- Create requests for each endpoint
- See the responses visually

### Git Workflow

```bash
git checkout -b feature/api-server
```

Suggested commits:
```
"Initialize Node project with package.json"
"Create basic HTTP server"
"Add GET /tasks endpoint"
"Add POST /tasks endpoint"
"Add DELETE /tasks/:id endpoint"
"Add error handling for invalid routes"
"Add error handling for invalid JSON"
```

### 🤖 AI Prompts for Building

- "How do I get the URL path and HTTP method from a Node.js request?"
- "How do I route to different handlers based on URL and method?"
- "How do I extract an ID from a URL like /tasks/123?"
- "How do I handle a request for a task that doesn't exist?"
- "What HTTP status code should I use when creating a resource?"
- "How do I handle errors when parsing JSON?"

---

## ✅ Week 3 Milestone Checklist

- [ ] I can explain what a server is and does
- [ ] I understand HTTP methods and when to use each
- [ ] I have Node.js and npm installed and working
- [ ] I understand what package.json is for
- [ ] I can create a basic HTTP server with Node.js
- [ ] I understand how to parse the request URL, method, and body
- [ ] I have a working REST API with GET, POST, DELETE
- [ ] I can test my API with curl or Postman
- [ ] I understand why data resets when server restarts (in-memory storage)

---

## Next Up

[Week 4: Frontend ↔ Backend Integration →](05-week-4.md)
