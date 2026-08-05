
# Git Rebase

## Introduction

Git Rebase is an advanced Git command used to move or replay commits from one branch onto another. Instead of creating a merge commit, rebase rewrites commit history to produce a clean and linear project history.

Professional software teams often use Git Rebase before merging feature branches to keep the commit history organized and easier to understand.

---

# What is Git Rebase?

Git Rebase takes commits from one branch and reapplies them on top of another branch.

Instead of combining histories with a merge commit, Git creates a straight timeline.

---

# Why Use Git Rebase?

Developers use Git Rebase to:

- Keep commit history clean.
- Reduce unnecessary merge commits.
- Update feature branches with the latest changes.
- Make project history easier to read.
- Prepare commits before opening a Pull Request.

---

# Merge vs Rebase

## Merge

```text
A──B──C (main)
     \
      D──E (feature)
           \
            M (Merge Commit)
```

A merge creates an extra merge commit.

---

## Rebase

```text
A──B──C (main)
        \
         D'──E' (feature)
```

The commits are replayed on top of the latest `main`, producing a linear history.

---

# Basic Rebase Command

Suppose you are on a feature branch:

```bash
git rebase main
```

Git moves your feature branch commits on top of the latest `main`.

---

# Typical Workflow

## Step 1

Update the main branch.

```bash
git switch main

git pull origin main
```

---

## Step 2

Switch back to your feature branch.

```bash
git switch feature-login
```

---

## Step 3

Rebase onto the latest `main`.

```bash
git rebase main
```

---

## Step 4

If no conflicts occur, the rebase completes successfully.

---

# Handling Merge Conflicts

Sometimes Git cannot automatically replay a commit.

Git pauses the rebase and reports a conflict.

Resolve the conflict manually.

After fixing the files:

```bash
git add .
```

Continue:

```bash
git rebase --continue
```

---

# Abort a Rebase

If something goes wrong:

```bash
git rebase --abort
```

Git restores the branch to its previous state.

---

# Skip a Commit

If a commit is causing problems:

```bash
git rebase --skip
```

Git skips that commit and continues.

---

# Interactive Rebase

Interactive Rebase allows you to edit commit history.

Command:

```bash
git rebase -i HEAD~5
```

This opens the last five commits for editing.

You can:

- Reorder commits
- Rename commit messages
- Squash commits
- Delete commits
- Split commits

---

# Interactive Rebase Commands

| Command | Purpose |
|---------|---------|
| pick | Keep the commit |
| reword | Edit the commit message |
| edit | Modify the commit |
| squash | Combine with previous commit |
| fixup | Combine without keeping the message |
| drop | Remove the commit |

---

# Real-World Example

You create three commits:

```text
Fix typo

Update README

Correct formatting
```

Before opening a Pull Request, you run:

```bash
git rebase -i HEAD~3
```

Then squash them into:

```text
Improve project documentation
```

Your commit history becomes cleaner and easier to review.

---

# Rebase Workflow

```text
main
 │
 ├────A────B────C
 │
 └────D────E (feature)

git rebase main

main
 │
 ├────A────B────C────D'────E'
```

---

# Advantages

- Clean and linear history.
- Easier code reviews.
- Fewer merge commits.
- Better project organization.
- Simpler debugging.

---

# Best Practices

- Rebase feature branches before opening a Pull Request.
- Pull the latest changes before rebasing.
- Use Interactive Rebase to clean commits.
- Resolve conflicts carefully.
- Rebase only your own local branches.

---

# Common Mistakes

- Rebasing shared branches.
- Forgetting to pull before rebasing.
- Ignoring merge conflicts.
- Force-pushing without understanding the consequences.

---

# Important Warning

Avoid rebasing branches that other developers are actively using.

Rebasing rewrites commit history, which can create confusion and conflicts for collaborators.

A common guideline is:

> **Rebase local branches. Merge shared branches.**

---

# Interview Questions

### What is Git Rebase?

Git Rebase reapplies commits from one branch onto another to create a clean, linear history.

---

### What is the difference between Merge and Rebase?

Merge creates a merge commit.

Rebase rewrites commit history without creating a merge commit.

---

### Which command starts an interactive rebase?

```bash
git rebase -i HEAD~5
```

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
2. Make three small commits.
3. Update the `main` branch.
4. Run:

```bash
git rebase main
```

5. Resolve any conflicts if they occur.
6. Try:

```bash
git rebase -i HEAD~3
```

7. Squash the three commits into one.
8. Verify the commit history with:

```bash
git log --oneline
```

---

# Summary

Git Rebase is a powerful Git command that creates a clean and linear commit history by replaying commits onto another branch. It is widely used in professional software development to simplify project history, improve code reviews, and maintain an organized repository. Developers should use it carefully, especially when collaborating on shared branches.
