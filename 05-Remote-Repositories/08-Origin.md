# Origin

## Introduction

`origin` is the default name Git gives to the remote repository when you clone a project. It acts as a shortcut for the repository URL, allowing you to use simple commands like `git push` and `git pull` instead of typing the full repository address.

---

# What is Origin?

`origin` is simply the name (alias) of a remote repository.

Example:

```text
origin
      │
      ▼
https://github.com/username/project.git
```

It is **not** a Git command or a special server. It is just a convenient name.

---

# Why is Origin Used?

Using `origin` makes Git commands shorter and easier.

Instead of writing:

```text
https://github.com/username/project.git
```

every time, you simply write:

```bash
git push origin main
```

---

# How is Origin Created?

When you clone a repository:

```bash
git clone https://github.com/username/project.git
```

Git automatically creates:

```text
origin
```

If you create a local repository manually, you add it yourself:

```bash
git remote add origin https://github.com/username/project.git
```

---

# View Origin

List all remotes:

```bash
git remote
```

Example:

```text
origin
```

---

# View Origin URL

```bash
git remote -v
```

Example:

```text
origin  https://github.com/username/project.git (fetch)

origin  https://github.com/username/project.git (push)
```

---

# Change Origin URL

```bash
git remote set-url origin https://github.com/new-user/project.git
```

Verify it:

```bash
git remote -v
```

---

# Remove Origin

```bash
git remote remove origin
```

---

# Workflow

```text
Create Repository
        │
Add Origin
        │
git push
        │
GitHub Updated
```

---

# Real-World Example

A developer creates a repository on GitHub.

They connect it to the local project:

```bash
git remote add origin https://github.com/company/ecommerce.git
```

Now they can use:

```bash
git push

git pull

git fetch
```

without typing the full repository URL.

---

# Origin vs Upstream

| Origin | Upstream |
|---------|----------|
| Name of a remote repository | Branch tracked by a local branch |
| Usually your own GitHub repository | Usually `origin/main` or another tracked branch |
| Created automatically after cloning | Configured using `git push -u` |

---

# Best Practices

- Keep only valid remote URLs.
- Use `origin` as the primary remote.
- Verify remotes with `git remote -v`.
- Update the URL if the repository moves.

---

# Common Mistakes

- Thinking `origin` is a Git command.
- Assuming every repository must have only one remote.
- Using the wrong repository URL.
- Confusing `origin` with `upstream`.

---

# Interview Questions

### What is `origin`?

`origin` is the default alias for a remote Git repository.

---

### Which command displays the origin URL?

```bash
git remote -v
```

---

### Which command adds an origin remote?

```bash
git remote add origin <repository-url>
```

---

### Is `origin` mandatory?

No.

It is simply the default name. You can choose another name if needed.

---

# Practice

1. Create a local repository.
2. Create a GitHub repository.
3. Connect them:

```bash
git remote add origin <repository-url>
```

4. Verify:

```bash
git remote -v
```

5. Push your first commit.

---

# Summary

`origin` is the default name for a remote Git repository. It simplifies Git commands by acting as a shortcut to the repository URL and is a fundamental concept in everyday Git and GitHub workflows.
