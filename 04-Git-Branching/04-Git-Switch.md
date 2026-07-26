
# Git Switch

## Introduction

The `git switch` command was introduced to make branch switching simpler and safer. Unlike `git checkout`, which performs multiple tasks, `git switch` focuses only on switching between branches.

It is the recommended command for modern Git workflows.

---

# What is `git switch`?

`git switch` is used to:

- Switch between existing branches
- Create and switch to a new branch

It does **not** restore files or check out commits.

---

# Why Use `git switch`?

Advantages:

- Easier to understand
- Less chance of mistakes
- Dedicated only to branch management
- Recommended for beginners

---

# Switch to an Existing Branch

Command:

```bash
git switch main
```

or

```bash
git switch feature-login
```

---

# Create and Switch to a New Branch

Command:

```bash
git switch -c feature-profile
```

This command:

- Creates the branch
- Switches to it immediately

---

# Switch Back

```bash
git switch main
```

---

# Workflow

```text
main
 │
 ├── git switch -c feature-login
 │
 ▼
feature-login
 │
Make Changes
 │
Commit Changes
 │
git switch main
```

---

# Git Switch vs Git Checkout

| Git Switch | Git Checkout |
|------------|--------------|
| Only switches branches | Switches branches and performs other tasks |
| Easier for beginners | More powerful but more complex |
| Modern command | Older command |

---

# Real-World Example

A developer starts working on a new feature.

Instead of using:

```bash
git checkout -b feature-payment
```

They use:

```bash
git switch -c feature-payment
```

After completing the work:

```bash
git switch main
```

---

# Best Practices

- Commit your work before switching branches.
- Use meaningful branch names.
- Verify the active branch using `git branch`.

---

# Common Mistakes

- Confusing `git switch` with `git checkout`.
- Forgetting to commit changes before switching.
- Creating unnecessary branches.

---

# Interview Questions

### What is `git switch`?

A command used to switch between Git branches.

---

### Which command creates and switches to a new branch?

```bash
git switch -c feature-login
```

---

### Which command switches back to the main branch?

```bash
git switch main
```

---

### Why was `git switch` introduced?

To provide a simpler and safer way to switch branches compared to `git checkout`.

---

# Practice

1. View your current branch.

```bash
git branch
```

2. Create and switch to a new branch.

```bash
git switch -c feature-profile
```

3. Switch back to `main`.

```bash
git switch main
```

4. Switch again to `feature-profile`.

---

# Summary

The `git switch` command is the modern and recommended way to move between Git branches. It simplifies branch management, reduces confusion, and is widely used in current Git workflows.
