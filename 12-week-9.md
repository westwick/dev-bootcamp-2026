# Week 9 — React Fundamentals

*Month 3: The Modern Web*

---

## Goals

- Understand why React exists
- Learn component-based architecture
- Master props and state
- Learn React hooks (useState, useEffect)
- Build a React frontend for your API

---

## Part 1: Why React Exists

### What to Learn

- What problems does React solve?
- What was painful about vanilla JavaScript for complex UIs?
- What is component-based architecture?
- What is the Virtual DOM?
- What is declarative vs imperative?

### Problems React Solves

| Vanilla JS Problem | React Solution |
|-------------------|----------------|
| Manual DOM updates | Automatic re-rendering |
| State scattered everywhere | Centralized state |
| Complex event handling | Declarative event handlers |
| Hard to reuse UI | Reusable components |
| UI and state out of sync | React keeps them in sync |

### Declarative vs Imperative

```javascript
// Imperative (vanilla JS) - HOW to do it
const list = document.getElementById('list');
const item = document.createElement('li');
item.textContent = 'New item';
list.appendChild(item);

// Declarative (React) - WHAT you want
function List({ items }) {
  return (
    <ul>
      {items.map(item => <li key={item.id}>{item.text}</li>)}
    </ul>
  );
}
```

### 🤖 AI Learning Prompts

- "Why was React invented? What problems did it solve?"
- "What made managing complex UIs hard before React?"
- "Explain the Virtual DOM like I'm a beginner"
- "What is declarative vs imperative UI code?"
- "What is component-based architecture?"

---

## Part 2: Setting Up React

### Create a New React Project

```bash
# Using Vite (recommended)
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install
npm run dev
```

### Project Structure

```
my-react-app/
├── src/
│   ├── App.jsx        # Main component
│   ├── main.jsx       # Entry point
│   └── index.css      # Global styles
├── public/            # Static files
├── index.html         # HTML template
├── package.json
└── vite.config.js
```

### 🤖 AI Learning Prompts

- "What is Vite and why use it over Create React App?"
- "What does npm run build do for a React app?"
- "What is a good folder structure for a React project?"

---

## Part 3: JSX

### What to Learn

- What is JSX?
- How is it different from HTML?
- How to embed JavaScript in JSX
- JSX rules and gotchas

### JSX Basics

```jsx
// JSX looks like HTML but is JavaScript
const element = <h1>Hello, World!</h1>;

// Embed JavaScript with curly braces
const name = 'John';
const greeting = <h1>Hello, {name}!</h1>;

// Expressions work too
const element = <p>2 + 2 = {2 + 2}</p>;

// Use className, not class
const element = <div className="container">...</div>;

// Self-closing tags must close
const element = <img src="..." alt="..." />;

// Wrap multiple elements in a parent
const element = (
  <div>
    <h1>Title</h1>
    <p>Content</p>
  </div>
);

// Or use fragments
const element = (
  <>
    <h1>Title</h1>
    <p>Content</p>
  </>
);
```

### 🤖 AI Learning Prompts

- "Explain JSX and why it exists"
- "What are the differences between JSX and HTML?"
- "Why do I need to wrap multiple elements?"
- "What are React fragments and when do I use them?"

---

## Part 4: Components

### What to Learn

- What is a component?
- Function components vs class components
- How to create and use components
- Component naming conventions

### Function Components

```jsx
// Simple component
function Welcome() {
  return <h1>Hello!</h1>;
}

// Arrow function (also works)
const Welcome = () => {
  return <h1>Hello!</h1>;
};

// Using the component
function App() {
  return (
    <div>
      <Welcome />
      <Welcome />
    </div>
  );
}
```

### Component Files

**src/components/Welcome.jsx**
```jsx
function Welcome() {
  return <h1>Welcome!</h1>;
}

export default Welcome;
```

**src/App.jsx**
```jsx
import Welcome from './components/Welcome';

function App() {
  return (
    <div>
      <Welcome />
    </div>
  );
}

export default App;
```

### 🤖 AI Learning Prompts

- "What is a React component?"
- "What's the difference between function and class components?"
- "How do I organize components in files?"
- "What naming conventions should I use for components?"

---

## Part 5: Props

### What to Learn

- What are props?
- How to pass props
- Props are read-only
- Children prop

### Passing Props

