# Month 1 Project: Vanilla Task Tracker

*Full-Stack Application Without Frameworks*

---

## Overview

Build a complete task tracking application using only vanilla technologies:
- HTML, CSS, and JavaScript (no React, no jQuery)
- Node.js with built-in http module (no Express)
- In-memory data storage (no database)

**The goal is to understand what frameworks abstract away.**

---

## Project Structure

```
vanilla-task-tracker/
├── backend/
│   ├── package.json
│   └── server.js
├── frontend/
│   ├── index.html
│   ├── styles.css
│   └── app.js
└── README.md
```

---

## Backend Requirements

### API Endpoints

| Method | Endpoint | Request Body | Response | Status |
|--------|----------|--------------|----------|--------|
| GET | `/tasks` | - | `{ tasks: [...] }` | 200 |
| POST | `/tasks` | `{ text: "..." }` | Created task | 201 |
| PATCH | `/tasks/:id` | `{ completed: true/false }` | Updated task | 200 |
| DELETE | `/tasks/:id` | - | `{ success: true }` | 200 |

### Task Object Shape

```javascript
{
  id: 1,
  text: "Learn JavaScript",
  completed: false,
  createdAt: "2024-01-15T10:30:00.000Z"
}
```

### Technical Requirements

- [ ] Uses Node.js `http` module only (no Express)
- [ ] Proper routing based on method and URL
- [ ] Parses JSON request bodies
- [ ] Returns JSON responses with correct Content-Type
- [ ] Appropriate HTTP status codes
- [ ] CORS headers for frontend access
- [ ] Handles OPTIONS preflight requests
- [ ] 404 for unknown routes
- [ ] Error handling for invalid JSON

### Example Implementation Skeleton

```javascript
const http = require('http');

let tasks = [];
let nextId = 1;

const server = http.createServer(async (req, res) => {
  // CORS Headers
  res.setHeader('Access-Control-Allow-Origin', '*');
  res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PATCH, DELETE, OPTIONS');
  res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
  
  // Handle preflight
  if (req.method === 'OPTIONS') {
    res.writeHead(204);
    res.end();
    return;
  }
  
  res.setHeader('Content-Type', 'application/json');
  
  // Your routing logic here...
});

server.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

---

## Frontend Requirements

### Features

- [ ] Display all tasks from the API
- [ ] Form to add new tasks
- [ ] Checkbox or button to toggle task completion
- [ ] Button to delete tasks
- [ ] Visual distinction for completed tasks (strikethrough, different color)
- [ ] Loading indicator while fetching data
- [ ] Error message display when requests fail

### Technical Requirements

- [ ] Vanilla JavaScript only (no frameworks or libraries)
- [ ] Uses Fetch API for all HTTP requests
- [ ] DOM manipulation for dynamic content
- [ ] Event delegation for dynamically created elements
- [ ] Proper async/await usage
- [ ] Responsive design (looks good on mobile)

### Example HTML Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Task Tracker</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="app">
    <h1>Task Tracker</h1>
    
    <form id="task-form">
      <input 
        type="text" 
        id="task-input" 
        placeholder="What needs to be done?" 
        required
      >
      <button type="submit">Add Task</button>
    </form>
    
    <div id="loading" class="hidden">Loading...</div>
    <div id="error" class="hidden error"></div>
    
    <ul id="task-list">
      <!-- Tasks rendered by JavaScript -->
    </ul>
    
    <div id="stats">
      <span id="task-count">0 tasks</span>
    </div>
  </div>
  
  <script src="app.js"></script>
</body>
</html>
```

### Example JavaScript Structure

```javascript
const API_URL = 'http://localhost:3000';

// State
let tasks = [];

// DOM Elements
const form = document.getElementById('task-form');
const input = document.getElementById('task-input');
const taskList = document.getElementById('task-list');
const loading = document.getElementById('loading');
const error = document.getElementById('error');

// Initialize
document.addEventListener('DOMContentLoaded', loadTasks);

// Event Listeners
form.addEventListener('submit', handleSubmit);
taskList.addEventListener('click', handleTaskClick);

// Functions
async function loadTasks() {
  // Show loading, fetch tasks, render
}

async function handleSubmit(e) {
  // Prevent default, get input, POST to API, re-render
}

async function handleTaskClick(e) {
  // Handle toggle complete or delete based on target
}

function renderTasks() {
  // Clear list, loop through tasks, create DOM elements
}

function showLoading() { /* ... */ }
function hideLoading() { /* ... */ }
function showError(message) { /* ... */ }
function hideError() { /* ... */ }
```

