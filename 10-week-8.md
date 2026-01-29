# Week 8 — Data Relationships & Advanced Git

*Month 2: The Structured Web*

---

## Goals

- Understand database relationships
- Learn SQL JOINs
- Implement a Projects feature
- Master Git Flow and Pull Requests
- Understand semantic versioning

---

## Part 1: Database Relationships

### What to Learn

- What is a foreign key?
- What is a one-to-many relationship?
- What is a many-to-many relationship?
- How do JOINs work?

### Types of Relationships

| Relationship | Example | Implementation |
|--------------|---------|----------------|
| One-to-Many | User has many Tasks | Foreign key on "many" side |
| Many-to-Many | Tasks have many Tags | Junction table |
| One-to-One | User has one Profile | Foreign key + unique |

### Our Data Model

```
Users
  ↓ (one-to-many)
Projects
  ↓ (one-to-many)
Tasks
```

A user has many projects. A project has many tasks.

### 🤖 AI Learning Prompts

- "Explain foreign keys like I'm new to databases"
- "What is a one-to-many relationship with an example?"
- "When would I use a many-to-many relationship?"
- "What is a junction table?"

---

## Part 2: Creating Related Tables

### Projects Table

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Updated Tasks Table

```sql
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  text VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT false,
  project_id INTEGER REFERENCES projects(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Foreign Key Constraints

```sql
-- ON DELETE CASCADE: Delete tasks when project is deleted
-- ON DELETE SET NULL: Set project_id to NULL when project deleted
-- ON DELETE RESTRICT: Prevent deletion if tasks exist
```

---

## Part 3: JOIN Queries

### What is a JOIN?

JOINs combine rows from multiple tables based on related columns.

### INNER JOIN

Returns only matching rows from both tables:

```sql
-- Get tasks with their project names
SELECT 
  tasks.id,
  tasks.text,
  tasks.completed,
  projects.name AS project_name
FROM tasks
INNER JOIN projects ON tasks.project_id = projects.id;
```

### LEFT JOIN

Returns all rows from left table, matching rows from right:

```sql
-- Get all projects, even those with no tasks
SELECT 
  projects.name,
  COUNT(tasks.id) AS task_count
FROM projects
LEFT JOIN tasks ON tasks.project_id = projects.id
GROUP BY projects.id;
```

### Multiple JOINs

```sql
-- Get tasks with project and user info
SELECT 
  tasks.text,
  projects.name AS project_name,
  users.email AS user_email
FROM tasks
JOIN projects ON tasks.project_id = projects.id
JOIN users ON projects.user_id = users.id
WHERE users.id = $1;
```

### 🤖 AI Learning Prompts

- "Explain SQL JOIN with a simple example"
- "What's the difference between INNER JOIN and LEFT JOIN?"
- "How do I join three tables together?"
- "When would I use GROUP BY with a JOIN?"

---

## Part 4: Implementing Projects

### Project Routes

**src/routes/projectRoutes.js**
```javascript
const express = require('express');
const router = express.Router();
const projectController = require('../controllers/projectController');
const auth = require('../middleware/auth');

router.use(auth);

router.get('/', projectController.getAll);
router.post('/', projectController.create);
router.get('/:id', projectController.getOne);
router.patch('/:id', projectController.update);
router.delete('/:id', projectController.delete);

// Nested tasks routes
router.get('/:id/tasks', projectController.getTasks);
router.post('/:id/tasks', projectController.createTask);

module.exports = router;
```

### Project Controller

**src/controllers/projectController.js**
```javascript
const pool = require('../db');

exports.getAll = async (req, res, next) => {
  try {
    const { userId } = req.user;
    
    const result = await pool.query(`
      SELECT 
        projects.*,
        COUNT(tasks.id) AS task_count,
        COUNT(CASE WHEN tasks.completed THEN 1 END) AS completed_count
      FROM projects
      LEFT JOIN tasks ON tasks.project_id = projects.id
      WHERE projects.user_id = $1
      GROUP BY projects.id
      ORDER BY projects.created_at DESC
    `, [userId]);
    
    res.json({ projects: result.rows });
  } catch (error) {
    next(error);
  }
};

