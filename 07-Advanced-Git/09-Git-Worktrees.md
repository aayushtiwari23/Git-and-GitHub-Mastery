
# Git Worktrees

## Introduction

Git Worktrees allow you to work on multiple Git branches simultaneously using separate directories.

Normally, Git allows only one branch to be checked out in a working directory at a time. With Worktrees, you can have multiple branches checked out simultaneously without repeatedly switching branches.

---

# What is a Git Worktree?

A Git Worktree is an additional working directory connected to the same Git repository.

Example:

```text
project/
│
├── main/
│   └── Production Code
│
├── feature-login/
│   └── Login Development
│
└── bugfix-payment/
    └── Payment Bug Fix
```

All three directories can belong to the same Git repository.

---

# Why Use Git Worktrees?

Worktrees are useful when you need to:

- Work on multiple branches simultaneously.
- Quickly switch between features.
- Fix an urgent production bug.
- Compare branches.
- Run different versions of an application.
- Avoid repeatedly stashing unfinished work.

---

# Traditional Workflow

Without Worktrees:

```text
Feature Branch
      │
      ▼
git stash
      │
      ▼
git switch main
      │
      ▼
Fix Bug
      │
      ▼
git switch feature
      │
      ▼
git stash pop
```

This can become inconvenient.

---

# Worktree Workflow

With Worktrees:

```text
Main Directory
     │
     ├── main
     │
     ├── feature-login
     │
     └── bugfix-payment
```

Each branch has its own directory.

No repeated branch switching is required.

---

# Create a Worktree

Basic command:

```bash
git worktree add <path> <branch>
```

Example:

```bash
git worktree add ../feature-login feature-login
```

Git creates:

```text
../feature-login
```

and checks out the `feature-login` branch there.

---

# Create a New Branch and Worktree

You can create a new branch at the same time:

```bash
git worktree add -b feature-dashboard ../feature-dashboard
```

This:

1. Creates the branch.
2. Creates the worktree.
3. Checks out the new branch.

---

# List Worktrees

```bash
git worktree list
```

Example:

```text
/home/user/project       a1b2c3d [main]
/home/user/feature-login d4e5f6g [feature-login]
/home/user/bugfix        7h8i9j0 [bugfix-payment]
```

---

# Move Into a Worktree

```bash
cd ../feature-login
```

Now you are working on:

```text
feature-login
```

independently from the main directory.

---

# Remove a Worktree

First leave the directory.

Then:

```bash
git worktree remove ../feature-login
```

The branch itself is not automatically deleted.

---

# Prune Worktree Information

If a worktree directory was manually deleted:

```bash
git worktree prune
```

This cleans up stale Worktree metadata.

---

# Real-World Example

Imagine you're working on:

```text
feature-dashboard
```

Suddenly, production has a critical bug.

Instead of stashing your dashboard work:

```bash
git worktree add ../hotfix hotfix-payment
```

Now you have:

```text
project/
    → feature-dashboard

../hotfix/
    → hotfix-payment
```

You can fix the production issue while continuing your dashboard work.

---

# Worktree Structure

```text
                    Git Repository
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
        main       feature-login    bugfix-payment
          │              │              │
          ▼              ▼              ▼
       Folder A       Folder B       Folder C
```

---

# Advantages

- Multiple branches available simultaneously.
- No unnecessary stashing.
- Faster branch switching.
- Useful for emergency fixes.
- Great for large projects.

---

# Limitations

- Requires additional disk space.
- Multiple working directories can be confusing.
- A branch generally cannot be checked out in two worktrees simultaneously.
- Requires understanding of multiple working directories.

---

# Worktree vs Clone

| Worktree | Clone |
|----------|-------|
| Shares one repository | Creates another repository |
| Shares Git history | Separate repository metadata |
| Saves disk space | Uses more disk space |
| Multiple branches | Usually one working branch |
| Fast setup | More independent |

---

# Common Commands

| Command | Purpose |
|---------|---------|
| `git worktree add` | Create a worktree |
| `git worktree list` | List worktrees |
| `git worktree remove` | Remove a worktree |
| `git worktree prune` | Clean stale metadata |
| `git worktree lock` | Lock a worktree |
| `git worktree unlock` | Unlock a worktree |

---

# Best Practices

- Use descriptive worktree directory names.
- Remove unused worktrees.
- Use Worktrees for parallel development.
- Keep track of which branch belongs to each directory.
- Use Worktrees for urgent fixes instead of unnecessarily stashing work.

---

# Common Mistakes

- Forgetting which directory contains which branch.
- Trying to check out the same branch in multiple worktrees.
- Manually deleting worktrees without cleaning metadata.
- Keeping unnecessary worktrees for too long.

---

# Interview Questions

### What is Git Worktree?

Git Worktree allows multiple working directories to be connected to the same Git repository, each checking out a different branch.

---

### Which command creates a Worktree?

```bash
git worktree add
```

---

### How do you list Worktrees?

```bash
git worktree list
```

---

### How do you remove a Worktree?

```bash
git worktree remove <path>
```

---

### Why use Worktrees instead of cloning the repository multiple times?

Worktrees share the same repository data and Git history, reducing duplication while allowing multiple branches to be checked out simultaneously.

---
