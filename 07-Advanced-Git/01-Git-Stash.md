# Git Stash

## Introduction

While working on a project, you may sometimes need to switch to another task before finishing your current work. However, your changes are not yet ready to be committed.

Git provides the `git stash` command to temporarily save these uncommitted changes, allowing you to work on something else and return later without losing your progress.

Git Stash acts like a temporary storage area for your unfinished work.

---

# What is Git Stash?

Git Stash temporarily saves your:

- Modified tracked files
- Staged changes
- (Optionally) untracked files

After stashing, your working directory returns to the last committed state.

---

# Why Use Git Stash?

Git Stash is useful when you need to:

- Switch branches quickly
- Fix an urgent bug
- Pull the latest changes
- Experiment without committing
- Keep commit history clean

---

# Basic Workflow

```text
Modify Files
      │
      ▼
git stash
      │
      ▼
Clean Working Directory
      │
      ▼
Switch Branch / Fix Bug
      │
      ▼
Return
      │
      ▼
git stash pop
      │
      ▼
Continue Working
```

---

# Save Changes

```bash
git stash
```

Git saves your current work and restores the last committed version.

---

# Save with a Message

```bash
git stash push -m "Work on login page"
```

Example:

```bash
git stash push -m "Navbar redesign"
```

---

# View All Stashes

```bash
git stash list
```

Example:

```text
stash@{0}: On main: Navbar redesign

stash@{1}: On feature-login: Login validation
```

---

# View Stash Details

```bash
git stash show
```

To view all changes:

```bash
git stash show -p
```

---

# Restore the Latest Stash

```bash
git stash apply
```

The stash is restored but remains in the stash list.

---

# Restore and Remove the Latest Stash

```bash
git stash pop
```

This is the most commonly used command.

---

# Restore a Specific Stash

```bash
git stash apply stash@{1}
```

---

# Delete a Stash

```bash
git stash drop stash@{0}
```

---

# Delete All Stashes

```bash
git stash clear
```

Use this carefully because all stored stashes are permanently removed.

---

# Stash Untracked Files

Normally, untracked files are not included.

To include them:

```bash
git stash -u
```

or

```bash
git stash --include-untracked
```

---

# Stash Everything

Including ignored files:

```bash
git stash -a
```

---

# Real-World Example

You're building a Dashboard feature.

Suddenly, a production bug needs immediate attention.

Instead of committing incomplete work:

```bash
git stash
```

Switch branches:

```bash
git switch main
```

Fix the bug.

After finishing:

```bash
git switch feature-dashboard

git stash pop
```

Continue working exactly where you left off.

---

# Common Git Stash Commands

| Command | Purpose |
|---------|---------|
| `git stash` | Save current changes |
| `git stash list` | View all stashes |
| `git stash show` | Show stash summary |
| `git stash show -p` | Show full changes |
| `git stash apply` | Restore stash |
| `git stash pop` | Restore and delete stash |
| `git stash drop` | Delete one stash |
| `git stash clear` | Delete all stashes |

---

# Best Practices

- Add messages when creating stashes.
- Keep the stash list organized.
- Remove old stashes.
- Use `pop` if you no longer need the stash.
- Don't use stashes as permanent storage.

---

# Common Mistakes

- Forgetting stashed work.
- Keeping dozens of old stashes.
- Accidentally clearing all stashes.
- Confusing `apply` with `pop`.

---

# Interview Questions

### What is Git Stash?

Git Stash temporarily saves uncommitted changes so you can work on something else.

---

### What is the difference between `git stash apply` and `git stash pop`?

`apply` restores the stash but keeps it in the stash list.

`pop` restores the stash and removes it from the stash list.

---

### Which command lists all stashes?

```bash
git stash list
```

---

### How do you stash untracked files?

```bash
git stash -u
```

---

# Practice

1. Modify a file.
2. Run:

```bash
git stash
```

3. Verify:

```bash
git status
```

4. List all stashes:

```bash
git stash list
```

5. Restore your work:

```bash
git stash pop
```

6. Continue development.

---

# Summary

Git Stash is a powerful feature that temporarily saves unfinished work without creating a commit. It allows developers to switch tasks quickly, keep commit history clean, and return to their work whenever they are ready.
