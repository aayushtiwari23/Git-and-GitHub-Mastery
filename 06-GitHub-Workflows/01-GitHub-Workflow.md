# GitHub Workflow

## Introduction

A GitHub Workflow is a structured process that developers follow while building software using Git and GitHub. It ensures that code is developed safely, reviewed properly, and merged without affecting the stability of the project.

Almost every software company follows a GitHub workflow, although the exact steps may vary.

---

# What is a GitHub Workflow?

A GitHub Workflow is the complete lifecycle of a code change, from writing code to merging it into the main branch.

It usually includes:

- Creating a branch
- Writing code
- Committing changes
- Pushing to GitHub
- Opening a Pull Request
- Code Review
- Merging
- Deployment

---

# Why Use a Workflow?

A proper workflow helps developers:

- Collaborate efficiently
- Prevent bugs
- Review code before merging
- Track project history
- Keep the main branch stable

---

# Standard GitHub Workflow

```text
Clone Repository
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
Approve Changes
        │
Merge into main
        │
Delete Branch
```

---

# Step-by-Step Workflow

### Step 1

Clone the repository.

```bash
git clone <repository-url>
```

---

### Step 2

Create a feature branch.

```bash
git switch -c feature-login
```

---

### Step 3

Write or modify code.

---

### Step 4

Stage the changes.

```bash
git add .
```

---

### Step 5

Commit the changes.

```bash
git commit -m "Add login feature"
```

---

### Step 6

Push the branch.

```bash
git push -u origin feature-login
```

---

### Step 7

Open a Pull Request on GitHub.

---

### Step 8

Receive code review and make improvements if needed.

---

### Step 9

Merge the Pull Request.

---

### Step 10

Delete the feature branch.

---

# Real-World Example

A developer is assigned to build a user profile page.

They:

1. Create `feature-profile`.
2. Write the code.
3. Commit the changes.
4. Push the branch.
5. Open a Pull Request.
6. Team members review the code.
7. After approval, the Pull Request is merged into `main`.
8. The branch is deleted.

---

# Benefits

- Clean project history
- Better teamwork
- Easier debugging
- Safe code integration
- Organized development

---

# Best Practices

- Pull before starting work.
- Create one branch per feature.
- Write meaningful commit messages.
- Open small Pull Requests.
- Delete merged branches.

---

# Common Mistakes

- Working directly on `main`.
- Pushing incomplete code.
- Ignoring code review comments.
- Creating very large Pull Requests.
- Forgetting to pull before starting work.

---

# Interview Questions

### What is a GitHub Workflow?

A GitHub Workflow is the process of developing, reviewing, and merging code using Git and GitHub.

---

### Why is a feature branch used?

To isolate development from the stable `main` branch.

---

### Why is code review important?

It improves code quality, catches bugs, and ensures coding standards are followed.

---

### What is the final step of a GitHub Workflow?

Merge the Pull Request and delete the feature branch.

---

# Practice

1. Clone a repository.
2. Create a feature branch.
3. Make a small change.
4. Commit it.
5. Push the branch.
6. Open a Pull Request.
7. Merge it.
8. Delete the branch.

---

# Summary

A GitHub Workflow is the standard process used by software teams to develop, review, and merge code safely. Following a consistent workflow improves collaboration, code quality, and project stability.
