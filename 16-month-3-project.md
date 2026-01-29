# Month 3 Final Project: Modern Full-Stack Task App

*Complete SaaS-style Application - Deployed and Live*

---

## Overview

Build and deploy a complete, modern full-stack application:
- React frontend with routing and state management
- Express.js backend with Prisma ORM
- PostgreSQL database
- JWT authentication
- Deployed to production

This is the culmination of everything you've learned.

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React + Vite |
| Routing | React Router |
| Styling | CSS (or Tailwind) |
| Backend | Express.js |
| ORM | Prisma |
| Database | PostgreSQL |
| Auth | JWT |
| Frontend Host | Vercel |
| Backend Host | Railway |
| Database Host | Railway / Neon |

---

## Features

### Authentication
- [ ] User registration with email/password
- [ ] User login
- [ ] Protected routes (redirect if not logged in)
- [ ] Logout functionality
- [ ] Persist login across page refreshes

### Projects
- [ ] View list of user's projects
- [ ] Create new project
- [ ] Edit project name/description
- [ ] Delete project (with confirmation)
- [ ] View project detail page

### Tasks
- [ ] View tasks in a project
- [ ] Add new task to project
- [ ] Mark task complete/incomplete
- [ ] Delete task
- [ ] Task count on project cards

### UX
- [ ] Loading states while fetching
- [ ] Error messages when things fail
- [ ] Empty states (no projects, no tasks)
- [ ] Responsive design (mobile-friendly)
- [ ] Form validation with error messages

---

## Project Structure

### Frontend

```
frontend/
├── src/
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button.jsx
│   │   │   ├── Input.jsx
│   │   │   ├── Spinner.jsx
│   │   │   └── ErrorMessage.jsx
│   │   ├── layout/
│   │   │   ├── Header.jsx
│   │   │   └── Layout.jsx
│   │   ├── auth/
│   │   │   ├── LoginForm.jsx
│   │   │   └── RegisterForm.jsx
│   │   ├── projects/
│   │   │   ├── ProjectList.jsx
│   │   │   ├── ProjectCard.jsx
│   │   │   └── ProjectForm.jsx
│   │   └── tasks/
│   │       ├── TaskList.jsx
│   │       ├── TaskItem.jsx
│   │       └── TaskForm.jsx
│   ├── pages/
│   │   ├── HomePage.jsx
│   │   ├── LoginPage.jsx
│   │   ├── RegisterPage.jsx
│   │   ├── ProjectsPage.jsx
│   │   └── ProjectDetailPage.jsx
│   ├── context/
│   │   └── AuthContext.jsx
│   ├── services/
│   │   └── api.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── .env
├── .env.example
└── package.json
```

### Backend

```
backend/
├── prisma/
│   └── schema.prisma
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
│   ├── lib/
│   │   └── prisma.js
│   ├── app.js
│   └── server.js
├── .env
├── .env.example
├── .gitignore
└── package.json
```

---

## API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/register` | Create account |
| POST | `/auth/login` | Get token |
| GET | `/auth/me` | Get current user |

### Projects
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/projects` | List user's projects |
| POST | `/projects` | Create project |
| GET | `/projects/:id` | Get project |
| PATCH | `/projects/:id` | Update project |
| DELETE | `/projects/:id` | Delete project |

### Tasks
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/projects/:id/tasks` | List tasks |
| POST | `/projects/:id/tasks` | Create task |
| PATCH | `/tasks/:id` | Update task |
| DELETE | `/tasks/:id` | Delete task |

---

## Page Requirements

### Login Page (`/login`)
- Email and password inputs
- Form validation
- Error message display
- Link to register page
- Redirect to projects on success

### Register Page (`/register`)
- Email and password inputs
- Password confirmation
- Form validation
- Error message display
- Link to login page
- Redirect to projects on success

### Projects Page (`/projects`)
- Protected route
- List of project cards
- Each card shows name, description, task count
- "New Project" button
- Loading spinner while fetching
- Empty state if no projects

### Project Detail Page (`/projects/:id`)
- Protected route
- Project name and description
- Edit/Delete project buttons
- List of tasks
- Add task form
- Toggle task complete
- Delete task button
- Loading/error/empty states

---

## Code Quality Requirements

### Git
- [ ] Feature branches for major work
- [ ] Meaningful commit messages
- [ ] At least one Pull Request
- [ ] Tagged release (v1.0.0)
- [ ] Clean commit history

### Code
- [ ] ESLint configured (no errors)
- [ ] Prettier configured
- [ ] No console.log in production
- [ ] Consistent code style
- [ ] Meaningful variable names

### Documentation
- [ ] README with setup instructions
- [ ] .env.example files
- [ ] API documentation

---

## Deployment Requirements

### Frontend (Vercel)
- [ ] Connected to GitHub
- [ ] Environment variables set
- [ ] Auto-deploy on push to main
- [ ] Working production URL

### Backend (Railway)
- [ ] Connected to GitHub
- [ ] PostgreSQL database added
- [ ] Environment variables set
- [ ] Migrations run
- [ ] Working production URL

### Integration
- [ ] Frontend can reach backend API
- [ ] CORS configured correctly
- [ ] Auth flow works end-to-end
- [ ] All CRUD operations work

---

## Stretch Goals

If you finish early:

### Features
- [ ] Search/filter tasks
- [ ] Task due dates
- [ ] Task priorities
- [ ] Drag and drop task reordering
- [ ] Project sharing between users
- [ ] Dark mode

### Technical
- [ ] Unit tests with Jest
- [ ] E2E tests with Playwright
- [ ] Docker configuration
- [ ] Custom domain
- [ ] SSL certificate
- [ ] Rate limiting
- [ ] Request logging

---

## Evaluation Criteria

### Functionality (40%)
- All features work
- No broken pages
- Error handling works
- Auth flow complete

### Code Quality (25%)
- Clean, readable code
- Proper structure
- Consistent style
- No obvious bugs

### Deployment (20%)
- Both parts deployed
- Works in production
- Environment properly configured

### Git/Documentation (15%)
- Good commit history
- Feature branches used
- README is helpful
- Code is documented

---

## Submission

When complete:

1. Ensure both apps are deployed and working
2. Push all code to GitHub
3. Tag release as `v1.0.0`
4. Update README with:
   - Live URLs
   - Setup instructions
   - Tech stack
   - Screenshots

### Live URLs to Submit
- Frontend: `https://your-app.vercel.app`
- Backend: `https://your-api.railway.app`
- GitHub: `https://github.com/you/repo`

---

## You Did It! 🎉

After completing this project, proceed to:

[Graduation →](17-graduation.md)
