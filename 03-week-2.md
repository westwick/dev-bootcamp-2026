# Week 2 — JavaScript & The DOM

*Month 1: The Raw Web*

---

## Goals

- Learn JavaScript fundamentals
- Understand the DOM and how to manipulate it
- Learn event handling
- Build an interactive to-do list application
- Use LocalStorage for data persistence

---

## Part 1: JavaScript Fundamentals

### What to Learn

- What is JavaScript? Why was it created?
- Where does JavaScript run? (Browser vs Node.js)
- What is the difference between JavaScript and Java? (Nothing! Just naming confusion)
- How to add JavaScript to an HTML page

### Variables & Data Types

```javascript
// Use const for values that won't change
const PI = 3.14159;

// Use let for values that will change
let count = 0;
count = count + 1;

// Avoid var (old way, has quirks)
```

**Data Types:**
| Type | Example |
|------|---------|
| String | `"Hello"` or `'Hello'` |
| Number | `42` or `3.14` |
| Boolean | `true` or `false` |
| Null | `null` (intentionally empty) |
| Undefined | `undefined` (not yet assigned) |
| Array | `[1, 2, 3]` |
| Object | `{ name: "John", age: 30 }` |

### Operators

```javascript
// Arithmetic
+  -  *  /  %

// Comparison (use === not ==)
===  !==  >  <  >=  <=

// Logical
&&  ||  !
```

### Control Flow

```javascript
// If statements
if (condition) {
  // do something
} else if (otherCondition) {
  // do something else
} else {
  // default
}

// Ternary operator
const result = condition ? valueIfTrue : valueIfFalse;
```

### Loops

```javascript
// For loop
for (let i = 0; i < 10; i++) {
  console.log(i);
}

// For...of (arrays)
for (const item of array) {
  console.log(item);
}

// While loop
while (condition) {
  // do something
}
```

### Functions

```javascript
// Function declaration
function greet(name) {
  return `Hello, ${name}!`;
}

// Arrow function (modern)
const greet = (name) => {
  return `Hello, ${name}!`;
};

// Arrow function (shorthand for single expression)
const greet = (name) => `Hello, ${name}!`;
```

### Arrays

```javascript
const fruits = ['apple', 'banana', 'orange'];

// Access elements
fruits[0]; // 'apple'

// Common methods
fruits.push('grape');     // Add to end
fruits.pop();             // Remove from end
fruits.length;            // Get length

// Iteration methods (very important!)
fruits.forEach(fruit => console.log(fruit));
fruits.map(fruit => fruit.toUpperCase());
fruits.filter(fruit => fruit.length > 5);
fruits.find(fruit => fruit === 'banana');
```

### Objects

```javascript
const person = {
  name: 'John',
  age: 30,
  greet() {
    return `Hi, I'm ${this.name}`;
  }
};

// Access properties
person.name;        // 'John'
person['name'];     // 'John'

// Add/modify properties
person.email = 'john@example.com';
```

### 🤖 AI Learning Prompts

- "Explain JavaScript variables to someone who's never programmed"
- "What's the difference between let, const, and var?"
- "Why should I use === instead of ==?"
- "Explain arrow functions vs regular functions"
- "What does 'this' mean in JavaScript?"
- "Explain map, filter, and reduce with simple examples"
- "What is scope in JavaScript?"

---

## Part 2: The DOM (Document Object Model)

### What to Learn

- What is the DOM exactly?
- How does JavaScript "see" your HTML?
- What is a node? What is an element?
- How does the browser build the DOM?

### DOM Selection Methods

```javascript
// By ID (returns single element)
const header = document.getElementById('header');

// By CSS selector (returns first match)
const button = document.querySelector('.submit-btn');

// By CSS selector (returns all matches)
const paragraphs = document.querySelectorAll('p');
```

### DOM Manipulation

```javascript
// Get/set text content
element.textContent = 'New text';

// Get/set HTML (be careful with security!)
element.innerHTML = '<strong>Bold text</strong>';

// Get/set attributes
element.setAttribute('href', 'https://example.com');
element.getAttribute('href');

// Modify CSS classes
element.classList.add('active');
element.classList.remove('active');
element.classList.toggle('active');

// Modify styles (avoid when possible, use classes)
element.style.color = 'red';
```

### Creating Elements

```javascript
// Create a new element
const newDiv = document.createElement('div');
newDiv.textContent = 'Hello!';
newDiv.classList.add('greeting');

// Add to the page
document.body.appendChild(newDiv);

// Remove from page
newDiv.remove();
// or
parentElement.removeChild(newDiv);
```

### 🤖 AI Learning Prompts

- "Explain the DOM like I've never heard of it before"
- "What's the difference between querySelector and getElementById?"
- "Why is innerHTML potentially dangerous?"
- "How do I add a new element to the page with JavaScript?"
- "Explain the difference between textContent and innerHTML"

---

## Part 3: Events

### What to Learn

- What is an event?
- What is event-driven programming?
- What is an event listener?
- What is the event object?
- What is event bubbling and capturing?

### Common Events

| Event | Triggered When |
|-------|---------------|
| `click` | Mouse click |
| `submit` | Form submission |
| `input` | Input field changes (immediate) |
| `change` | Input loses focus after change |
| `keydown` | Key is pressed |
| `keyup` | Key is released |
| `mouseover` | Mouse enters element |
| `mouseout` | Mouse leaves element |
| `DOMContentLoaded` | HTML fully loaded |

### Adding Event Listeners

```javascript
// Basic pattern
element.addEventListener('click', function(event) {
  // handle the event
  console.log('Button clicked!');
});

