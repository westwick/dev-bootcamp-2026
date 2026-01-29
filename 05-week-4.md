# Week 4 — Frontend ↔ Backend Integration

*Month 1: The Raw Web*

---

## Goals

- Connect your frontend to your backend
- Understand the Fetch API and async JavaScript
- Learn about CORS and why it exists
- Master Git branching fundamentals
- Complete the Month 1 project

---

## Part 1: The Fetch API

### What to Learn

- What is the Fetch API?
- What is a Promise?
- What is async/await?
- How to make HTTP requests from the browser

### Basic Fetch (Promises)

```javascript
// GET request
fetch('http://localhost:3000/tasks')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### Fetch with async/await (Recommended)

```javascript
// GET request
async function getTasks() {
  try {
    const response = await fetch('http://localhost:3000/tasks');
    const tasks = await response.json();
    console.log(tasks);
  } catch (error) {
    console.error('Error:', error);
  }
}

// POST request
async function createTask(text) {
  try {
    const response = await fetch('http://localhost:3000/tasks', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ text })
    });
    const newTask = await response.json();
    return newTask;
  } catch (error) {
    console.error('Error:', error);
  }
}

// DELETE request
async function deleteTask(id) {
  try {
    await fetch(`http://localhost:3000/tasks/${id}`, {
      method: 'DELETE'
    });
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### Handling Response Status

```javascript
async function getTasks() {
  const response = await fetch('http://localhost:3000/tasks');
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  const tasks = await response.json();
  return tasks;
}
```

### 🤖 AI Learning Prompts

- "Explain the Fetch API for beginners"
- "Explain Promises like I'm new to programming"
- "What is async/await and how does it relate to Promises?"
- "How do I send a POST request with JSON data using fetch?"
- "How do I handle errors with fetch?"
- "What's the difference between fetch and XMLHttpRequest?"

---

## Part 2: CORS (Cross-Origin Resource Sharing)

### What to Learn

- What is the Same-Origin Policy?
- What is CORS and why does it exist?
- Why do you get CORS errors?
- How to enable CORS on your server

### The Problem

When your frontend (localhost:5500) tries to fetch from your backend (localhost:3000), you'll see:

```
Access to fetch at 'http://localhost:3000/tasks' from origin 'http://localhost:5500' 
has been blocked by CORS policy
```

### Why CORS Exists

- Security feature to prevent malicious websites from making requests on your behalf
- Browsers block requests to different origins by default
- Server must explicitly allow other origins

### Adding CORS Headers to Your Server

```javascript
const server = http.createServer((req, res) => {
  // Handle preflight requests
  if (req.method === 'OPTIONS') {
    res.writeHead(204, {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type'
    });
    res.end();
    return;
  }
  
  // Set CORS headers for all responses
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Content-Type', 'application/json');
  
  // ... rest of your routes
});
```

### 🤖 AI Learning Prompts

- "Explain CORS like I'm a beginner"
- "Why do I get CORS errors when my frontend tries to fetch from my backend?"
- "What is a preflight OPTIONS request?"
- "How do I add CORS headers to a Node.js http server?"
- "What's the difference between allowing all origins (*) vs specific origins?"

---

## Part 3: Connecting Frontend to Backend

### What to Learn

- How to structure your code for API calls
- How to update the UI after API calls
- How to handle loading and error states
- How to remove localStorage dependency

### Updating Your To-Do App

Replace localStorage with API calls:

```javascript
// Before (localStorage)
function loadTasks() {
  const saved = localStorage.getItem('tasks');
  return saved ? JSON.parse(saved) : [];
}

// After (API)
async function loadTasks() {
  const response = await fetch('http://localhost:3000/tasks');
  const data = await response.json();
  return data.tasks;
}
```

### Complete Integration Pattern

```javascript
const API_URL = 'http://localhost:3000';

// Load and display tasks on page load
async function init() {
  try {
    const response = await fetch(`${API_URL}/tasks`);
    const data = await response.json();
    displayTasks(data.tasks);
  } catch (error) {
    showError('Failed to load tasks');
  }
}

// Add a new task
async function addTask(text) {
  try {
    const response = await fetch(`${API_URL}/tasks`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ text })
    });
    const newTask = await response.json();
    
    // Re-fetch all tasks or add to DOM directly
    init();
  } catch (error) {
    showError('Failed to add task');
  }
}

// Delete a task
async function deleteTask(id) {
  try {
    await fetch(`${API_URL}/tasks/${id}`, {
      method: 'DELETE'
    });
    
    // Remove from DOM or re-fetch
    init();
  } catch (error) {
    showError('Failed to delete task');
  }
}

// Run when page loads
document.addEventListener('DOMContentLoaded', init);
```

### 🤖 AI Prompts for Building

- "How do I show a loading state while fetching data?"
- "How do I display an error message if the fetch fails?"
- "How do I update the UI after successfully adding a task?"
- "How do I run my server and open my frontend at the same time?"
- "Should I re-fetch all data or just update the DOM after changes?"

---

## Part 4: Git Branching Fundamentals

### What to Learn

- Why do we use branches?
- What is the main/master branch?
- What is a feature branch?
- How to merge branches
- What is a merge conflict?

### Why Branches?

- Work on features without affecting main code
- Multiple people can work simultaneously
- Easy to abandon failed experiments
- Keep main branch always deployable

### Essential Commands

```bash
# See all branches (current branch has *)
git branch

# Create a new branch
git branch feature/login

# Switch to a branch
git checkout feature/login

# Create AND switch (shortcut)
git checkout -b feature/login

# See which branch you're on
git status
```

### Basic Workflow

```bash
# 1. Start from main, make sure it's up to date
git checkout main
git pull

# 2. Create feature branch
git checkout -b feature/add-api-calls

# 3. Do your work, make commits
git add .
git commit -m "Add fetch calls for tasks API"

# 4. When done, switch back to main
git checkout main

# 5. Merge your feature
git merge feature/add-api-calls

# 6. Delete the feature branch (optional)
git branch -d feature/add-api-calls

# 7. Push to GitHub
git push
```

### Handling Merge Conflicts

When the same lines were changed in both branches:

```
<<<<<<< HEAD
const API_URL = 'http://localhost:3000';
=======
const API_URL = 'http://localhost:8080';
>>>>>>> feature/change-port
```

**To resolve:**
1. Open the file
2. Choose which version to keep (or combine them)
3. Remove the conflict markers (`<<<<`, `====`, `>>>>`)
4. Save and commit

### 🤖 AI Learning Prompts

- "Explain Git branches like I'm new to version control"
- "What is a feature branch and why use one?"
- "How do I resolve a merge conflict?"
- "What's the difference between merge and rebase?"
- "When should I create a new branch?"

---

## Part 5: Running Both Servers

### The Challenge

You need to run:
1. Your backend server (Node.js on port 3000)
2. Your frontend server (Live Server or similar)

### Option 1: Two Terminal Windows

```bash
# Terminal 1: Backend
cd backend
node server.js

# Terminal 2: Frontend
cd frontend
# Use VS Code Live Server or:
npx serve .
```

### Option 2: VS Code Split Terminal

- Open integrated terminal (Ctrl+`)
- Click the "+" to add another terminal
- Run each server in its own terminal

### Option 3: Using npm scripts

In `package.json`:
```json
{
  "scripts": {
    "server": "node server.js",
    "client": "npx serve ./frontend",
    "dev": "npm run server & npm run client"
  }
}
```

---

## 🧩 Month 1 Project: Vanilla Task Tracker

### Final Requirements

**Frontend:**
- [ ] HTML/CSS/JavaScript (no frameworks)
- [ ] Form to add new tasks
- [ ] Display list of tasks
- [ ] Mark tasks complete
- [ ] Delete tasks
- [ ] Loading states while fetching
- [ ] Error messages when requests fail

**Backend:**
- [ ] Node.js HTTP server (no Express)
- [ ] GET /tasks - list all tasks
- [ ] POST /tasks - create task
- [ ] PATCH /tasks/:id - toggle complete
- [ ] DELETE /tasks/:id - delete task
- [ ] CORS headers enabled

**Git:**
- [ ] Meaningful commit history
- [ ] Used feature branches
- [ ] README with setup instructions

### What This Project Proves

You understand:
- How HTTP requests and responses work at a low level
- How the browser and server communicate
- How to build an API from scratch
- How JavaScript manipulates the DOM
- How to handle asynchronous operations
- Basic version control workflow

---

## ✅ Week 4 / Month 1 Final Checklist

- [ ] I understand async/await and Promises
- [ ] I can use the Fetch API to make HTTP requests
- [ ] I understand CORS and can enable it on my server
- [ ] My frontend successfully communicates with my backend
- [ ] I can create and merge Git branches
- [ ] I can handle merge conflicts
- [ ] I have a complete full-stack task tracker application
- [ ] My project has a proper README

---

## What You've Accomplished

By completing Month 1, you now understand what frameworks like Express and React abstract away. You've built:

- A multi-page website from scratch
- An interactive frontend application
- A REST API server from scratch
- A connected full-stack application

**This is foundational knowledge that many developers skip. You didn't.**

---

## Next Up

[Month 1 Project Requirements →](06-month-1-project.md)

or

[Week 5: Express.js & Project Structure →](07-week-5.md)
