# Fork

## Introduction

A Fork is a personal copy of someone else's GitHub repository. It allows you to experiment, make changes, and contribute to projects without affecting the original repository.

Forking is one of the most important concepts in open-source development.

---

# What is a Fork?

A Fork creates a copy of a repository under your own GitHub account.

Original Repository

↓

Fork

↓

Your GitHub Account

The fork is completely independent from the original repository until you create a Pull Request.

---

# Why Use Fork?

Developers use Forks to:

- Contribute to open-source projects
- Experiment safely
- Learn from existing projects
- Build improvements
- Fix bugs without affecting the original project

---

# How to Fork a Repository

### Step 1

Open the repository you want to contribute to.

### Step 2

Click the **Fork** button in the top-right corner.

### Step 3

Choose your GitHub account.

### Step 4

GitHub creates a copy of the repository under your account.

---

# Workflow

```text
Original Repository
        │
      Fork
        │
Your GitHub Repository
        │
      Clone
        │
Make Changes
        │
Commit
        │
Push
        │
Pull Request
        │
Original Repository
```

---

# Example

Original Repository:

```text
github.com/octocat/Hello-World
```

After Fork:

```text
github.com/your-username/Hello-World
```

Now you own the copied repository.

---

# Fork vs Clone

| Fork | Clone |
|------|------|
| Creates a copy on GitHub | Creates a copy on your computer |
| Used for contributing | Used for development |
| Exists online | Exists locally |

---

# Real-World Example

Suppose Microsoft has an open-source project.

You want to improve its documentation.

You:

1. Fork the repository.
2. Clone your fork.
3. Make changes.
4. Commit.
5. Push.
6. Create a Pull Request.

If the maintainers approve your changes, they merge them into the original repository.

---

# Best Practices

- Keep your fork updated.
- Create a new branch for each feature.
- Make small, focused changes.
- Write clear commit messages.
- Read the project's contribution guidelines.

---

# Common Mistakes

- Editing the original repository directly.
- Making unrelated changes.
- Not syncing your fork with the original repository.
- Creating very large Pull Requests.

---

# Interview Questions

### What is a Fork?

A Fork is your own copy of another GitHub repository.

---

### Why do developers use Fork?

To contribute to projects without affecting the original repository.

---

### Does Fork copy commit history?

Yes.

The complete repository history is copied.

---

### Is Fork the same as Clone?

No.

Fork creates an online copy.

Clone creates a local copy.

---

# Practice

1. Fork any public repository.
2. Open your fork.
3. Compare it with the original repository.
4. Notice that both have the same files and commit history.

---

# Summary

Forking is the standard way to contribute to open-source projects on GitHub. It allows developers to work independently, experiment safely, and submit improvements through Pull Requests without modifying the original repository directly.
