
# Git Reset (Deep Dive)

## Introduction

Git Reset is one of the most powerful and misunderstood commands in Git. It is used to move the current branch (HEAD) to another commit and optionally modify the staging area (Index) and working directory.

Understanding Git Reset is essential because it allows developers to undo mistakes, reorganize commits, and recover from accidental changes.

---

# What is Git Reset?

Git Reset changes where the current branch points.

Depending on the option used, it can affect:

- HEAD
- Staging Area (Index)
- Working Directory

---

# The Three Git Areas

Before learning Git Reset, understand Git's three areas.

```text
Working Directory
        │
git add
        ▼
Staging Area (Index)
        │
git commit
        ▼
Git Repository (HEAD)
```

---

# Types of Git Reset

Git provides three primary reset modes:

- Soft Reset
- Mixed Reset (Default)
- Hard Reset

---

# 1. Soft Reset

Command:

```bash
git reset --soft HEAD~1
```

## What Happens?

- HEAD moves ✔
- Staging Area remains ✔
- Working Directory remains ✔

Diagram:

```text
Before

Repository
A → B → C (HEAD)

After

Repository
A → B (HEAD)

Working Directory ✔

Staging Area ✔
```

The changes from commit **C** remain staged.

---

### When to Use

- Edit the last commit.
- Combine commits.
- Rewrite commit messages.

---

# 2. Mixed Reset (Default)

Command:

```bash
git reset HEAD~1
```

or

```bash
git reset --mixed HEAD~1
```

## What Happens?

- HEAD moves ✔
- Staging Area resets ✔
- Working Directory remains ✔

Diagram:

```text
Repository
A → B (HEAD)

Working Directory ✔

Staging Area ✘
```

The changes become unstaged.

---

### When to Use

- Unstage files.
- Reorganize commits.
- Modify changes before committing again.

---

# 3. Hard Reset

Command:

```bash
git reset --hard HEAD~1
```

## What Happens?

- HEAD moves ✔
- Staging Area resets ✔
- Working Directory resets ✔

Diagram:

```text
Repository
A → B (HEAD)

Working Directory ✘

Staging Area ✘
```

All uncommitted changes are permanently removed.

---

### When to Use

- Discard local changes.
- Restore repository to a previous commit.
- Remove unwanted work.

---

# Comparison Table

| Feature | Soft | Mixed | Hard |
|----------|------|--------|------|
| Moves HEAD | ✔ | ✔ | ✔ |
| Resets Staging Area | ✘ | ✔ | ✔ |
| Resets Working Directory | ✘ | ✘ | ✔ |
| Deletes Uncommitted Changes | ✘ | ✘ | ✔ |

---

# Reset to a Specific Commit

Example:

```bash
git log --oneline
```

Output:

```text
a1b2c3d Add Login

d4e5f6g Update README

7h8i9j0 Initial Commit
```

Reset:

```bash
git reset --soft d4e5f6g
```

HEAD now points to:

```text
Update README
```

---

# Undo the Last Commit

Keep changes staged:

```bash
git reset --soft HEAD~1
```

Keep changes but unstage them:

```bash
git reset HEAD~1
```

Delete everything:

```bash
git reset --hard HEAD~1
```

---

# Recover After Hard Reset

Suppose you accidentally run:

```bash
git reset --hard HEAD~3
```

Recover using:

```bash
git reflog
```

Find:

```text
HEAD@{1}
```

Restore:

```bash
git reset --hard HEAD@{1}
```

---

# Real-World Example

A developer commits:

```text
Fix Login

Update README

Add Dashboard
```

The last commit contains mistakes.

Options:

Soft Reset:

```bash
git reset --soft HEAD~1
```

Edit the commit.

Mixed Reset:

```bash
git reset HEAD~1
```

Modify files before committing again.

Hard Reset:

```bash
git reset --hard HEAD~1
```

Remove the commit completely.

---

# Advantages

- Undo commits.
- Reorganize commit history.
- Fix mistakes.
- Prepare clean commits.
- Restore previous repository states.

---

# Best Practices

- Use Soft Reset for rewriting commits.
- Use Mixed Reset for unstaging files.
- Use Hard Reset carefully.
- Check `git status` before resetting.
- Learn Git Reflog for recovery.

---

# Common Mistakes

- Using Hard Reset without backups.
- Confusing Mixed and Soft Reset.
- Forgetting that Hard Reset removes uncommitted work.
- Resetting shared branches.

---

# Reset vs Revert

| Git Reset | Git Revert |
|------------|------------|
| Rewrites history | Preserves history |
| Removes commits | Creates a new commit |
| Better for local branches | Better for shared branches |
| Can lose work | Safe for collaboration |

---

# Interview Questions

### What is Git Reset?

Git Reset moves HEAD to another commit and may also modify the staging area and working directory.

---

### Which reset keeps changes staged?

```bash
git reset --soft
```

---

### Which reset unstages files?

```bash
git reset
```

or

```bash
git reset --mixed
```

---

### Which reset removes all local changes?

```bash
git reset --hard
```

---

### How can you recover after a Hard Reset?

Using:

```bash
git reflog
```

---

# Practice

1. Create three commits.
2. Run:

```bash
git reset --soft HEAD~1
```

3. Observe:

```bash
git status
```

4. Repeat with:

```bash
git reset --mixed HEAD~1
```

5. Finally, test:

```bash
git reset --hard HEAD~1
```

6. Recover the lost commit using:

```bash
git reflog
```

---

# Summary

Git Reset is a powerful command that allows developers to move HEAD and optionally modify the staging area and working directory. Understanding the differences between **Soft**, **Mixed**, and **Hard** Reset is essential for safely managing commits, correcting mistakes, and maintaining a clean Git history. Combined with **Git Reflog**, it becomes one of the most valuable recovery and history-management tools in Git.
