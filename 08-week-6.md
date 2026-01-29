# Week 6 — Databases & SQL

*Month 2: The Structured Web*

---

## Goals

- Understand why databases exist
- Learn SQL fundamentals
- Set up PostgreSQL
- Connect Node.js to PostgreSQL
- Replace in-memory storage with a real database

---

## Part 1: Why Databases Exist

### What to Learn

- What is a database?
- Why not just use files or JavaScript arrays?
- What is relational vs non-relational (SQL vs NoSQL)?
- What is ACID?

### Problems Databases Solve

| Problem | Array/File | Database |
|---------|------------|----------|
| Server restarts | Data lost | Data persists |
| Multiple servers | Can't share | Shared storage |
| Concurrent access | Conflicts | Transactions |
| Querying | Manual loops | Optimized queries |
| Large datasets | Memory limits | Efficient storage |

### Relational vs Non-Relational

| SQL (Relational) | NoSQL (Non-Relational) |
|-----------------|------------------------|
| Tables with rows | Documents or key-value |
| Fixed schema | Flexible schema |
| PostgreSQL, MySQL | MongoDB, Redis |
| Complex queries | Simple queries |
| ACID compliant | Eventually consistent |

### ACID Properties

- **Atomicity** - All or nothing (transactions)
- **Consistency** - Data always valid
- **Isolation** - Concurrent transactions don't interfere
- **Durability** - Committed data survives crashes

### 🤖 AI Learning Prompts

- "Explain databases like I've never heard of one"
- "What problems do databases solve that files don't?"
- "What's the difference between SQL and NoSQL databases?"
- "Explain ACID properties in simple terms"
- "When would I use PostgreSQL vs MongoDB?"

---

## Part 2: SQL Fundamentals

### What to Learn

- What is SQL? (Structured Query Language)
- What is a table, row, column?
- What is a primary key?
- What is a schema?

### Key Terminology

| Term | Description | Example |
|------|-------------|---------|
| Table | Collection of related data | `tasks` table |
| Row | Single record | One task |
| Column | Data field | `text`, `completed` |
| Primary Key | Unique identifier | `id` column |
| Schema | Database structure | All tables and their columns |

### Data Types (PostgreSQL)

| Type | Description | Example |
|------|-------------|---------|
| `INTEGER` | Whole numbers | `42` |
| `SERIAL` | Auto-incrementing integer | Primary keys |
| `VARCHAR(n)` | Variable text up to n chars | Names, titles |
| `TEXT` | Unlimited text | Descriptions |
| `BOOLEAN` | true/false | `completed` |
| `TIMESTAMP` | Date and time | `created_at` |

---

## Part 3: Essential SQL Commands

### Creating Tables

```sql
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  text VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Inserting Data

```sql
-- Single row
INSERT INTO tasks (text) VALUES ('Learn SQL');

-- Multiple rows
INSERT INTO tasks (text) VALUES 
  ('Learn SQL'),
  ('Build an API'),
  ('Deploy app');

-- Returning the inserted row
INSERT INTO tasks (text) VALUES ('Learn SQL') RETURNING *;
```

### Querying Data

```sql
-- Get all rows
SELECT * FROM tasks;

-- Get specific columns
SELECT id, text FROM tasks;

-- Filter with WHERE
SELECT * FROM tasks WHERE completed = false;
SELECT * FROM tasks WHERE id = 1;
SELECT * FROM tasks WHERE text LIKE '%SQL%';

-- Sort results
SELECT * FROM tasks ORDER BY created_at DESC;

-- Limit results
SELECT * FROM tasks LIMIT 10;

-- Combine conditions
SELECT * FROM tasks 
WHERE completed = false 
ORDER BY created_at DESC 
LIMIT 5;
```

### Updating Data

```sql
-- Update specific row
UPDATE tasks SET completed = true WHERE id = 1;

-- Update multiple columns
UPDATE tasks 
SET text = 'Updated text', completed = true 
WHERE id = 1;

-- Return updated row
UPDATE tasks SET completed = true WHERE id = 1 RETURNING *;
```

### Deleting Data

```sql
-- Delete specific row
DELETE FROM tasks WHERE id = 1;

-- Delete multiple rows
DELETE FROM tasks WHERE completed = true;

-- Return deleted row
DELETE FROM tasks WHERE id = 1 RETURNING *;

-- Delete ALL rows (careful!)
DELETE FROM tasks;
```

### 🤖 AI Learning Prompts

- "Explain SQL databases for beginners"
- "What is a primary key and why is it important?"
- "What are common SQL data types?"
- "Explain SELECT with WHERE, ORDER BY, and LIMIT"
- "What's the difference between DELETE and DROP?"

---

## Part 4: PostgreSQL Setup

### Installing PostgreSQL

**Mac (with Homebrew):**
```bash
brew install postgresql
brew services start postgresql
```

**Windows:**
- Download from postgresql.org
- Run installer
- Remember your password!

**Linux (Ubuntu):**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

### Using psql (Command Line)

```bash
# Connect to default database
psql postgres

# Create a database
CREATE DATABASE tasktracker;

# Connect to database
\c tasktracker

# List databases
\l

# List tables
\dt

# Describe a table
\d tasks

# Quit
\q
```

### Creating Your Database and Table

```sql
-- In psql, create database
CREATE DATABASE tasktracker;

-- Connect to it
\c tasktracker

