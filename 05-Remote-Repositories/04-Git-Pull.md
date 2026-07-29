
# Git Pull

## Introduction

The `git pull` command downloads the latest changes from a remote repository and automatically merges them into your current branch. It helps keep your local repository up to date with the latest work from GitHub or your teammates.

---

# What is `git pull`?

`git pull` performs two operations:

1. `git fetch`
2. `git merge`

This means Git first downloads the latest changes and then merges them into your current branch.

---

# Why Use Git Pull?

Developers use `git pull` to:

- Get teammates' latest changes
- Update the local repository
- Stay synchronized with GitHub
- Reduce merge conflicts
- Start working with the latest code

---

# Basic Command

```bash
git pull
```

Git downloads and merges the latest changes.

---

# Pull from a Specific Branch

```bash
git pull origin main
```

Meaning:

- `origin` → Remote repository
- `main` → Branch to download from

---

# Pull Workflow

```text
GitHub Repository
        │
    git pull
        │
        ▼
Local Repository Updated
```

---

# Example

Suppose your teammate pushed three new commits.

Before starting your work, run:

```bash
git pull
```

Your local repository now contains those commits.

---

# What Happens Internally?

When you run:

```bash
git pull
```

Git executes:

```bash
git fetch

git merge
```

Automatically.

---

# Real-World Example

Developer A pushes a new Login feature.

Developer B starts work later.

Before writing any code, Developer B runs:

```bash
git pull
```

Now both developers are working on the latest version of the project.

---

# Git Pull vs Git Fetch

| Git Pull | Git Fetch |
|----------|-----------|
| Downloads changes | Downloads changes |
| Automatically merges | Does not merge |
| Updates working directory | Only updates local references |

---

# Common Git Pull Commands

Pull current branch:

```bash
git pull
```

Pull from main:

```bash
git pull origin main
```

Pull another branch:

```bash
git pull origin feature-login
```

---

# Best Practices

- Pull before starting work.
- Commit your work before pulling.
- Resolve conflicts carefully.
- Pull regularly when working in a team.

---

# Common Mistakes

- Pulling with uncommitted changes.
- Ignoring merge conflicts.
- Forgetting to pull before pushing.
- Pulling the wrong branch.

---

# Interview Questions

### What does `git pull` do?

It downloads and merges the latest changes from a remote repository.

---

### Which two commands does `git pull` perform internally?

```bash
git fetch

git merge
```

---

### Which command pulls the latest changes from the main branch?

```bash
git pull origin main
```

---

### Why should developers run `git pull` before starting work?

To ensure they are working with the latest version of the project.

---

# Practice

1. Push a commit from another computer or ask a teammate to push a commit.
2. Open your local repository.
3. Run:

```bash
git pull
```

4. Verify the new changes.
5. Continue your work.

---

# Summary

The `git pull` command keeps your local repository synchronized with the remote repository by downloading and merging the latest changes. It is one of the most commonly used Git commands in collaborative software development.