exports.create = async (req, res, next) => {
  try {
    const { name, description } = req.body;
    const { userId } = req.user;
    
    const result = await pool.query(
      `INSERT INTO projects (name, description, user_id) 
       VALUES ($1, $2, $3) 
       RETURNING *`,
      [name, description, userId]
    );
    
    res.status(201).json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};

exports.getTasks = async (req, res, next) => {
  try {
    const { id } = req.params;
    const { userId } = req.user;
    
    // Verify project belongs to user
    const project = await pool.query(
      'SELECT * FROM projects WHERE id = $1 AND user_id = $2',
      [id, userId]
    );
    
    if (project.rows.length === 0) {
      return res.status(404).json({ error: 'Project not found' });
    }
    
    const tasks = await pool.query(
      'SELECT * FROM tasks WHERE project_id = $1 ORDER BY created_at DESC',
      [id]
    );
    
    res.json({ tasks: tasks.rows });
  } catch (error) {
    next(error);
  }
};

exports.createTask = async (req, res, next) => {
  try {
    const { id } = req.params;
    const { text } = req.body;
    const { userId } = req.user;
    
    // Verify project belongs to user
    const project = await pool.query(
      'SELECT * FROM projects WHERE id = $1 AND user_id = $2',
      [id, userId]
    );
    
    if (project.rows.length === 0) {
      return res.status(404).json({ error: 'Project not found' });
    }
    
    const result = await pool.query(
      'INSERT INTO tasks (text, project_id) VALUES ($1, $2) RETURNING *',
      [text, id]
    );
    
    res.status(201).json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};
```

---

## Part 5: Git Flow

### What to Learn

- What is Git Flow?
- What are long-lived branches?
- When to use which branch type?
- What is a hotfix?

### Git Flow Branches

```
main (or master)
  ↑
  └── hotfix/*     ← Urgent production fixes
  
develop
  ↑
  ├── feature/*    ← New features
  └── release/*    ← Preparing for release
```

### Branch Purposes

| Branch | Purpose | Merges To |
|--------|---------|-----------|
| `main` | Production code | - |
| `develop` | Integration branch | main (via release) |
| `feature/*` | New features | develop |
| `release/*` | Release prep | main + develop |
| `hotfix/*` | Urgent fixes | main + develop |

### Git Flow Workflow

```bash
# Start a feature
git checkout develop
git checkout -b feature/add-projects

# Work on feature, commit...
git add .
git commit -m "Add projects table and model"

# Finish feature
git checkout develop
git merge feature/add-projects
git branch -d feature/add-projects

# Start a release
git checkout -b release/1.0.0
# Final testing, version bumps...
git checkout main
git merge release/1.0.0
git tag v1.0.0
git checkout develop
git merge release/1.0.0

# Hotfix (urgent production fix)
git checkout main
git checkout -b hotfix/fix-login-bug
# Fix the bug...
git checkout main
git merge hotfix/fix-login-bug
git tag v1.0.1
git checkout develop
git merge hotfix/fix-login-bug
```

### GitHub Flow (Simpler Alternative)

For smaller teams or projects:

```bash
# 1. Create branch from main
git checkout -b feature/add-projects

# 2. Make changes, commit

# 3. Push branch
git push -u origin feature/add-projects

# 4. Open Pull Request on GitHub

# 5. Code review

# 6. Merge PR to main

# 7. Deploy main
```

### 🤖 AI Learning Prompts

- "Explain Git Flow branching strategy"
- "What is GitHub Flow and how is it different from Git Flow?"
- "When should I use Git Flow vs GitHub Flow?"
- "What is a hotfix branch and when do I use it?"

---

## Part 6: Pull Requests

### What to Learn

- What is a Pull Request?
- How to write good PR descriptions
- How to do code review
- How to handle review feedback

### Creating a Pull Request

1. Push your branch to GitHub:
```bash
git push -u origin feature/add-projects
```

2. Go to GitHub → "Pull Requests" → "New Pull Request"

3. Select your branch

4. Write a good description

### Good PR Description Template

```markdown
## Description
Brief explanation of what this PR does.

## Changes
- Added projects table
- Created project CRUD endpoints
- Added project-task relationship

## How to Test
1. Run migrations
2. Create a new project via POST /projects
3. Verify tasks can be added to projects

## Screenshots (if UI changes)
[Include screenshots]

## Related Issues
Closes #123
```

### Code Review Best Practices

**As a Reviewer:**
- Be constructive, not critical
- Ask questions instead of demanding
- Explain why, not just what
- Approve once changes are good

**As an Author:**
- Don't take feedback personally
- Explain your reasoning
- Make requested changes promptly
- Thank reviewers

### 🤖 AI Learning Prompts

- "Explain Pull Requests for someone new to GitHub"
- "What makes a good Pull Request description?"
- "What should I look for when reviewing code?"
- "How do I request changes on a Pull Request?"

---

## Part 7: Semantic Versioning

### What to Learn

- What is versioning?
- What is SemVer?
- When to bump each number
- How to create releases

### Semantic Versioning Format

```
MAJOR.MINOR.PATCH

Examples:
1.0.0 → First release
1.1.0 → Added feature
1.1.1 → Bug fix
2.0.0 → Breaking change
```

### When to Bump

| Change Type | Version Bump | Example |
|-------------|--------------|---------|
| Breaking change | MAJOR | API endpoint renamed |
| New feature | MINOR | Added projects |
| Bug fix | PATCH | Fixed login error |
| Documentation | None or PATCH | Updated README |

### Creating a Release

```bash
# Update version in package.json
npm version minor  # or major, patch

# This creates a commit and tag
# Or manually:
git tag v1.1.0
git push origin v1.1.0
```

### On GitHub

1. Go to "Releases"
2. Click "Create a new release"
3. Choose your tag
4. Write release notes
5. Publish

### 🤖 AI Learning Prompts

- "Explain semantic versioning for beginners"
- "When should I bump MAJOR vs MINOR vs PATCH?"
- "What are Git tags and how do they relate to versions?"
- "How do I write good release notes?"

---

## 🧩 Month 2 Project: Multi-User Project Manager API

See the full project requirements in [Month 2 Project](11-month-2-project.md).

---

## ✅ Week 8 Milestone Checklist

- [ ] I understand database relationships (one-to-many, many-to-many)
- [ ] I can write JOIN queries
- [ ] I have implemented projects with tasks
- [ ] I understand Git Flow branching strategy
- [ ] I have created at least one Pull Request
- [ ] I understand semantic versioning
- [ ] I have tagged a release

---

## Next Up

[Month 2 Project Requirements →](11-month-2-project.md)
