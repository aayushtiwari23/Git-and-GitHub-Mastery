# Git Commit

## Introduction

A commit is a snapshot of your project at a specific point in time. Every commit records the changes made since the previous commit and stores them permanently in the repository history.

Think of a commit as a checkpoint in a game. If something goes wrong later, you can always return to that checkpoint.

---

# What is a Commit?

A commit contains:

- Project changes
- Commit message
- Author name
- Author email
- Date and time
- Unique Commit ID (SHA)

Every commit has a unique SHA (Secure Hash Algorithm) that identifies it.

Example:

```text
9fceb02
```

No two commits have the same SHA.

---

# Git Workflow

```text
Working Directory
        │
        │ git add
        ▼
Staging Area
        │
        │ git commit
        ▼
Local Repository
        │
        │ git push
        ▼
GitHub Repository
```

---

# Command Syntax

```bash
git commit -m "Commit Message"
```

Example:

```bash
git commit -m "Add login functionality"
```

---

# What Happens After a Commit?

Git:

- Creates a snapshot
- Saves project history
- Generates a unique SHA
- Records author information
- Stores the commit message

---

# Anatomy of a Commit

```text
Commit SHA
│
├── Author
├── Date
├── Message
└── Project Snapshot
```

---

# Good Commit Messages

A commit message should clearly describe the change.

✅ Good Examples

```text
Add user authentication

Fix login validation bug

Update README documentation

Create SQL practice examples

Implement search functionality
```

❌ Bad Examples

```text
Update

Changes

Done

Final

Testing
```

---

# Conventional Commits

Many companies follow a standard format.

Examples:

```text
feat: add login page

fix: resolve payment bug

docs: update README

style: format source code

refactor: improve authentication logic

test: add unit tests

chore: update dependencies
```

Learning this format early is a good habit.

---

# View Commit History

```bash
git log
```

Example:

```text
commit 9fceb02
Author: Aayush Suraj Tiwari
Date: ...

Add login functionality
```

---

# Professional Workflow

```bash
git status

git add .

git status

git commit -m "feat: add login page"

git push
```

---

# Developer Tip

One commit should represent **one logical change**.

Instead of:

```text
Login
Payment
Dashboard
README
```

Make separate commits:

```text
feat: add login page

feat: implement payment module

docs: update README
```

This creates a clean and understandable history.

---

# Best Practices

- Write meaningful commit messages.
- Commit small, complete changes.
- Commit frequently.
- Follow Conventional Commits when possible.
- Review changes before committing.

---

# Common Mistakes

- Writing vague commit messages.
- Committing unfinished work.
- Creating one huge commit for many unrelated changes.
- Forgetting to review staged files.

---

# Interview Questions

### What is a Git Commit?

A commit is a snapshot of the project at a specific point in time.

---

### Which command creates a commit?

```bash
git commit -m "message"
```

---

### Why are commit messages important?

They explain the purpose of each change and make project history easier to understand.

---

### What is a SHA?

A SHA is the unique identifier assigned to every Git commit.

---

# Practice

1. Create a new file.
2. Stage it.

```bash
git add .
```

3. Commit it.

```bash
git commit -m "feat: add practice file"
```

4. View the history.

```bash
git log
```

---

# Summary

A Git commit permanently records staged changes in your repository. Well-written commits create a clear project history, simplify collaboration, and make debugging and maintenance much easier.