-- Create tasks table
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  text VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Verify
\dt
\d tasks
```

### GUI Options

Instead of command line, you can use:
- **pgAdmin** (official, free)
- **DBeaver** (free, supports many databases)
- **TablePlus** (paid, nice UI)

### 🤖 AI Learning Prompts

- "Walk me through installing PostgreSQL on [your OS]"
- "How do I create a database and table in PostgreSQL?"
- "What are the most useful psql commands?"
- "How do I see what tables exist in my database?"

---

## Part 5: Connecting Node.js to PostgreSQL

### Install the pg Package

```bash
npm install pg
```

### Basic Connection

```javascript
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

// Or with individual options
const pool = new Pool({
  host: 'localhost',
  port: 5432,
  database: 'tasktracker',
  user: 'yourusername',
  password: 'yourpassword'
});
```

### Environment Variables

**.env**
```
DATABASE_URL=postgresql://username:password@localhost:5432/tasktracker
```

### Making Queries

```javascript
// Simple query
const result = await pool.query('SELECT * FROM tasks');
console.log(result.rows); // Array of tasks

// Parameterized query (ALWAYS use for user input!)
const result = await pool.query(
  'SELECT * FROM tasks WHERE id = $1',
  [taskId]
);

// Insert and return
const result = await pool.query(
  'INSERT INTO tasks (text) VALUES ($1) RETURNING *',
  [taskText]
);
const newTask = result.rows[0];
```

### Why Parameterized Queries?

```javascript
// ❌ DANGEROUS - SQL Injection vulnerable
const query = `SELECT * FROM tasks WHERE id = ${userInput}`;
// If userInput is: "1; DROP TABLE tasks;"  😱

// ✅ SAFE - Parameterized
const query = 'SELECT * FROM tasks WHERE id = $1';
await pool.query(query, [userInput]);
```

### Connection Pool

A pool maintains multiple connections:
- Reuses connections (faster)
- Limits concurrent connections
- Handles connection failures

```javascript
const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20,  // Maximum connections
});

// Use pool for most queries
await pool.query('SELECT...');

// For transactions, get a dedicated client
const client = await pool.connect();
try {
  await client.query('BEGIN');
  // ... multiple queries
  await client.query('COMMIT');
} catch (e) {
  await client.query('ROLLBACK');
  throw e;
} finally {
  client.release();
}
```

### 🤖 AI Learning Prompts

- "How do I connect to PostgreSQL from Node.js?"
- "What is a connection pool and why use one?"
- "Explain SQL injection and how to prevent it with parameterized queries"
- "How do I handle database errors in Node.js?"

---

## Part 6: Update Your API

### Refactor Controller to Use Database

**src/controllers/taskController.js**
```javascript
const pool = require('../db');

exports.getAll = async (req, res, next) => {
  try {
    const result = await pool.query(
      'SELECT * FROM tasks ORDER BY created_at DESC'
    );
    res.json({ tasks: result.rows });
  } catch (error) {
    next(error);
  }
};

exports.create = async (req, res, next) => {
  try {
    const { text } = req.body;
    const result = await pool.query(
      'INSERT INTO tasks (text) VALUES ($1) RETURNING *',
      [text]
    );
    res.status(201).json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};

exports.getOne = async (req, res, next) => {
  try {
    const { id } = req.params;
    const result = await pool.query(
      'SELECT * FROM tasks WHERE id = $1',
      [id]
    );
    if (result.rows.length === 0) {
      return res.status(404).json({ error: 'Task not found' });
    }
    res.json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};

exports.update = async (req, res, next) => {
  try {
    const { id } = req.params;
    const { text, completed } = req.body;
    const result = await pool.query(
      `UPDATE tasks 
       SET text = COALESCE($1, text), 
           completed = COALESCE($2, completed)
       WHERE id = $3 
       RETURNING *`,
      [text, completed, id]
    );
    if (result.rows.length === 0) {
      return res.status(404).json({ error: 'Task not found' });
    }
    res.json(result.rows[0]);
  } catch (error) {
    next(error);
  }
};

exports.delete = async (req, res, next) => {
  try {
    const { id } = req.params;
    const result = await pool.query(
      'DELETE FROM tasks WHERE id = $1 RETURNING *',
      [id]
    );
    if (result.rows.length === 0) {
      return res.status(404).json({ error: 'Task not found' });
    }
    res.json({ success: true });
  } catch (error) {
    next(error);
  }
};
```

**src/db/index.js**
```javascript
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

module.exports = pool;
```

---

## Part 7: Database Setup Scripts

### Create a Setup Script

**scripts/setup.sql**
```sql
-- Drop table if exists (for development)
DROP TABLE IF EXISTS tasks;

-- Create tasks table
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  text VARCHAR(255) NOT NULL,
  completed BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Insert sample data
INSERT INTO tasks (text) VALUES 
  ('Learn PostgreSQL'),
  ('Connect to Node.js'),
  ('Build full-stack app');
```

Run it:
```bash
psql tasktracker < scripts/setup.sql
```

---

## ✅ Week 6 Milestone Checklist

- [ ] I understand why databases exist and what problems they solve
- [ ] I can write basic SQL queries (SELECT, INSERT, UPDATE, DELETE)
- [ ] I have PostgreSQL installed and can use psql
- [ ] I can connect to PostgreSQL from Node.js
- [ ] I understand SQL injection and how to prevent it
- [ ] My API now uses PostgreSQL for data persistence
- [ ] Data persists across server restarts

---

## Next Up

[Week 7: Users & Authentication →](09-week-7.md)
