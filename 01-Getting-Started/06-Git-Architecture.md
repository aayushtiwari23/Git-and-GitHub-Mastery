# Git Architecture

## Introduction

Before using Git commands, it is important to understand how Git works internally. Every file you create or modify passes through different stages before becoming part of your project's history.

Understanding Git Architecture helps developers use Git confidently and avoid common mistakes while managing projects.

---

# Git Workflow

```
                Git Architecture

        Working Directory
                │
        git add │
                ▼
          Staging Area
                │
     git commit │
                ▼
       Local Repository
                │
      git push  │
                ▼
     Remote Repository (GitHub)
```

Every change you make follows this workflow.

---

# Components of Git Architecture

## 1. Working Directory

The Working Directory is the folder where you create, edit, and delete files.

Whenever you modify a file, Git first detects the changes in the Working Directory.

### Example

```
Project
│
├── index.html
├── style.css
└── app.js
```

Suppose you edit `app.js`.

The changes exist only in the Working Directory until you tell Git to track them.

---

## 2. Staging Area

The Staging Area acts as a temporary checkpoint.

Instead of committing every file immediately, Git allows you to choose which files should be included in the next commit.

Command:

```bash
git add .
```

or

```bash
git add app.js
```

After running `git add`, Git moves the selected changes into the Staging Area.

---

## 3. Local Repository

The Local Repository stores the complete history of your project on your computer.

Once changes are staged, they are permanently recorded using:

```bash
git commit -m "Add login validation"
```

Each commit creates a snapshot of your project.

Every commit has:

- Unique ID (SHA)
- Commit message
- Author
- Date and time

---

## 4. Remote Repository

A Remote Repository is hosted on platforms like GitHub.

After committing locally, upload your changes using:

```bash
git push origin main
```

Now your code is safely stored online and can be accessed by other developers.

---

# Complete Flow Example

Imagine you create a new file.

```
notes.md
```

### Step 1

Create or edit the file.

↓

Working Directory

### Step 2

Run

```bash
git add notes.md
```

↓

Staging Area

### Step 3

Run

```bash
git commit -m "Add notes"
```

↓

Local Repository

### Step 4

Run

```bash
git push origin main
```

↓

GitHub Repository

---

# Why Does Git Have a Staging Area?

Many beginners wonder why Git doesn't commit files directly.

The Staging Area provides flexibility.

Example:

Suppose you changed three files.

```
login.js
payment.js
README.md
```

Only `login.js` is complete.

You can stage only that file:

```bash
git add login.js
```

Then commit only the finished work.

This keeps commits clean and meaningful.

---

# Advantages of Git Architecture

- Organized workflow
- Better project history
- Selective commits
- Easy collaboration
- Easy rollback
- Faster debugging
- Professional development process

---

# Real-World Example

A developer builds a Food Delivery application.

1. Creates a new Login page.
2. Tests the feature.
3. Adds only the completed files.
4. Creates a commit.
5. Pushes the changes to GitHub.

Other developers can now download the latest version without affecting their own work.

---

# Common Beginner Mistakes

- Forgetting to stage files
- Writing poor commit messages
- Pushing without committing
- Committing unfinished work
- Editing directly on the main branch

---

# Interview Questions

### What are the four stages of Git Architecture?

- Working Directory
- Staging Area
- Local Repository
- Remote Repository

---

### What is the purpose of the Staging Area?

It allows developers to select specific changes before creating a commit.

---

### What command moves files to the Staging Area?

```bash
git add
```

---

### Which command creates a snapshot?

```bash
git commit
```

---

### Which command uploads commits to GitHub?

```bash
git push
```

---

# Summary

Git Architecture consists of four main stages:

Working Directory → Staging Area → Local Repository → Remote Repository

Understanding this workflow is essential because almost every Git command operates on one of these stages. Once you understand this architecture, learning Git commands becomes much easier.
