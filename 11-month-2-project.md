# Month 2 Project: Multi-User Project Manager API

*Professional Backend with Database, Auth, and Relationships*

---

## Overview

Build a complete REST API for a project management application:
- Express.js backend with proper structure
- PostgreSQL database with relationships
- JWT authentication
- Full CRUD for users, projects, and tasks

---

## Data Model

```
┌─────────────┐
│   Users     │
├─────────────┤
│ id          │
│ email       │
│ password_hash│
│ created_at  │
└──────┬──────┘
       │ one-to-many
       ▼
┌─────────────┐
│  Projects   │
├─────────────┤
│ id          │
│ name        │
│ description │
│ user_id (FK)│
│ created_at  │
└──────┬──────┘
       │ one-to-many
       ▼
┌─────────────┐
│   Tasks     │
├─────────────┤
│ id          │
│ text        │
│ completed   │
│ project_id (FK)│
│ created_at  │
└─────────────┘
```

---

## API Endpoints

### Authentication

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/auth/register` | Create account | No |
| POST | `/auth/login` | Get JWT token | No |
| GET | `/auth/me` | Get current user | Yes |

### Projects

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/projects` | List user's projects | Yes |
| POST | `/projects` | Create project | Yes |
| GET | `/projects/:id` | Get project details | Yes |
| PATCH | `/projects/:id` | Update project | Yes |
| DELETE | `/projects/:id` | Delete project | Yes |

### Tasks

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/projects/:id/tasks` | List project's tasks | Yes |
| POST | `/projects/:id/tasks` | Create task in project | Yes |
| PATCH | `/tasks/:id` | Update task | Yes |
| DELETE | `/tasks/:id` | Delete task | Yes |

---

## Technical Requirements

### Project Structure

```
project-manager-api/
├── src/
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── projectRoutes.js
│   │   └── taskRoutes.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── projectController.js
│   │   └── taskController.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── db/
│   │   └── index.js
│   └── app.js
├── scripts/
│   └── setup.sql
├── .env
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

### Database

- [ ] PostgreSQL with proper tables
- [ ] Foreign key relationships
- [ ] ON DELETE CASCADE where appropriate
- [ ] Setup script to create tables

### Authentication

- [ ] Password hashing with bcrypt
- [ ] JWT tokens for authentication
- [ ] Auth middleware to protect routes
- [ ] Users can only access their own data

### API Quality

- [ ] Proper HTTP status codes
- [ ] Consistent error response format
- [ ] Input validation
- [ ] Parameterized SQL queries (prevent injection)

### Code Quality

- [ ] Organized folder structure
- [ ] Environment variables for config
- [ ] No secrets in code
- [ ] Error handling middleware

---

## Request/Response Examples

### Register

**Request:**
```http
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword123"
}
```

**Response (201):**
```json
{
  "user": {
    "id": 1,
    "email": "user@example.com",
    "created_at": "2024-01-15T10:30:00.000Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

### Login

**Request:**
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "securepassword123"
}
```

**Response (200):**
```json
{
  "user": {
    "id": 1,
    "email": "user@example.com"
  },
  "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

### Create Project

**Request:**
```http
POST /projects
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Website Redesign",
  "description": "Redesign company website"
}
```

**Response (201):**
```json
{
  "id": 1,
  "name": "Website Redesign",
  "description": "Redesign company website",
  "user_id": 1,
  "created_at": "2024-01-15T10:30:00.000Z"
}
```

### List Projects

**Request:**
```http
GET /projects
Authorization: Bearer <token>
```

**Response (200):**
```json
{
  "projects": [
    {
      "id": 1,
      "name": "Website Redesign",
      "description": "Redesign company website",
      "task_count": "5",
      "completed_count": "2",
      "created_at": "2024-01-15T10:30:00.000Z"
    }
  ]
}
```

### Create Task

**Request:**
```http
POST /projects/1/tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "text": "Create wireframes"
}
```

**Response (201):**
```json
{
  "id": 1,
  "text": "Create wireframes",
  "completed": false,
  "project_id": 1,
  "created_at": "2024-01-15T10:30:00.000Z"
}
```

---

## Error Response Format

All errors should follow this format:

```json
{
  "error": "Error message here"
}
```

### Status Codes

| Code | Usage |
|------|-------|
| 200 | Success (GET, PATCH) |
| 201 | Created (POST) |
| 400 | Bad request (validation error) |
| 401 | Unauthorized (no/invalid token) |
| 403 | Forbidden (not your resource) |
| 404 | Not found |
| 409 | Conflict (email exists) |
| 500 | Server error |

---

## Git Requirements

### Branch Strategy

Use Git Flow or GitHub Flow:

```bash
# Feature branches
feature/auth-endpoints
feature/project-crud
feature/task-crud
feature/validation

# At least one Pull Request
```

### Commit Messages

Follow conventional commits:

```
feat: add user registration endpoint
feat: add JWT authentication
feat: add project CRUD endpoints
fix: handle duplicate email registration
docs: add API documentation to README
```

### Required Tags

- `v1.0.0` - Initial release

---

## README Requirements

Your README should include:

```markdown
# Project Manager API

REST API for project and task management.

## Tech Stack
- Node.js / Express.js
- PostgreSQL
- JWT Authentication

## Getting Started

### Prerequisites
- Node.js 18+
- PostgreSQL

### Installation
1. Clone the repo
2. Install dependencies: `npm install`
3. Copy `.env.example` to `.env` and fill in values
4. Run database setup: `psql yourdb < scripts/setup.sql`
5. Start server: `npm run dev`

## API Endpoints

### Authentication
- POST /auth/register - Create account
- POST /auth/login - Login
- GET /auth/me - Get current user

### Projects
- GET /projects - List projects
- POST /projects - Create project
- GET /projects/:id - Get project
- PATCH /projects/:id - Update project
- DELETE /projects/:id - Delete project

### Tasks
- GET /projects/:id/tasks - List tasks
- POST /projects/:id/tasks - Create task
- PATCH /tasks/:id - Update task
- DELETE /tasks/:id - Delete task

## Environment Variables
See `.env.example`
```

---

## Stretch Goals

If you finish early:

- [ ] Add task due dates
- [ ] Add task priorities (low, medium, high)
- [ ] Add project status (active, archived)
- [ ] Add search/filter for tasks
- [ ] Add pagination for lists
- [ ] Add rate limiting
- [ ] Add request logging
- [ ] Write unit tests

---

## Evaluation Checklist

### Functionality
- [ ] Can register new users
- [ ] Can login and receive token
- [ ] Can create, read, update, delete projects
- [ ] Can create, read, update, delete tasks
- [ ] Users can only see their own data
- [ ] Proper error responses

### Security
- [ ] Passwords are hashed
- [ ] SQL queries are parameterized
- [ ] Tokens expire
- [ ] No secrets in code

### Code Quality
- [ ] Proper folder structure
- [ ] Environment variables used
- [ ] Consistent error handling
- [ ] Clean, readable code

### Git
- [ ] Multiple meaningful commits
- [ ] Used feature branches
- [ ] Created at least one PR
- [ ] Tagged v1.0.0 release

---

## Next Steps

After completing this project, you're ready for:

[Week 9: React Fundamentals →](12-week-9.md)
