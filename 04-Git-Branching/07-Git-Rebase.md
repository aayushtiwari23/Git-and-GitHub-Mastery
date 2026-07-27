# Git Rebase

## Introduction

`git rebase` is used to move or combine a sequence of commits onto another branch. Unlike `git merge`, which creates a merge commit, `git rebase` rewrites commit history to produce a cleaner and more linear project history.

Many professional development teams use rebase before merging feature branches.

---

# What is Git Rebase?

Rebase changes the base of a branch.

Instead of creating a merge commit, Git replays your commits on top of another branch.

Example:

```text
Before Rebase

main
A --- B --- C

          \
           D --- E (feature)
```

After Rebase

```text
main
A --- B --- C --- D' --- E'
```

The commits are replayed on top of the latest `main`.

---

# Why Use Rebase?

Developers use rebase to:

- Keep commit history clean.
- Update feature branches.
- Reduce unnecessary merge commits.
- Make project history easier to read.

---

# Basic Command

```bash
git rebase main
```

This replays the current branch on top of `main`.

---

# Common Workflow

### Step 1

Switch to your feature branch.

```bash
git switch feature-login
```

---

### Step 2

Update the branch.

```bash
git rebase main
```

---

### Step 3

Resolve conflicts if Git reports any.

---

### Step 4

Continue the rebase.

```bash
git rebase --continue
```

---

### Step 5

If needed, cancel the rebase.

```bash
git rebase --abort
```

---

# Merge vs Rebase

| Merge | Rebase |
|-------|--------|
| Creates a merge commit | Rewrites commit history |
| Preserves original history | Creates a cleaner history |
| Easier for beginners | More advanced |
| Safe for shared branches | Best before sharing |

---

# Real-World Example

A developer creates a branch:

```text
feature-payment
```

Meanwhile, the `main` branch receives several new commits.

Before opening a Pull Request, the developer runs:

```bash
git rebase main
```

Now the feature branch contains the latest changes from `main` with a cleaner history.

---

# Best Practices

- Rebase your own feature branches.
- Resolve conflicts carefully.
- Test the project after rebasing.
- Use rebase before creating a Pull Request.

---

# Common Mistakes

- Rebasing shared branches.
- Ignoring conflicts.
- Forgetting to test after rebasing.
- Force pushing without understanding the consequences.

---

# Interview Questions

### What is Git Rebase?

Git Rebase moves or replays commits onto another branch to create a cleaner project history.

---

### What is the main difference between Merge and Rebase?

Merge creates a merge commit.

Rebase rewrites commit history.

---

### Which command continues a paused rebase?

```bash
git rebase --continue
```

---

### Which command cancels a rebase?

```bash
git rebase --abort
```

---

# Practice

1. Create a feature branch.

2. Make two commits.

3. Add a new commit to `main`.

4. Switch back to the feature branch.

5. Run:

```bash
git rebase main
```

6. Resolve any conflicts.

7. Continue the rebase.

---

# Summary

`git rebase` is an advanced Git command used to create a clean and linear commit history. It is widely used in professional software development and is a common topic in technical interviews.
