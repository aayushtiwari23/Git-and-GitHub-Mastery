# Git Push

## Introduction

The `git push` command uploads your local commits to a remote repository such as GitHub. It is one of the most frequently used Git commands because it allows you to share your work, back up your code, and collaborate with other developers.

---

# What is `git push`?

`git push` sends your local commits to a remote repository.

Example:

```text
Local Repository
       │
 git push
       ▼
GitHub Repository
```

Only committed changes are pushed.

---

# Why Use Git Push?

Developers use `git push` to:

- Upload code to GitHub
- Share work with teammates
- Back up projects
- Update remote branches
- Deploy code through CI/CD pipelines

---

# Basic Command

```bash
git push
```

If the branch is already linked to the remote branch, this uploads all new commits.

---

# First Push

When pushing a branch for the first time:

```bash
git push -u origin main
```

Explanation:

- `-u` sets the upstream branch.
- `origin` is the remote repository.
- `main` is the branch name.

After this, future pushes only require:

```bash
git push
```

---

# Push Another Branch

```bash
git push origin feature-login
```

This uploads only the `feature-login` branch.

---

# Push All Branches

```bash
git push --all origin
```

---

# Force Push

```bash
git push --force
```

or

```bash
git push -f
```

⚠️ Use this carefully because it can overwrite remote history.

---

# Check Push Status

After pushing:

```bash
git log

git status
```

Or verify your commits on GitHub.

---

# Workflow

```text
Edit Files
      │
git add
      │
git commit
      │
git push
      │
GitHub Updated
```

---

# Real-World Example

A developer completes the Login Module.

Commands:

```bash
git add .

git commit -m "Add login feature"

git push
```

The commits are now available on GitHub for teammates to review.

---

# Common Git Push Commands

Push current branch:

```bash
git push
```

First push:

```bash
git push -u origin main
```

Push another branch:

```bash
git push origin feature-payment
```

Push all branches:

```bash
git push --all origin
```

---

# Best Practices

- Commit before pushing.
- Pull the latest changes before pushing.
- Write meaningful commit messages.
- Avoid force pushing on shared branches.

---

# Common Mistakes

- Forgetting to commit before pushing.
- Pushing to the wrong branch.
- Using `--force` unnecessarily.
- Ignoring push errors.

---

# Interview Questions

### What does `git push` do?

It uploads local commits to a remote repository.

---

### Which command is used for the first push?

```bash
git push -u origin main
```

---

### What does the `-u` option do?

It sets the upstream branch so future pushes can be done with `git push`.

---

### Why should `git push --force` be used carefully?

Because it can overwrite the remote branch history and affect other developers.

---

# Practice

1. Create a repository on GitHub.
2. Make a change locally.
3. Stage the change.

```bash
git add .
```

4. Commit the change.

```bash
git commit -m "Initial commit"
```

5. Push it.

```bash
git push -u origin main
```

6. Refresh your GitHub repository and verify the commit.

---

# Summary

The `git push` command uploads your local commits to a remote repository. It is one of the most important Git commands for collaboration, backup, and sharing code with others through platforms like GitHub.
