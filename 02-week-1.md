# Week 1 — The Internet, Browser & Your First Website

*Month 1: The Raw Web*

---

## Goals

- Understand what happens when you visit a website
- Learn HTML structure and semantic elements
- Learn CSS styling and the box model
- Build and deploy a multi-page website
- Practice Git workflow throughout

---

## Part 1: How the Internet Actually Works

### What to Learn

- What happens when you type a URL and press Enter?
- What is DNS? (Domain Name System — the internet's phone book)
- What is an IP address?
- What is HTTP? What is HTTPS?
- What is a request? What is a response?
- What are HTTP headers?
- What are HTTP status codes? (200, 404, 500, etc.)

### Key Concepts

| Term | Description |
|------|-------------|
| DNS | Translates domain names (google.com) to IP addresses |
| HTTP | Protocol for transferring web pages |
| HTTPS | HTTP with encryption (secure) |
| Request | What your browser sends to get a page |
| Response | What the server sends back |
| Headers | Metadata about the request/response |
| Status Code | Number indicating success/failure (200 = OK, 404 = Not Found) |

### 🤖 AI Learning Prompts

- "Walk me through every step that happens from typing google.com to seeing the page"
- "Explain DNS like I'm explaining it to a friend"
- "What's the difference between HTTP and HTTPS?"
- "What are HTTP headers and why do they exist?"
- "Explain the most common HTTP status codes and what they mean"
- "What is latency and why does it matter for websites?"

### Hands-On Exploration

1. Open browser DevTools (F12)
2. Go to the Network tab
3. Visit a website and watch the requests
4. Click on a request and examine:
   - Headers
   - Response
   - Timing
5. Notice how many requests a single page makes

---

## Part 2: HTML — Structure of Web Pages

### What to Learn

- What is HTML? (HyperText Markup Language)
- What is the DOM? (Document Object Model)
- What is an HTML element, tag, and attribute?
- What is semantic HTML and why does it matter?
- What is the document structure? (DOCTYPE, html, head, body)

### Essential HTML Elements

**Document Structure:**
```html
<!DOCTYPE html>          <!-- tells browser this is HTML5 -->
<html>                   <!-- root element -->
  <head>                 <!-- metadata, title, links to CSS -->
    <title>Page Title</title>
  </head>
  <body>                 <!-- visible content -->
  </body>
</html>
```

**Text Content:**
| Element | Purpose |
|---------|---------|
| `<h1>` - `<h6>` | Headings (hierarchy matters for accessibility) |
| `<p>` | Paragraphs |
| `<span>` | Inline container |
| `<div>` | Block container |
| `<strong>` | Important text (bold) |
| `<em>` | Emphasized text (italic) |

**Lists:**
| Element | Purpose |
|---------|---------|
| `<ul>` | Unordered list (bullets) |
| `<ol>` | Ordered list (numbers) |
| `<li>` | List item |

**Links & Media:**
| Element | Purpose |
|---------|---------|
| `<a href="">` | Anchor/link (understand relative vs absolute URLs) |
| `<img src="" alt="">` | Images (alt text required for accessibility) |

**Semantic Elements:**
| Element | Purpose |
|---------|---------|
| `<header>` | Page or section header |
| `<nav>` | Navigation links |
| `<main>` | Main content |
| `<section>` | Thematic grouping |
| `<article>` | Self-contained content |
| `<aside>` | Sidebar content |
| `<footer>` | Page or section footer |

### 🤖 AI Learning Prompts

- "Explain the DOM like I've never heard of it"
- "What's the difference between div and span?"
- "Why is semantic HTML important for accessibility?"
- "When should I use section vs article vs div?"

---

## Part 3: HTML Forms (Important!)

### What to Learn

- How forms collect user input
- Different input types and when to use them
- Form attributes and how they work
- Accessibility with labels

### Form Elements

```html
<form action="/submit" method="POST">
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" required>
  
  <label for="message">Message:</label>
  <textarea id="message" name="message"></textarea>
  
  <button type="submit">Send</button>
</form>
```

### Input Types to Know

| Type | Purpose |
|------|---------|
| `type="text"` | Single line text |
| `type="email"` | Email with validation |
| `type="password"` | Hidden characters |
| `type="number"` | Numeric input |
| `type="checkbox"` | Multiple selection |
| `type="radio"` | Single selection from group |
| `type="submit"` | Form submission button |
| `type="date"` | Date picker |

### Form Attributes to Understand

| Attribute | Purpose |
|-----------|---------|
| `action` | Where to send form data |
| `method` | GET or POST |
| `name` | Identifies the field in submitted data |
| `id` | Unique identifier (for labels) |
| `placeholder` | Hint text inside input |
| `required` | Makes field mandatory |
| `value` | Default or submitted value |

### 🤖 AI Learning Prompts

- "Explain the form action and method attributes"
- "What's the difference between name and id on form inputs?"
- "Why should every input have a label?"
- "How do GET and POST methods differ for forms?"
- "What is form validation and how does it work?"

---

## Part 4: CSS — Styling Web Pages

### What to Learn

- What is CSS? (Cascading Style Sheets)
- What does "cascading" mean?
- How to connect CSS to HTML (external, internal, inline)
- What is specificity and why does it matter?
- What is the box model?

### CSS Selectors

| Selector | Example | Selects |
|----------|---------|---------|
| Element | `p { }` | All paragraphs |
| Class | `.classname { }` | Elements with that class |
| ID | `#idname { }` | Single element with that ID |
| Descendant | `div p { }` | Paragraphs inside divs |
| Multiple | `h1, h2, h3 { }` | All three elements |
| Pseudo-class | `:hover` | Element being hovered |

### The Box Model (Critical!)

Every element is a box with:
1. **Content** — the actual text/image
2. **Padding** — space inside the border
3. **Border** — the edge
4. **Margin** — space outside the border

```css
/* Makes sizing intuitive — highly recommended */
* {
  box-sizing: border-box;
}
```

### Essential CSS Properties

**Text:**
```css
color: #333;
font-family: Arial, sans-serif;
font-size: 16px;
font-weight: bold;
text-align: center;
line-height: 1.5;
```

**Box Model:**
```css
width: 100%;
height: auto;
padding: 20px;
margin: 10px 0;
border: 1px solid #ccc;
```

**Layout:**
```css
display: flex;
position: relative;
```

**Visual:**
```css
background-color: #f5f5f5;
border-radius: 8px;
box-shadow: 0 2px 4px rgba(0,0,0,0.1);
```

### Flexbox Basics

```css
.container {
  display: flex;
  flex-direction: row;      /* or column */
  justify-content: center;  /* horizontal alignment */
  align-items: center;      /* vertical alignment */
  gap: 20px;                /* space between items */
}
```

### 🤖 AI Learning Prompts

- "Explain the CSS box model with a diagram description"
- "What does 'cascading' mean in Cascading Style Sheets?"
- "Explain CSS specificity and why my styles might not be applying"
- "When should I use flexbox vs grid?"
- "What's the difference between margin and padding?"
- "Explain em, rem, and px units in CSS"
- "What does box-sizing: border-box do and why use it?"

---

## Part 5: Build Your First Multi-Page Website

### Project Requirements

Build a personal website with at least 3 pages:

1. **Home** — introduction, hero section
2. **About** — about you, interests, background
3. **Contact** — contact form (doesn't need to work yet)

### Technical Requirements

- [ ] Proper HTML5 document structure on each page
- [ ] Navigation menu that links all pages together
- [ ] External CSS stylesheet (not inline styles)
- [ ] At least one form with multiple input types
- [ ] Responsive design (looks decent on mobile)
- [ ] Semantic HTML elements (header, nav, main, footer)

### Git Workflow

Initialize repo at project start and make meaningful commits:

```
"Initial commit: add project structure"
"Add HTML structure for home page"
"Add navigation menu"
"Add CSS styling for header"
"Create about page"
"Add contact form"
"Style form inputs"
"Add responsive styles for mobile"
```

### 🤖 AI Prompts for Building

- "What's a good file structure for a multi-page HTML website?"
- "How do I link between HTML pages in the same folder?"
- "How do I make a navigation menu that shows what page I'm on?"
- "How do I make a form that has text input, email, and a textarea?"
- "How do I make my website look good on mobile using CSS?"
- "What's a CSS reset and should I use one?"

---

## Part 6: Deploy Your Site (Make It Live!)

### What to Learn

- What is web hosting?
- What is static hosting vs dynamic hosting?
- What is a CDN? (Content Delivery Network)
- What is a deploy?

### Deployment Options (Pick One)

**GitHub Pages** (Recommended for beginners)
1. Push your code to GitHub
2. Go to repo Settings → Pages
3. Select branch to deploy (main)
4. Your site is live at `username.github.io/repo-name`

**Netlify**
1. Create account at netlify.com
2. Drag and drop your folder, or connect to GitHub
3. Automatic deploys on every push

**Vercel**
1. Create account at vercel.com
2. Import from GitHub
3. Automatic deploys on every push

### 🤖 AI Learning Prompts

- "Explain web hosting like I'm new to it"
- "What's the difference between static and dynamic hosting?"
- "Walk me through deploying to GitHub Pages step by step"
- "Why would I use Netlify instead of GitHub Pages?"

---

## ✅ Week 1 Milestone Checklist

- [ ] I can explain what happens when you visit a website
- [ ] I understand HTTP requests, responses, and status codes
- [ ] I can write valid HTML5 with proper document structure
- [ ] I understand semantic HTML and when to use which elements
- [ ] I can create forms with various input types
- [ ] I understand the CSS box model
- [ ] I can use flexbox for layouts
- [ ] I have a live website deployed on the internet
- [ ] My project is on GitHub with meaningful commit history

---

## Next Up

[Week 2: JavaScript & The DOM →](03-week-2.md)
