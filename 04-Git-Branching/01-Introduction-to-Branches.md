
# Introduction to Git Branches

## Introduction

Branches are one of Git's most powerful features. They allow developers to work on new features, fix bugs, or experiment with ideas without affecting the main project.

Instead of modifying the main code directly, developers create separate branches, complete their work, and later merge the changes into the main branch.

Branching is a standard practice in modern software development.

---

# What is a Branch?

A branch is an independent line of development within a Git repository.

Each branch has its own commits and history until it is merged.

The default branch is usually named:

```text
main
```

---

# Why Do We Use Branches?

Branches allow developers to:

- Develop new features safely
- Fix bugs independently
- Experiment without risk
- Work in teams simultaneously
- Keep the main branch stable

---

# Branch Workflow

```text
             main
               │
        Create Branch
               │
               ▼
      feature-login
               │
      Make Changes
               │
          Commit Changes
               │
         Merge into main
```

---

# Real-World Example

A team is developing an E-Commerce website.

Different developers work on different branches:

```text
main

feature-login

feature-payment

feature-search

bugfix-checkout
```

Each developer works independently without interfering with others.

---

# Advantages

- Safe development
- Better collaboration
- Easy bug fixing
- Cleaner project history
- Parallel development

---

# Best Practices

- Keep the `main` branch stable.
- Create one branch for one feature.
- Use meaningful branch names.
- Delete merged branches when no longer needed.

---

# Common Branch Names

```text
feature-login

feature-dashboard

bugfix-payment

hotfix-security

docs-readme

release-v1.0
```

---

# Common Mistakes

- Working directly on `main`.
- Mixing multiple features in one branch.
- Forgetting to merge completed work.
- Creating unclear branch names.

---

# Interview Questions

### What is a Git Branch?

A Git Branch is an independent line of development that allows developers to work without affecting the main project.

---

### Why are branches important?

They enable parallel development, safer collaboration, and better project management.

---

### What is the default branch called?

Usually:

```text
main
```

---

# Practice

1. Open any Git repository.
2. View the current branch.
3. Create a new branch.
4. Switch to it.
5. Make a small change.
6. Commit the change.

---

# Summary

Git branches allow developers to work on features, bug fixes, and experiments independently. They improve collaboration, protect the main project, and are an essential part of every professional Git workflow.
