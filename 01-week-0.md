# Week 0 — Developer Environment Setup

*Complete this BEFORE starting Week 1*

---

## Goals

- Set up a professional development environment
- Learn to navigate using the terminal/command line
- Understand what a code editor is and how to use it
- Install and configure Git for version control
- Set up GitHub for remote repositories

---

## Part 1: Understanding Your Terminal

### What to Learn

- What is a terminal/command line interface (CLI)?
- Why do developers use the terminal instead of clicking around?
- What is a shell? (Bash, Zsh, PowerShell)
- What is a file path? (absolute vs relative paths)
- What is your "current working directory"?

### Essential Terminal Commands

**Navigation:**
| Command | Description |
|---------|-------------|
| `pwd` | "print working directory" — where am I right now? |
| `ls` | list files and folders in current directory |
| `ls -la` | list ALL files including hidden ones, with details |
| `cd foldername` | change directory into a folder |
| `cd ..` | go up one level (parent folder) |
| `cd ~` | go to your home directory |
| `cd /` | go to root directory |

**File & Folder Operations:**
| Command | Description |
|---------|-------------|
| `mkdir foldername` | make a new directory/folder |
| `touch filename.txt` | create an empty file |
| `rm filename.txt` | remove/delete a file (careful, no trash!) |
| `rm -rf foldername` | remove a folder and everything inside (VERY careful!) |
| `cp source destination` | copy a file |
| `mv source destination` | move or rename a file |
| `cat filename` | display contents of a file |
| `clear` | clear the terminal screen |

### 🤖 AI Learning Prompts

- "Explain what a terminal is like I've never seen one before"
- "What's the difference between a terminal, console, shell, and command line?"
- "Why do developers prefer using the terminal over clicking through folders?"
- "Explain absolute vs relative file paths with examples"
- "What does the ~ symbol mean in terminal paths?"

### Practice Exercise

Using ONLY the terminal (no mouse clicking):
1. Navigate to your Documents folder
2. Create a folder called "bootcamp"
3. Inside bootcamp, create a folder called "week0"
4. Inside week0, create a file called "notes.txt"
5. Navigate back to Documents
6. List all contents to verify your work

---

## Part 2: Code Editor Setup (VS Code)

### What to Learn

- What is an IDE vs a text editor?
- Why VS Code is popular among developers
- What are extensions and why they matter
- How to open folders and projects
- How to use the integrated terminal

### Essential VS Code Concepts

- Opening a folder (not just a file) as your "workspace"
- The file explorer sidebar
- The integrated terminal (Ctrl+` or Cmd+`)
- Command palette (Ctrl+Shift+P or Cmd+Shift+P)
- Settings and preferences
- Installing extensions

### Recommended Starter Extensions

- **Prettier** — code formatting
- **ESLint** — JavaScript error checking
- **Live Server** — preview HTML files
- **GitLens** — enhanced Git features
- **Auto Rename Tag** — HTML helper

### 🤖 AI Learning Prompts

- "What's the difference between an IDE and a text editor?"
- "Why do developers open folders instead of individual files in VS Code?"
- "Explain what the VS Code integrated terminal is and why it's useful"
- "What does the Prettier extension do and why should I use it?"

---

## Part 3: Git & Version Control Fundamentals

### What to Learn

- What is version control and why does it exist?
- What problem does Git solve?
- What did developers do before Git? (manual backups, email code around)
- What is the difference between Git and GitHub?
- What is a repository (repo)?

### Installing Git

**Windows:**
- Install Git for Windows (includes Git Bash)
- Git Bash gives you a Unix-like terminal on Windows
- Also includes Git GUI for visual interface

**Mac:**
- Git comes pre-installed, or install via Xcode Command Line Tools
- Run `git --version` to check

**Linux:**
- Use your package manager: `sudo apt install git`

### Git Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

### Core Git Concepts

| Concept | Description |
|---------|-------------|
| Working Directory | Where your actual files are |
| Staging Area (Index) | Files you've marked to include in next commit |
| Repository | The .git folder that stores all version history |
| Commit | A snapshot of your code at a point in time |
| Branch | A parallel version of your code |

### Essential Git Commands

**Starting:**
| Command | Description |
|---------|-------------|
| `git init` | initialize a new Git repository in current folder |
| `git clone [url]` | download a copy of an existing repository |

**Daily Workflow:**
| Command | Description |
|---------|-------------|
| `git status` | see what's changed, what's staged, what branch you're on |
| `git add filename` | stage a specific file |
| `git add .` | stage ALL changed files |
| `git commit -m "message"` | save a snapshot with a description |
| `git log` | see history of commits |
| `git log --oneline` | compact view of commit history |

**Understanding Changes:**
| Command | Description |
|---------|-------------|
| `git diff` | see what's changed but not yet staged |
| `git diff --staged` | see what's staged but not yet committed |

### 🤖 AI Learning Prompts

- "Explain version control like I'm 10 years old"
- "What problems did developers have before Git existed?"
- "What's the difference between Git and GitHub?"
- "Explain the Git staging area — why does it exist?"
- "What makes a good commit message?"
- "Walk me through what happens internally when I run git commit"

### Practice Exercise

1. Create a new folder called "git-practice"
2. Initialize it as a Git repository
3. Create a file called "hello.txt" with some text
4. Check the status
5. Stage the file
6. Commit with message "Initial commit: add hello.txt"
7. Modify the file
8. Check the diff
9. Stage and commit with message "Update hello.txt with more content"
10. View your commit log

---

## Part 4: GitHub Setup

### What to Learn

- What is GitHub? (remote hosting for Git repositories)
- What is a remote repository vs local repository?
- What is the difference between Git, GitHub, GitLab, and Bitbucket?
- How does authentication work? (HTTPS vs SSH)

### Setup Tasks

1. Create a GitHub account
2. Set up authentication (SSH key recommended)
3. Understand your profile and repositories page

### SSH Key Setup (Recommended)

- What is SSH and why use it instead of HTTPS?
- Generate an SSH key pair
- Add public key to GitHub
- Test your connection

### Core Remote Commands

| Command | Description |
|---------|-------------|
| `git remote add origin [url]` | connect local repo to GitHub |
| `git push -u origin main` | upload your commits to GitHub |
| `git pull` | download latest changes from GitHub |
| `git fetch` | download changes without merging |

### 🤖 AI Learning Prompts

- "Explain the difference between Git and GitHub"
- "What is a remote repository?"
- "Explain SSH keys like I'm new to security concepts"
- "What's the difference between git pull and git fetch?"
- "Why do I need to set an upstream branch with -u?"

### Practice Exercise

1. Create a new repository on GitHub (don't initialize with README)
2. Connect your local "git-practice" folder to GitHub
3. Push your commits
4. Make a change on GitHub directly (edit a file in browser)
5. Pull that change to your local machine

---

## ✅ Week 0 Milestone Checklist

- [ ] I can navigate my file system using only terminal commands
- [ ] I understand the difference between absolute and relative paths
- [ ] I have VS Code installed with basic extensions
- [ ] I can open a folder as a workspace and use the integrated terminal
- [ ] I understand what version control is and why Git exists
- [ ] I can init, add, commit, and view log in Git
- [ ] I have a GitHub account with SSH authentication set up
- [ ] I can push and pull between local and remote repositories

---

## Next Up

[Week 1: The Internet, Browser & Your First Website →](02-week-1.md)
