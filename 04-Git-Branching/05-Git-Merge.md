
# Git Merge

## Introduction

The `git merge` command is used to combine changes from one branch into another. It is one of the most important Git commands because it allows completed work to become part of the main project.

In professional software development, features are usually developed on separate branches and merged into the `main` branch after review and testing.

---

# What is `git merge`?

`git merge` combines the history and changes of one branch into another.

Example:

```text
main
 │
 ├── feature-login
 │       │
 │   Work Completed
 │       │
 └────── Merge
         │
        main
```

---

# Why Use Merge?

Developers use merge to:

- Add completed features
- Combine team members' work
- Integrate bug fixes
- Keep the project updated

---

# Basic Merge Process

### Step 1

Check your current branch.

```bash
git branch
```

---

### Step 2

Switch to the target branch.

Usually:

```bash
git switch main
```

---

### Step 3

Merge the feature branch.

```bash
git merge feature-login
```

Git combines the changes into `main`.

---

# Example

Current branches:

```text
main

feature-login
```

Merge:

```bash
git switch main

git merge feature-login
```

Result:

```text
main
│
├── Login Feature Added
```

---

# Fast-Forward Merge

If no new commits exist on `main`, Git performs a **Fast-Forward Merge**.

```text
main
  │
  └────► feature-login
```

Git simply moves the `main` pointer forward.

---

# Three-Way Merge

If both branches contain new commits, Git performs a **Three-Way Merge**.

```text
main
   \
    \
 feature-login

      ↓

   Merge Commit
```

A new merge commit is created.

---

# Merge Workflow

```text
Create Branch
      │
Develop Feature
      │
Commit Changes
      │
Switch to main
      │
Merge Feature
      │
Delete Branch
```

---

# Real-World Example

A developer completes the Payment Module.

Commands:

```bash
git switch main

git merge feature-payment
```

The Payment feature is now part of the main project.

---

# Best Practices

- Test your code before merging.
- Pull the latest changes before merging.
- Use meaningful branch names.
- Delete merged branches.

---

# Common Mistakes

- Merging the wrong branch.
- Forgetting to switch to `main`.
- Merging unfinished work.
- Ignoring merge conflicts.

---

# Interview Questions

### What does `git merge` do?

It combines changes from one branch into another.

---

### Which branch should you switch to before merging?

The target branch.

Usually:

```bash
git switch main
```

---

### What is a Fast-Forward Merge?

A merge where Git moves the branch pointer forward without creating a merge commit.

---

### What is a Three-Way Merge?

A merge that creates a new merge commit because both branches contain new changes.

---

# Practice

1. Create a branch.

```bash
git switch -c feature-profile
```

2. Make a small change.

3. Commit the change.

4. Switch back.

```bash
git switch main
```

5. Merge the branch.

```bash
git merge feature-profile
```

6. Verify the merge.

---

# Summary

The `git merge` command combines completed work into another branch. It is an essential part of every professional Git workflow and allows multiple developers to work independently while keeping the main project stable.
