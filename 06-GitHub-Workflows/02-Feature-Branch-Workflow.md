# Feature Branch Workflow

## Introduction

The Feature Branch Workflow is one of the most popular Git workflows used by software companies. Instead of making changes directly on the `main` branch, every new feature, bug fix, or improvement is developed in its own branch.

This approach keeps the `main` branch stable and makes collaboration much easier.

---

# What is a Feature Branch Workflow?

A Feature Branch Workflow is a development strategy where every task is completed in a separate branch.

Each branch contains only one feature or one bug fix.

Example:

```text
main
 │
 ├── feature-login
 │
 ├── feature-payment
 │
 ├── feature-profile
 │
 └── bugfix-navbar
```

After development is complete, the branch is merged into `main`.

---

# Why Use Feature Branches?

Developers use feature branches to:

- Keep the `main` branch stable.
- Develop multiple features simultaneously.
- Make code reviews easier.
- Reduce merge conflicts.
- Test features independently.

---

# Complete Workflow

```text
main
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
Merge
 │
Delete Branch
```

---

# Step-by-Step Process

## Step 1: Update Your Local Repository

```bash
git pull origin main
```

---

## Step 2: Create a New Feature Branch

```bash
git switch -c feature-login
```

---

## Step 3: Write Your Code

Modify the required files.

---

## Step 4: Check the Status

```bash
git status
```

---

## Step 5: Stage Changes

```bash
git add .
```

---

## Step 6: Commit the Changes

```bash
git commit -m "Add login functionality"
```

---

## Step 7: Push the Branch

```bash
git push -u origin feature-login
```

---

## Step 8: Open a Pull Request

Go to GitHub.

Click:

```text
Compare & Pull Request
```

Describe your changes clearly.

---

## Step 9: Code Review

Team members:

- Review the code.
- Suggest improvements.
- Approve the Pull Request.

---

## Step 10: Merge the Pull Request

Click:

```text
Merge Pull Request
```

GitHub merges your branch into `main`.

---

## Step 11: Delete the Branch

Delete locally:

```bash
git branch -d feature-login
```

Delete remotely:

```bash
git push origin --delete feature-login
```

---

# Visual Workflow

```text
main
 │
 ├───────────────┐
 │               │
 │        feature-login
 │               │
 │        Add Commits
 │               │
 │       Pull Request
 │               │
 └──────Merge────┘
         │
       main
```

---

# Naming Convention

Good branch names:

```text
feature-login

feature-dashboard

feature-payment

feature-user-profile

bugfix-navbar

bugfix-login

hotfix-security

docs-readme
```

Avoid:

```text
test

new

branch1

abc

demo
```

---

# Real-World Example

A company wants to add Dark Mode.

Developer creates:

```text
feature-dark-mode
```

They:

- Write the code.
- Test the feature.
- Push the branch.
- Open a Pull Request.
- Receive code review.
- Merge into `main`.
- Delete the branch.

Meanwhile, another developer can work on:

```text
feature-payment
```

without affecting the Dark Mode feature.

---

# Advantages

- Stable `main` branch.
- Easier collaboration.
- Better code reviews.
- Easier testing.
- Simple rollback if needed.

---

# Best Practices

- One feature per branch.
- Pull the latest changes before creating a branch.
- Keep branches short-lived.
- Use descriptive branch names.
- Delete merged branches.

---

# Common Mistakes

- Working directly on `main`.
- Mixing multiple features in one branch.
- Forgetting to pull before creating a branch.
- Keeping branches open for too long.
- Using unclear branch names.

---

# Interview Questions

### What is a Feature Branch Workflow?

A workflow where each feature or bug fix is developed in a separate branch before being merged into the `main` branch.

---

### Why shouldn't developers work directly on `main`?

Because it can introduce bugs into the stable version of the project.

---

### Which command creates a new feature branch?

```bash
git switch -c feature-login
```

---

### Why should feature branches be deleted after merging?

To keep the repository clean and organized.

---

# Practice

1. Create a repository.
2. Create a branch named:

```text
feature-profile
```

3. Add a new file.
4. Commit the changes.
5. Push the branch.
6. Open a Pull Request.
7. Merge it.
8. Delete the branch.

---

# Summary

The Feature Branch Workflow is the industry-standard development workflow used by companies of all sizes. It allows developers to build features independently, review code safely, and maintain a stable `main` branch throughout the software development lifecycle.