---

## Git Requirements

### Repository Setup

```bash
# Initialize repository
git init

# Create .gitignore
echo "node_modules/" > .gitignore

# Initial commit
git add .
git commit -m "Initial commit: project structure"
```

### Required Branch Strategy

1. `main` - Always working, deployable code
2. Feature branches for each major piece:
   - `feature/backend-setup`
   - `feature/api-endpoints`
   - `feature/frontend-structure`
   - `feature/api-integration`
   - `feature/styling`

### Commit Message Guidelines

```
# Good commit messages
"Add GET /tasks endpoint"
"Add POST /tasks endpoint with validation"
"Create HTML structure for task form"
"Add fetch calls to load tasks on page load"
"Style completed tasks with strikethrough"

# Bad commit messages
"update"
"fix"
"stuff"
"wip"
```

### Expected Commit History (Example)

```
* Add README with setup instructions
* Style completed tasks with strikethrough
* Add loading and error states to UI
* Connect delete button to DELETE endpoint
* Connect toggle complete to PATCH endpoint
* Add fetch call for creating tasks
* Add fetch call for loading tasks
* Create basic HTML and CSS structure
* Add DELETE /tasks/:id endpoint
* Add PATCH /tasks/:id endpoint
* Add POST /tasks endpoint
* Add GET /tasks endpoint
* Set up Node.js server with CORS
* Initial commit: project structure
```

---

## README Requirements

Your README.md should include:

```markdown
# Vanilla Task Tracker

A full-stack task tracking application built without frameworks.

## Tech Stack

- **Frontend:** HTML, CSS, Vanilla JavaScript
- **Backend:** Node.js (http module only)
- **Storage:** In-memory (resets on server restart)

## Features

- Create, read, update, and delete tasks
- Mark tasks as complete
- Responsive design

## Getting Started

### Prerequisites

- Node.js (v18 or higher)

### Installation

1. Clone the repository
   ```bash
   git clone https://github.com/yourusername/vanilla-task-tracker.git
   cd vanilla-task-tracker
   ```

2. Start the backend
   ```bash
   cd backend
   npm install
   node server.js
   ```

3. Start the frontend
   ```bash
   cd frontend
   # Use Live Server in VS Code, or:
   npx serve .
   ```

4. Open http://localhost:5000 (or whatever port)

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /tasks | Get all tasks |
| POST | /tasks | Create a task |
| PATCH | /tasks/:id | Toggle complete |
| DELETE | /tasks/:id | Delete a task |

## What I Learned

- How HTTP requests and responses work
- How to build a REST API from scratch
- How the browser and server communicate
- DOM manipulation and event handling
- Asynchronous JavaScript with async/await
```

---

## Stretch Goals (Optional)

If you finish early, try adding:

- [ ] Filter tasks (All / Active / Completed)
- [ ] Task count display ("3 tasks remaining")
- [ ] Clear all completed button
- [ ] Edit task text (double-click to edit)
- [ ] Due dates for tasks
- [ ] Drag and drop reordering
- [ ] Keyboard shortcuts

---

## Evaluation Checklist

### Functionality
- [ ] Can add new tasks
- [ ] Tasks appear in the list
- [ ] Can mark tasks complete (persists via API)
- [ ] Can delete tasks
- [ ] Data loads from server on page refresh
- [ ] Loading state displays while fetching
- [ ] Errors display when API fails

### Code Quality
- [ ] Clean, readable code
- [ ] Consistent formatting
- [ ] Meaningful variable/function names
- [ ] No unnecessary code duplication
- [ ] Proper error handling

### Git
- [ ] Multiple meaningful commits
- [ ] Used feature branches
- [ ] Clear commit messages
- [ ] .gitignore excludes node_modules

### Documentation
- [ ] README explains project
- [ ] README has setup instructions
- [ ] Code has comments where needed

---

## Submission

When complete:

1. Push all code to GitHub
2. Verify README has setup instructions
3. Test by cloning to a new location and following setup
4. (Optional) Deploy frontend to GitHub Pages / Netlify
5. (Optional) Deploy backend to Railway / Render

---

## Next Steps

After completing this project, you're ready for:

[Week 5: Express.js & Project Structure →](07-week-5.md)
