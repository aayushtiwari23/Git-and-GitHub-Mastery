# Upstream Branch

## Introduction

An upstream branch is the remote branch that your current local branch is connected to. Once an upstream branch is configured, Git automatically knows where to push your commits and where to pull updates from.

This makes your daily Git workflow much simpler.

---

# What is an Upstream Branch?

An upstream branch is the default remote branch associated with your current local branch.

Example:

```text
Local Branch
main
    │
    │
Upstream
    │
    ▼
origin/main
```

Whenever you run:

```bash
git push
```

Git automatically pushes to:

```text
origin/main
```

Likewise,

```bash
git pull
```

automatically pulls from:

```text
origin/main
```

---

# Why Use an Upstream Branch?

An upstream branch allows Git to:

- Remember where to push changes.
- Remember where to pull updates.
- Reduce typing.
- Simplify daily development.

---

# Set an Upstream Branch

The first time you push a new branch:

```bash
git push -u origin main
```

or

```bash
git push --set-upstream origin main
```

After this, you only need:

```bash
git push
```

and

```bash
git pull
```

---

# Check the Upstream Branch

```bash
git branch -vv
```

Example:

```text
* main 8ab72f3 [origin/main] Initial commit
```

The text inside the brackets shows the upstream branch.

---

# Change the Upstream Branch

```bash
git branch --set-upstream-to=origin/main
```

Example:

```bash
git branch --set-upstream-to=origin/main feature-login
```

---

# Remove the Upstream Branch

```bash
git branch --unset-upstream
```

---

# Workflow

```text
Create Branch
       │
git push -u origin feature-login
       │
Upstream Created
       │
Future Commands
git push
git pull
```

---

# Real-World Example

A developer creates:

```text
feature-payment
```

First push:

```bash
git push -u origin feature-payment
```

From now on:

```bash
git push

git pull
```

Git automatically knows the destination branch.

---

# Upstream vs Origin

| Upstream | Origin |
|----------|--------|
| Default tracking branch | Name of the remote repository |
| Used for push and pull | Points to GitHub repository |

Example:

```text
origin
      │
      ▼
origin/main
      ▲
      │
Upstream of local main
```

---

# Best Practices

- Set the upstream during the first push.
- Verify it using `git branch -vv`.
- Keep local and remote branches synchronized.
- Use meaningful branch names.

---

# Common Mistakes

- Forgetting to set the upstream.
- Pushing to the wrong branch.
- Confusing `origin` with `upstream`.
- Deleting a tracked remote branch accidentally.

---

# Interview Questions

### What is an upstream branch?

An upstream branch is the default remote branch that a local branch tracks.

---

### Which command sets an upstream branch?

```bash
git push -u origin main
```

---

### Which command displays upstream information?

```bash
git branch -vv
```

---

### Why are upstream branches useful?

They allow developers to use simple commands like:

```bash
git push

git pull
```

without specifying the remote and branch every time.

---

# Practice

1. Create a new branch.

```bash
git switch -c feature-profile
```

2. Push it for the first time.

```bash
git push -u origin feature-profile
```

3. Verify the upstream.

```bash
git branch -vv
```

4. Make another commit.

5. Push again using only:

```bash
git push
```

---

# Summary

An upstream branch links a local branch to its corresponding remote branch. Once configured, Git automatically knows where to push and pull changes, making day-to-day development faster and more efficient.
