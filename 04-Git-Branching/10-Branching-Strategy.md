# Branching Strategy

## Introduction

A Branching Strategy is a set of rules that defines how developers create, manage, and merge branches in a Git repository. It helps teams work together efficiently while keeping the codebase stable and organized.

Professional software companies use branching strategies to reduce conflicts, improve collaboration, and manage releases effectively.

---

# What is a Branching Strategy?

A Branching Strategy defines:

- When to create branches
- How to name branches
- When to merge branches
- Who reviews changes
- How releases are managed

---

# Why is it Important?

A good branching strategy helps teams:

- Work simultaneously
- Reduce merge conflicts
- Improve code quality
- Release software safely
- Track development easily

---

# Popular Branching Strategies

## 1. GitHub Flow

Best for:

- Small teams
- Continuous deployment
- Open-source projects

Workflow:

```text
main
 │
 ├── feature-login
 │
 ├── feature-payment
 │
 └── Pull Request
       │
     Review
       │
     Merge
```

---

## 2. Git Flow

Best for:

- Large projects
- Enterprise applications
- Scheduled releases

Main Branches:

```text
main

develop

feature/*

release/*

hotfix/*
```

Workflow:

```text
main
 │
develop
 │
├── feature-login
├── feature-payment
└── feature-profile
```

---

## 3. Trunk-Based Development

Best for:

- Fast-moving teams
- Continuous Integration (CI)
- Continuous Deployment (CD)

Workflow:

```text
main
 │
Small Feature Branch
 │
Merge Quickly
 │
Repeat
```

Branches are short-lived and merged frequently.

---

# Branch Naming Convention

Examples:

```text
feature-login

feature-dashboard

feature-payment

bugfix-navbar

hotfix-security

docs-readme

release-v1.0
```

Avoid:

```text
branch1

new

test

demo

abc
```

---

# Comparison

| Strategy | Best For | Complexity |
|----------|----------|------------|
| GitHub Flow | Small projects | Easy |
| Git Flow | Large enterprise projects | High |
| Trunk-Based Development | Continuous deployment | Medium |

---

# Real-World Example

A software company follows GitHub Flow.

1. Create a feature branch.
2. Develop the feature.
3. Commit changes.
4. Push the branch.
5. Open a Pull Request.
6. Review the code.
7. Merge into `main`.
8. Deploy the application.

---

# Best Practices

- Keep branches short-lived.
- Use meaningful branch names.
- Merge frequently.
- Delete merged branches.
- Protect the `main` branch.

---

# Common Mistakes

- Long-running branches.
- Working directly on `main`.
- Large Pull Requests.
- Poor branch naming.
- Delaying merges.

---

# Interview Questions

### What is a Branching Strategy?

A Branching Strategy is a workflow that defines how branches are created, managed, and merged.

---

### Which branching strategy is most common on GitHub?

GitHub Flow.

---

### Which strategy is suitable for enterprise applications?

Git Flow.

---

### Why are branching strategies important?

They improve collaboration, reduce merge conflicts, and help maintain a stable codebase.

---

# Practice

1. Create a repository.
2. Create a branch named:

```text
feature-profile
```

3. Make a small change.
4. Commit the change.
5. Open a Pull Request.
6. Merge it into `main`.
7. Delete the branch.

---

# Summary

A Branching Strategy provides a structured workflow for software development. Whether using GitHub Flow, Git Flow, or Trunk-Based Development, following a consistent branching strategy improves collaboration, code quality, and project management.
