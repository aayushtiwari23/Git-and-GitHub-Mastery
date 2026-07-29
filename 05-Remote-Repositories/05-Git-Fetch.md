# Git Fetch

## Introduction

The `git fetch` command downloads the latest commits, branches, and tags from a remote repository without changing your working directory. It lets you see what has changed before deciding whether to merge those changes.

Unlike `git pull`, `git fetch` does not automatically merge updates.

---

# What is `git fetch`?

`git fetch` downloads updates from the remote repository and stores them in your local Git database.

It does **not** modify your current files.

---

# Why Use Git Fetch?

Developers use `git fetch` to:

- Check for new changes
- Update remote tracking branches
- Review commits before merging
- Avoid unwanted automatic merges
- Stay synchronized safely

---

# Basic Command

```bash
git fetch
```

This downloads the latest changes from the default remote.

---

# Fetch from a Specific Remote

```bash
git fetch origin
```

---

# Fetch All Remotes

```bash
git fetch --all
```

---

# Workflow

```text
GitHub Repository
        │
    git fetch
        │
        ▼
Remote Tracking Branch Updated
        │
(No changes to working files)
```

---

# Git Fetch vs Git Pull

| Git Fetch | Git Pull |
|-----------|----------|
| Downloads changes | Downloads and merges changes |
| Safe | May create merge conflicts |
| Does not change working files | Updates working files |
| Used for review | Used to update your branch |

---

# Example

A teammate pushes new commits.

You run:

```bash
git fetch
```

Git downloads the updates.

You review them before merging:

```bash
git log main..origin/main
```

If everything looks good:

```bash
git merge origin/main
```

---

# Real-World Example

A developer wants to inspect new changes before updating their project.

Instead of:

```bash
git pull
```

They use:

```bash
git fetch
```

After reviewing the commits, they merge them manually.

This provides greater control over the update process.

---

# Common Git Fetch Commands

Fetch default remote:

```bash
git fetch
```

Fetch from origin:

```bash
git fetch origin
```

Fetch all remotes:

```bash
git fetch --all
```

---

# Best Practices

- Use `git fetch` before large merges.
- Review incoming commits before merging.
- Fetch regularly in team projects.
- Combine with `git log` to inspect updates.

---

# Common Mistakes

- Assuming `git fetch` updates your files.
- Forgetting to merge after fetching.
- Confusing `git fetch` with `git pull`.
- Ignoring newly fetched branches.

---

# Interview Questions

### What does `git fetch` do?

It downloads the latest changes from a remote repository without merging them.

---

### Does `git fetch` modify your working directory?

No.

---

### What is the main difference between `git fetch` and `git pull`?

`git fetch` only downloads changes.

`git pull` downloads and merges them automatically.

---

### Which command fetches updates from all remotes?

```bash
git fetch --all
```

---

# Practice

1. Ask a teammate to push a new commit.
2. Run:

```bash
git fetch
```

3. View the downloaded changes.
4. Merge them manually if required.

---

# Summary

The `git fetch` command safely downloads updates from remote repositories without changing your working files. It is widely used by professional developers to review incoming changes before merging them into the project.
