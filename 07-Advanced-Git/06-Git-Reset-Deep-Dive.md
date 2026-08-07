
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


