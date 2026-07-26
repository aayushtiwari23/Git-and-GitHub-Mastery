# Git Branch

## Introduction

The `git branch` command is used to create, list, rename, and delete branches. Branches allow developers to work on different features or fixes without affecting the main branch.

---

# What is `git branch`?

A branch is a separate line of development.

Instead of working directly on `main`, developers create a new branch for each task.

Example:

```text
main
│
├── feature-login
├── feature-payment
└── bugfix-search
```

---

# Why Use Branches?

Branches help developers:

- Build new features
- Fix bugs safely
- Work in teams
- Test ideas
- Keep the `main` branch stable

---

# List All Local Branches

Command:

```bash
git branch
```

Example Output:

```text
* main
  feature-login
  feature-payment
```

The `*` indicates the currently active branch.

---

# Create a New Branch

Command:

```bash
git branch feature-login
```

This creates the branch but does not switch to it.

---

# Create Multiple Branches

```bash
git branch feature-dashboard

git branch feature-profile

git branch bugfix-login
```

---

# Rename a Branch

Rename the current branch:

```bash
git branch -m new-name
```

Example:

```bash
git branch -m feature-authentication
```

---

# Delete a Branch

Delete a merged branch:

```bash
git branch -d feature-login
```

Force delete:

```bash
git branch -D feature-login
```

---

# View All Branches

Local branches:

```bash
git branch
```

Remote branches:

```bash
git branch -r
```

All branches:

```bash
git branch -a
```

---

# Professional Workflow

```text
Create Branch
      │
Work on Feature
      │
Commit Changes
      │
Merge into main
      │
Delete Branch
```

---

# Branch Naming Convention

Use meaningful names.

Examples:

```text
feature-login

feature-payment

feature-dashboard

bugfix-navbar

hotfix-security

docs-readme

release-v1.0
```

Avoid names like:

```text
branch1

test

new

abc

demo
```

---

# Real-World Example

A company is developing a food delivery application.

Different developers create different branches:

```text
main

feature-order

feature-payment

feature-map

bugfix-cart
```

Each developer works independently until the feature is ready.

---

# Best Practices

- Create one branch for one task.
- Keep branch names meaningful.
- Delete merged branches.
- Avoid working directly on `main`.

---

# Common Mistakes

- Making all changes on `main`.
- Using confusing branch names.
- Keeping old branches forever.
- Mixing multiple features in one branch.

---

# Interview Questions

### What does `git branch` do?

It creates, lists, renames, and deletes branches.

---

### Which command lists all local branches?

```bash
git branch
```

---

### Which command creates a new branch?

```bash
git branch feature-login
```

---

### Which command deletes a merged branch?

```bash
git branch -d feature-login
```

---

# Practice

1. List existing branches.

```bash
git branch
```

2. Create a branch.

```bash
git branch feature-profile
```

3. List branches again.

4. Rename the branch.

5. Delete the branch.

---

# Summary

The `git branch` command is the foundation of Git branching. It allows developers to organize work efficiently, collaborate safely, and keep projects clean by separating features and fixes into independent branches.