// With arrow function
element.addEventListener('click', (event) => {
  console.log('Button clicked!');
});

// Named function (useful for removing later)
function handleClick(event) {
  console.log('Button clicked!');
}
element.addEventListener('click', handleClick);
```

### The Event Object

```javascript
element.addEventListener('click', (event) => {
  event.target;           // Element that triggered event
  event.preventDefault(); // Stop default behavior
  event.stopPropagation(); // Stop bubbling
});
```

### Form Events

```javascript
const form = document.querySelector('form');

form.addEventListener('submit', (event) => {
  event.preventDefault(); // Stop page reload!
  
  // Get form data
  const formData = new FormData(form);
  const email = formData.get('email');
  
  // Or access inputs directly
  const emailInput = document.querySelector('#email');
  const email = emailInput.value;
});
```

### 🤖 AI Learning Prompts

- "Explain event listeners in JavaScript for beginners"
- "What is event.preventDefault() and when do I use it?"
- "Explain event bubbling with an example"
- "What's the difference between event.target and this?"
- "Why use addEventListener instead of onclick?"

---

## Part 4: Working with Forms in JavaScript

### What to Learn

- How to get values from form inputs
- How to validate form data
- How to prevent form submission and handle it with JavaScript
- How to show error messages

### Getting Form Values

```javascript
// Method 1: FormData API
const form = document.querySelector('form');
const formData = new FormData(form);
const email = formData.get('email');

// Method 2: Direct access
const emailInput = document.querySelector('#email');
const email = emailInput.value;

// Method 3: Form elements
const email = form.elements.email.value;
```

### Basic Validation

```javascript
form.addEventListener('submit', (event) => {
  event.preventDefault();
  
  const email = form.elements.email.value;
  
  if (!email) {
    showError('Email is required');
    return;
  }
  
  if (!email.includes('@')) {
    showError('Please enter a valid email');
    return;
  }
  
  // Form is valid, proceed
  submitForm();
});
```

### 🤖 AI Learning Prompts

- "How do I get form input values in JavaScript?"
- "Explain FormData API"
- "How do I validate an email address in JavaScript?"
- "How do I show error messages below form fields?"

---

## Part 5: LocalStorage

### What to Learn

- What is the Web Storage API?
- What is localStorage vs sessionStorage?
- What are the limitations?
- What is JSON and why do you need it for storage?

### LocalStorage Methods

```javascript
// Save a string
localStorage.setItem('username', 'john');

// Retrieve a string
const username = localStorage.getItem('username');

// Remove an item
localStorage.removeItem('username');

// Clear everything
localStorage.clear();
```

### Storing Objects and Arrays

LocalStorage only stores strings, so you need JSON:

```javascript
// Saving an array
const tasks = [
  { id: 1, text: 'Learn JS', completed: false },
  { id: 2, text: 'Build app', completed: false }
];
localStorage.setItem('tasks', JSON.stringify(tasks));

// Loading an array
const saved = localStorage.getItem('tasks');
const tasks = saved ? JSON.parse(saved) : [];
```

### 🤖 AI Learning Prompts

- "Explain localStorage in simple terms"
- "Why do I need JSON.stringify and JSON.parse with localStorage?"
- "What's the difference between localStorage and sessionStorage?"
- "What are the limitations of localStorage?"

---

## Part 6: Build — Interactive To-Do List

### Project Requirements

Build a to-do list application that:

- [ ] Lets users type a task and add it
- [ ] Displays all tasks in a list
- [ ] Lets users mark tasks as complete (toggle)
- [ ] Lets users delete tasks
- [ ] Saves tasks to localStorage
- [ ] Loads tasks when page opens

### Technical Requirements

- Pure JavaScript (no libraries)
- Event listeners for all interactions
- DOM manipulation to display tasks
- LocalStorage for persistence
- CSS for styling (including completed task style)

### Suggested HTML Structure

```html
<div class="app">
  <h1>My Tasks</h1>
  
  <form id="task-form">
    <input type="text" id="task-input" placeholder="Enter a task..." required>
    <button type="submit">Add Task</button>
  </form>
  
  <ul id="task-list">
    <!-- Tasks will be added here by JavaScript -->
  </ul>
</div>
```

### Git Workflow

Create a new branch for this feature:

```bash
git checkout -b feature/todo-app
```

Suggested commits:
```
"Add HTML structure for todo app"
"Add CSS styling for todo list"
"Add JavaScript: display static tasks"
"Add functionality to add new tasks"
"Add functionality to mark tasks complete"
"Add functionality to delete tasks"
"Add localStorage: save tasks"
"Add localStorage: load tasks on page load"
```

### 🤖 AI Prompts for Building

- "What's a good HTML structure for a to-do list app?"
- "How do I add a new list item to the DOM when form is submitted?"
- "How do I handle click events on dynamically created elements?"
- "How do I toggle a 'completed' class on and off?"
- "How do I make sure my tasks save to localStorage after every change?"
- "How do I generate unique IDs for each task?"

---

## ✅ Week 2 Milestone Checklist

- [ ] I understand JavaScript data types and variables
- [ ] I can write functions with parameters and return values
- [ ] I can use array methods like map, filter, forEach
- [ ] I understand what the DOM is and how to select elements
- [ ] I can create, modify, and remove DOM elements
- [ ] I understand events and event listeners
- [ ] I can handle form submissions with JavaScript
- [ ] I can use localStorage to persist data
- [ ] I have a working to-do app with full CRUD (Create, Read, Update, Delete)
- [ ] My commits show clear progression of features

---

## Next Up

[Week 3: Build a Server FROM SCRATCH →](04-week-3.md)
