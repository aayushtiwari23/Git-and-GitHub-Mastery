# Git Cherry-Pick

## Introduction

The `git cherry-pick` command allows you to copy a specific commit from one branch and apply it to another branch without merging the entire branch.

It is useful when you need only one or a few commits instead of all the changes.

---

# What is Git Cherry-Pick?

Cherry-pick copies an individual commit from one branch to another.

Example:

```text
main
A --- B --- C

feature
      \
       D --- E --- F
```

If you cherry-pick commit **E**, the result becomes:

```text
main
A --- B --- C --- E'
```

Only commit **E** is copied.

---

# Why Use Cherry-Pick?

Developers use Cherry-Pick to:

- Copy a bug fix.
- Move a specific feature.
- Reuse an important commit.
- Avoid merging unwanted commits.

---

# Basic Command

```bash
git cherry-pick <commit-hash>
```

Example:

```bash
git cherry-pick a1b2c3d
```

---

# Find the Commit Hash

Run:

```bash
git log
```

Example:

```text
a1b2c3d Fix login bug

b2c3d4e Add dashboard

c3d4e5f Update README
```

Copy the required commit hash.

---

# Cherry-Pick Workflow

### Step 1

Find the commit.

```bash
git log
```

---

### Step 2

Switch to the destination branch.

```bash
git switch main
```

---

### Step 3

Apply the commit.

```bash
git cherry-pick a1b2c3d
```

Git copies that commit into the current branch.

---

# Real-World Example

A developer fixes a security bug in the `feature-payment` branch.

Instead of merging the entire branch, only the bug fix commit is copied to `main` using:

```bash
git cherry-pick a1b2c3d
```

---

# Cherry-Pick vs Merge

| Cherry-Pick | Merge |
|-------------|-------|
| Copies selected commits | Combines the entire branch |
| Good for individual fixes | Good for completed features |
| Does not merge branch history | Merges branch history |

---

# Best Practices

- Cherry-pick only necessary commits.
- Test after cherry-picking.
- Use commit messages that clearly describe changes.
- Verify the copied commit using `git log`.

---

# Common Mistakes

- Cherry-picking the wrong commit.
- Forgetting to test after copying.
- Creating duplicate changes.
- Cherry-picking many commits instead of merging.

---

# Interview Questions

### What is Git Cherry-Pick?

It copies a specific commit from one branch to another.

---

### Which command is used?

```bash
git cherry-pick <commit-hash>
```

---

### How do you find a commit hash?

```bash
git log
```

---

### When should you use Cherry-Pick?

When only one or a few commits are needed from another branch.

---

# Practice

1. Create two branches.
2. Make a commit on one branch.
3. Copy the commit hash.
4. Switch to another branch.
5. Run:

```bash
git cherry-pick <commit-hash>
```

6. Verify the copied commit using:

```bash
git log
```

---

# Summary

`git cherry-pick` is a useful Git command for copying specific commits between branches without merging the entire branch. It is commonly used for bug fixes, hotfixes, and selective code sharing in professional development.
