# Git Stash

## Introduction

The `git stash` command temporarily saves your uncommitted changes without creating a commit. It allows you to switch branches or pull updates while keeping your unfinished work safe.

It is one of the most frequently used Git commands in professional development.

---

# What is Git Stash?

Git Stash temporarily stores:

- Modified files
- Staged changes
- Untracked files (optional)

After stashing, your working directory becomes clean.

---

# Why Use Git Stash?

Developers use Stash to:

- Switch branches quickly
- Pull latest changes
- Handle urgent bug fixes
- Save unfinished work
- Avoid unnecessary commits

---

# Basic Command

```bash
git stash
```

This saves your current changes and restores the last committed version.

---

# View Saved Stashes

```bash
git stash list
```

Example:

```text
stash@{0}: WIP on feature-login
stash@{1}: WIP on main
```

---

# Restore the Latest Stash

```bash
git stash apply
```

The stash is restored but remains in the stash list.

---

# Restore and Remove the Stash

```bash
git stash pop
```

This restores the changes and removes the stash from the list.

---

# Delete a Stash

Delete the latest stash:

```bash
git stash drop
```

Delete all stashes:

```bash
git stash clear
```

---

# Save a Named Stash

```bash
git stash push -m "Login page work"
```

This makes it easier to identify later.

---

# Workflow

```text
Modify Files
      │
git stash
      │
Switch Branch
      │
Complete Work
      │
Return
      │
git stash pop
```

---

# Real-World Example

You are working on a new feature.

Suddenly, your manager asks you to fix a production bug immediately.

Instead of committing incomplete work:

```bash
git stash

git switch main
```

Fix the bug.

Later:

```bash
git switch feature-login

git stash pop
```

Continue your work exactly where you left off.

---

# Stash vs Commit

| Git Stash | Git Commit |
|-----------|------------|
| Temporary | Permanent |
| Local only | Saved in Git history |
| Used for unfinished work | Used for completed work |

---

# Best Practices

- Stash only unfinished work.
- Use meaningful stash messages.
- Apply or remove old stashes regularly.
- Verify your work after restoring a stash.

---

# Common Mistakes

- Forgetting about saved stashes.
- Using stash instead of commits.
- Clearing stashes accidentally.
- Restoring the wrong stash.

---

# Interview Questions

### What is Git Stash?

Git Stash temporarily saves uncommitted changes.

---

### Which command shows all stashes?

```bash
git stash list
```

---

### Which command restores and removes a stash?

```bash
git stash pop
```

---

### Which command restores without deleting the stash?

```bash
git stash apply
```

---

# Practice

1. Modify a file.
2. Run:

```bash
git stash
```

3. Check:

```bash
git stash list
```

4. Restore it:

```bash
git stash pop
```

5. Verify your changes.

---

# Summary

`git stash` is a powerful Git command that temporarily stores unfinished work without creating a commit. It allows developers to switch tasks quickly while keeping their changes safe, making it an essential part of everyday Git workflows.
