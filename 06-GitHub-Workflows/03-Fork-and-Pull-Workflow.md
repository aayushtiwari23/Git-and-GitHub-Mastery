
# Fork and Pull Workflow

## Introduction

The Fork and Pull Workflow is the standard workflow used in open-source software development. It allows developers to contribute to projects without having direct write access to the original repository.

This workflow is followed by thousands of popular open-source projects such as React, Kubernetes, VS Code, Linux, TensorFlow, and many others.

---

# What is the Fork and Pull Workflow?

Instead of working directly on the original repository, contributors:

- Fork the repository.
- Clone their fork.
- Create a feature branch.
- Make changes.
- Push changes to their fork.
- Open a Pull Request to the original repository.

---

# Why Use This Workflow?

It allows:

- Safe collaboration.
- Open-source contributions.
- Better code reviews.
- Secure repository management.
- Independent development.

---

# Complete Workflow

```text
Original Repository
        │
      Fork
        │
Clone Your Fork
        │
Create Feature Branch
        │
Write Code
        │
git add
        │
git commit
        │
git push
        │
Open Pull Request
        │
Code Review
        │
Merge into Original Repository
```

---

# Step-by-Step Process

## Step 1: Fork the Repository

Open GitHub.

Click:

```text
Fork
```

GitHub creates a copy in your account.

---

## Step 2: Clone Your Fork

```bash
git clone https://github.com/your-username/project.git
```

---

## Step 3: Add the Original Repository

```bash
git remote add upstream https://github.com/original-owner/project.git
```

Verify:

```bash
git remote -v
```

---

## Step 4: Sync Your Fork

```bash
git fetch upstream

git merge upstream/main

git push origin main
```

---

## Step 5: Create a Feature Branch

```bash
git switch -c feature-update-readme
```

---

## Step 6: Make Changes

Modify the required files.

---

## Step 7: Stage Changes

```bash
git add .
```

---

## Step 8: Commit Changes

```bash
git commit -m "Improve README documentation"
```

---

## Step 9: Push the Branch

```bash
git push -u origin feature-update-readme
```

---

## Step 10: Open a Pull Request

Go to your fork on GitHub.

Click:

```text
Compare & Pull Request
```

Choose:

```text
Base Repository:
Original Repository

Compare:
feature-update-readme
```

Submit the Pull Request.

---

## Step 11: Code Review

Project maintainers:

- Review your code.
- Request changes if necessary.
- Approve the Pull Request.

---

## Step 12: Merge

After approval, the maintainer merges your Pull Request into the original repository.

---

# Visual Workflow

```text
Original Repository
        │
      Fork
        │
Your Repository
        │
Create Branch
        │
Develop
        │
Commit
        │
Push
        │
Pull Request
        │
Maintainer Review
        │
Merge
```

---

# Real-World Example

You want to fix a typo in Microsoft's VS Code documentation.

You:

1. Fork the repository.
2. Clone your fork.
3. Create:

```text
docs-fix-typo
```

4. Fix the typo.
5. Commit the changes.
6. Push your branch.
7. Open a Pull Request.
8. A maintainer reviews and merges your contribution.

---

# Advantages

- Safe for open-source projects.
- No direct access to the original repository is required.
- Maintainers control what gets merged.
- Easy to review contributions.
- Supports thousands of contributors.

---

# Best Practices

- Sync your fork before starting work.
- Create a new branch for each contribution.
- Keep Pull Requests small.
- Follow the project's contribution guidelines.
- Write clear commit messages.

---

# Common Mistakes

- Working directly on the `main` branch.
- Forgetting to sync the fork.
- Creating very large Pull Requests.
- Ignoring review comments.
- Opening multiple unrelated changes in one Pull Request.

---

# Interview Questions

### What is the Fork and Pull Workflow?

A workflow where contributors fork a repository, make changes in their own copy, and submit a Pull Request to the original repository.

---

### Why is this workflow popular in open source?

Because contributors do not need direct write access to the original repository.

---

### What is the purpose of the `upstream` remote?

It points to the original repository so your fork can stay updated.

---

### Who merges the Pull Request?

The project maintainer or an authorized reviewer.

---

# Practice

1. Fork any public GitHub repository.
2. Clone your fork.
3. Add the `upstream` remote.
4. Create a feature branch.
5. Make a small documentation change.
6. Commit and push the branch.
7. Open a Pull Request.
8. Sync your fork after the Pull Request is merged.

---

# Summary

The Fork and Pull Workflow is the standard contribution model for open-source software. It allows developers to contribute safely while giving project maintainers full control over the codebase. Learning this workflow is essential for anyone who wants to contribute to major open-source projects.
