
# Syncing Forks

## Introduction

A fork is your personal copy of someone else's GitHub repository. Forking allows you to contribute to open-source projects without having direct write access to the original repository.

To keep your fork updated, you need to sync it with the original repository.

---

# What is a Fork?

A fork is a copy of another repository under your own GitHub account.

Example:

```text
Original Repository
        │
        ▼
Fork (Your GitHub)
        │
        ▼
Local Repository
```

---

# Why Sync a Fork?

Developers sync forks to:

- Get the latest updates
- Reduce merge conflicts
- Stay compatible with the original project
- Contribute using the latest code

---

# Fork Workflow

```text
Original Repository
        │
      Fork
        │
Clone Fork
        │
Make Changes
        │
Commit
        │
Push
        │
Pull Request
```

---

# Step 1: Fork the Repository

On GitHub:

- Open the repository.
- Click **Fork**.
- Wait for GitHub to create your copy.

---

# Step 2: Clone Your Fork

```bash
git clone https://github.com/your-username/project.git
```

---

# Step 3: Check Existing Remotes

```bash
git remote -v
```

Example:

```text
origin  https://github.com/your-username/project.git (fetch)

origin  https://github.com/your-username/project.git (push)
```

---

# Step 4: Add the Original Repository

```bash
git remote add upstream https://github.com/original-owner/project.git
```

Verify:

```bash
git remote -v
```

Example:

```text
origin    https://github.com/your-username/project.git

upstream  https://github.com/original-owner/project.git
```

---

# Step 5: Download Latest Changes

```bash
git fetch upstream
```

---

# Step 6: Merge Updates

```bash
git switch main

git merge upstream/main
```

Your local repository is now updated.

---

# Step 7: Update Your Fork

```bash
git push origin main
```

Now your GitHub fork also contains the latest changes.

---

# Complete Workflow

```text
Original Repository
        │
git fetch upstream
        │
git merge upstream/main
        │
git push origin main
        │
Updated Fork
```

---

# Real-World Example

You want to contribute to the VS Code repository.

1. Fork the repository.
2. Clone your fork.
3. Add Microsoft's repository as `upstream`.
4. Sync regularly.
5. Create a feature branch.
6. Push changes.
7. Open a Pull Request.

---

# Best Practices

- Sync your fork before starting work.
- Never commit directly to `main`.
- Create a new branch for each feature or fix.
- Keep your fork updated regularly.

---

# Common Mistakes

- Forgetting to add `upstream`.
- Making changes directly on `main`.
- Opening Pull Requests from `main`.
- Working on an outdated fork.

---

# Interview Questions

### What is a GitHub Fork?

A personal copy of another repository under your GitHub account.

---

### Why is `upstream` added?

To connect your fork to the original repository.

---

### Which command downloads updates from the original repository?

```bash
git fetch upstream
```

---

### Which command updates your GitHub fork?

```bash
git push origin main
```

---

# Practice

1. Fork any public GitHub repository.
2. Clone your fork.
3. Add the original repository as `upstream`.
4. Fetch updates.
5. Merge them into `main`.
6. Push the updated branch to your fork.

---

# Summary

Syncing forks is an essential skill for open-source development. It keeps your fork up to date with the original project, reduces conflicts, and ensures you're contributing on the latest codebase.
