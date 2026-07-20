# Git Repository

## Introduction

A Git Repository (or Git Repo) is the central place where Git stores your project files, commit history, branches, tags, and other version control information. It allows developers to track changes, collaborate with others, and restore previous versions whenever required.

Every Git project starts with a repository.

---

# What is a Repository?

A repository is a storage location that contains:

- Project files
- Git history
- Branches
- Commit records
- Configuration files

Think of it as the complete database of your project.

---

# Types of Git Repositories

## 1. Local Repository

A Local Repository exists on your computer.

It contains:

- Source code
- Commit history
- Branches
- Tags

You can work completely offline.

---

## 2. Remote Repository

A Remote Repository is hosted on a server such as GitHub.

Examples:

- GitHub
- GitLab
- Bitbucket

It allows multiple developers to collaborate on the same project.

---

# Creating a New Repository

Navigate to your project folder.

Run:

```bash
git init
```

Example:

```bash
mkdir StudentManagementSystem
cd StudentManagementSystem

git init
```

Output:

```text
Initialized empty Git repository in ...
```

Git now creates a hidden folder named:

```text
.git
```

This folder stores all Git-related information.

---

# What is the .git Folder?

The `.git` folder is the heart of every Git repository.

It contains:

- Commit history
- Branch information
- Configuration
- References
- Objects
- Logs

**Never delete or modify the `.git` folder manually.**

Without it, Git will no longer recognize your project as a repository.

---

# Repository Structure

Example:

```text
StudentManagementSystem
│
├── .git
├── README.md
├── index.html
├── style.css
└── app.js
```

---

# Why Use a Repository?

Repositories help developers:

- Organize projects
- Track every change
- Restore previous versions
- Collaborate efficiently
- Maintain project history
- Manage branches

---

# Real-World Example

A company is developing an Online Banking Application.

The project contains:

- Login Module
- Payment Module
- Dashboard
- Reports

Instead of sharing files manually, the entire project is stored in a Git repository.

Every developer clones the repository, works on their assigned feature, commits changes, and pushes updates to the remote repository.

---

# Best Practices

- Create one repository for one project.
- Write a meaningful README.
- Use clear commit messages.
- Keep repositories organized.
- Push changes regularly.

---

# Common Mistakes

- Deleting the `.git` folder.
- Creating multiple unrelated projects in one repository.
- Uploading sensitive files such as passwords or API keys.
- Forgetting to initialize Git before making commits.

---

# Interview Questions

### What is a Git Repository?

A Git Repository is a storage location that contains project files and the complete version history of the project.

---

### What command creates a repository?

```bash
git init
```

---

### What is the purpose of the `.git` folder?

It stores all version control information, including commits, branches, and configuration.

---

### What are the two types of repositories?

- Local Repository
- Remote Repository

---

# Practice

1. Create a new folder.
2. Initialize it using `git init`.
3. Verify that the `.git` folder is created.
4. Add a `README.md` file.

---

# Summary

A Git Repository is the foundation of every Git project. It stores your code, project history, branches, and configuration, making version control possible. Understanding repositories is essential before learning how to stage, commit, and share code.