```jsx
// Parent passes props
function App() {
  return (
    <div>
      <Welcome name="Alice" />
      <Welcome name="Bob" />
    </div>
  );
}

// Child receives props
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Or with destructuring (preferred)
function Welcome({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

### Props Examples

```jsx
// Multiple props
<UserCard name="John" email="john@example.com" age={30} />

function UserCard({ name, email, age }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{email}</p>
      <p>Age: {age}</p>
    </div>
  );
}

// Default props
function Button({ text = 'Click me', onClick }) {
  return <button onClick={onClick}>{text}</button>;
}
```

### Children Prop

```jsx
// Using children
<Card>
  <h2>Title</h2>
  <p>Content goes here</p>
</Card>

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

### 🤖 AI Learning Prompts

- "Explain React props for beginners"
- "Why are props read-only?"
- "How do I pass multiple props to a component?"
- "What is the children prop in React?"

---

## Part 6: State

### What to Learn

- What is state?
- How is state different from props?
- How to use useState
- When to use state

### useState Hook

```jsx
import { useState } from 'react';

function Counter() {
  // Declare state variable
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

### State Rules

```jsx
// ✅ Use setState function to update
setCount(count + 1);

// ❌ Never mutate state directly
count = count + 1;  // WRONG!

// ✅ For objects, create new object
setUser({ ...user, name: 'New Name' });

// ❌ Don't mutate
user.name = 'New Name';  // WRONG!

// ✅ For arrays, create new array
setItems([...items, newItem]);

// ❌ Don't mutate
items.push(newItem);  // WRONG!
```

### Props vs State

| Props | State |
|-------|-------|
| Passed from parent | Managed internally |
| Read-only | Can be changed |
| External configuration | Internal data |

### 🤖 AI Learning Prompts

- "Explain React state for beginners"
- "What's the difference between props and state?"
- "Why can't I mutate state directly?"
- "How do I update state that is an object or array?"

---

## Part 7: useEffect

### What to Learn

- What are side effects?
- How to use useEffect
- Effect dependencies
- Cleanup functions

### useEffect Basics

```jsx
import { useState, useEffect } from 'react';

function Example() {
  const [data, setData] = useState(null);
  
  // Runs after every render
  useEffect(() => {
    console.log('Component rendered');
  });
  
  // Runs only once (on mount)
  useEffect(() => {
    console.log('Component mounted');
  }, []);
  
  // Runs when dependency changes
  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]);
  
  return <div>...</div>;
}
```

### Fetching Data

```jsx
function TaskList() {
  const [tasks, setTasks] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    async function fetchTasks() {
      try {
        const response = await fetch('http://localhost:3000/tasks');
        const data = await response.json();
        setTasks(data.tasks);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }
    
    fetchTasks();
  }, []); // Empty array = run once
  
  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error}</p>;
  
  return (
    <ul>
      {tasks.map(task => (
        <li key={task.id}>{task.text}</li>
      ))}
    </ul>
  );
}
```

### 🤖 AI Learning Prompts

- "Explain useEffect in React"
- "What is the dependency array in useEffect?"
- "How do I fetch data from an API in React?"
- "What are common mistakes with useEffect?"

---

## Part 8: Build - React UI for Your API

### Project Task

Create a React frontend that connects to your Month 2 API:

1. Login/Register pages
2. Projects list
3. Tasks within a project
4. Add/edit/delete functionality

### Suggested Component Structure

```
src/
├── components/
│   ├── common/
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   └── Spinner.jsx
│   ├── auth/
│   │   ├── LoginForm.jsx
│   │   └── RegisterForm.jsx
│   ├── projects/
│   │   ├── ProjectList.jsx
│   │   └── ProjectCard.jsx
│   └── tasks/
│       ├── TaskList.jsx
│       └── TaskItem.jsx
├── pages/
│   ├── LoginPage.jsx
│   ├── RegisterPage.jsx
│   ├── ProjectsPage.jsx
│   └── ProjectDetailPage.jsx
├── App.jsx
└── main.jsx
```

### 🤖 AI Prompts for Building

- "How do I structure a React project with multiple pages?"
- "How do I store a JWT token in React?"
- "How do I pass the token with every fetch request?"
- "How do I redirect after login in React?"

---

## ✅ Week 9 Milestone Checklist

- [ ] I understand why React exists
- [ ] I can create components with JSX
- [ ] I understand props and how to pass them
- [ ] I understand state and useState
- [ ] I can use useEffect to fetch data
- [ ] I have a React app connected to my API
- [ ] I can display data from my backend

---

## Next Up

[Week 10: Modern Frontend Patterns →](13-week-10.md)
