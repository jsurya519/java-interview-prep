# Phase 2 - Git & GitHub Basics

## Goal

Learn version control using Git and host projects using GitHub.

---

## Task 1 - Understand Git & GitHub

### Learn the Difference

- What is Git?
- What is GitHub?
- Why version control is important
- Why developers use Git in real projects

### Understand Basic Workflow

```text
Code Changes -> Git Commit -> Push to GitHub
```

---

## Task 2 - Install & Configure Git

### Install Git

Verify installation:

```bash
git --version
```

### Configure Git Username & Email

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

---

## Task 3 - Learn Basic Git Commands

Understand and practice the following commands:

### Clone Repository

```bash
git clone <repo-url>
```

### Check Status

```bash
git status
```

### Add Changes

```bash
git add .
```

### Commit Changes

```bash
git commit -m "Initial commit"
```

### Create Branch

```bash
git branch feature-branch
```

### Switch Branch

```bash
git checkout feature-branch
```

### Push Changes

```bash
git push
```

### Pull Latest Changes

```bash
git pull
```

---

## Task 4 - Create GitHub Repository

### Steps

1. Create GitHub account
2. Create a new repository
3. Push local Spring Boot project to GitHub

### Goal

Understand:
- Remote repositories
- Local vs remote code
- How developers share code

---

## Task 5 - Push End-to-End Mini Project to GitHub

### Requirements

Create a simple backend mini-project using:

- Java
- Spring Boot
- MySQL

### Features

- REST APIs
- CRUD operations
- Database integration

### Push Complete Project to GitHub

Ensure the repository contains:

- Source code
- `README.md`
- Proper project structure

---

# Final Outcome

By the end of this phase, the learner should be able to:

- Use Git for version control
- Work with GitHub repositories
- Commit and push code changes
- Create and manage branches
- Maintain project source code properly
- Host backend projects on GitHub