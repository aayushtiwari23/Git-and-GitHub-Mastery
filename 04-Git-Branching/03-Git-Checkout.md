
# Git Checkout

## Introduction

The `git checkout` command is one of Git's oldest and most widely used commands. It is used to switch between branches, restore files, and move to specific commits.

Although Git now provides `git switch` and `git restore` for better clarity, `git checkout` is still widely used in existing projects and interviews.

---

# What is `git checkout`?

The `git checkout` command changes your current working branch or restores files from another commit or branch.

It can:

- Switch branches
- Create and switch to a new branch
- Restore files
- View old commits

---

# Switch to Another Branch

Command:

```bash
git checkout feature-login
```

Git changes the current branch from `main` to `feature-login`.

---

# Switch Back to Main

```bash
git checkout main
```

---

# Create and Switch to a New Branch

Instead of creating and then switching separately:

```bash
git branch feature-profile

git checkout feature-profile
```

Use:

```bash
git checkout -b feature-profile
```

This creates the branch and switches to it immediately.

---

# Restore a File

Suppose you accidentally modified a file.

Restore it using:

```bash
git checkout -- README.md
```

The file returns to its last committed version.

---

# Checkout a Specific Commit

View an older version of the project:

```bash
git checkout a1b2c3d
```

This places Git in a **Detached HEAD** state.

To return:

```bash
git checkout main
```

---

# Workflow

```text
main
   │
git checkout feature-login
   │
Work on Feature
   │
Commit Changes
   │
git checkout main
```

---

# Checkout vs Branch

| git branch | git checkout |
|------------|--------------|
| Creates a branch | Switches branches |
| Does not change current branch | Changes current branch |

---

# Real-World Example

A developer finishes working on:

```text
feature-payment
```

They now need to fix a bug on the `main` branch.

Command:

```bash
git checkout main
```

After fixing the bug, they switch back:

```bash
git checkout feature-payment
```

---

# Best Practices

- Commit or stash your work before switching branches.
- Use meaningful branch names.
- Verify your current branch using `git branch`.

---

# Common Mistakes

- Switching branches with uncommitted changes.
- Forgetting which branch is active.
- Confusing `git checkout` with `git branch`.

---

# Interview Questions

### What does `git checkout` do?

It switches branches, restores files, or checks out previous commits.

---

### Which command creates and switches to a new branch?

```bash
git checkout -b feature-login
```

---

### Which command switches back to the main branch?

```bash
git checkout main
```

---

### Is `git checkout` still used?

Yes. Although `git switch` and `git restore` are recommended for newer workflows, `git checkout` is still widely used in existing projects and interviews.

---

# Practice

1. View your current branch.

```bash
git branch
```

2. Create and switch to a new branch.

```bash
git checkout -b feature-profile
```

3. Switch back to `main`.

```bash
git checkout main
```

4. Switch again to `feature-profile`.

---

# Summary

The `git checkout` command is a powerful Git command used to switch branches, restore files, and inspect previous commits. Understanding it is essential because it remains common in professional projects and technical interviews.
