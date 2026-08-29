

Merge
```

---

# Bug-Fix Branch

Use a separate branch for fixing a bug.

Example:

```bash
git switch -c fix/login-validation
```

Example names:

```text
fix/login
fix/navbar
fix/api-error
fix/database-connection
```

---

# Documentation Branch

Documentation changes can also use separate branches.

Examples:

```text
docs/readme-update
docs/installation-guide
docs/api-documentation
```

---

# Branch Naming

Good branch names are:

```text
Clear
Short
Descriptive
Consistent
```

Good:

```text
feature/user-login
fix/payment-error
docs/update-readme
test/api-tests
```

Bad:

```text
new
test
changes
branch1
mybranch
```

---

# Feature Branch Workflow

Typical workflow:

```text
main
 ↓
Create Branch
 ↓
feature/name
 ↓
Make Changes
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
Review
 ↓
Merge
```

---

# Short-Lived Branches

Keep feature branches reasonably short-lived.

Instead of:

```text
feature/login
 ↓
3 months of changes
 ↓
Huge PR
```

prefer:

```text
Small Change
 ↓
PR
 ↓
Review
 ↓
Merge
```

Shorter branches generally reduce merge conflicts and make review easier.

---

# Long-Lived Branches

Some projects maintain long-lived branches for specific purposes.

Examples:

```text
main
develop
release
```

However, not every project needs this complexity.

Use the strategy required by the project.

---

# Git Flow

Git Flow is a structured branching model.

A simplified version contains:

```text
main
develop
feature/*
release/*
hotfix/*
```

Conceptually:

```text
                 main
                  ↑
               release
                  ↑
               develop
               ↑     ↑
          feature   feature
```

---

# Feature Branches in Git Flow

New features are created from `develop`.

Example:

```text
develop
   ↓
feature/login
```

After development:

```text
feature/login
      ↓
   develop
```

---

# Release Branch

A release branch can be used to prepare a version.

Example:

```text
release/1.0
```

Typical flow:

```text
develop
   ↓
release/1.0
   ↓
Testing
   ↓
Bug Fixes
   ↓
Release
```

---

# Hotfix Branch

A hotfix is used for an urgent production fix.

Example:

```text
hotfix/security-patch
```

Conceptually:

```text
main
 ↓
hotfix
 ↓
Fix
 ↓
main
```

The fix may also need to be incorporated into the development branch depending on the project's workflow.

---

# Trunk-Based Development

Trunk-based development emphasizes keeping branches short-lived and integrating changes frequently into the main branch.

Conceptually:

```text
        small branch
             ↓
main ────────────────
        ↑
      merge
```

Developers avoid maintaining many long-lived branches.

---

# GitHub Flow

GitHub Flow is a simpler workflow.

Basic process:

```text
main
 ↓
Create Branch
 ↓
Make Changes
 ↓
Pull Request
 ↓
Review
 ↓
Merge
```

This is easy to understand and works well for many projects.

---

# GitHub Flow vs Git Flow

### GitHub Flow

Usually simpler:

```text
main
 ↓
feature branch
 ↓
Pull Request
 ↓
main
```

### Git Flow

More structured:

```text
main
 ↓
develop
 ↓
feature
 ↓
release
 ↓
main
```

Neither is automatically better.

The project determines what is appropriate.

---

# Branch Protection

Important branches can be protected.

For example:

```text
main
```

may require:

```text
Pull Request
Required Review
Passing CI
```

before merging.

Conceptually:

```text
Developer
    ↓
Feature Branch
    ↓
Pull Request
    ↓
Review + CI
    ↓
Protected main
```

---

# Why Protect Main?

Protection helps prevent:

```text
Broken Builds
Unreviewed Code
Accidental Changes
Failed Tests
```

from reaching the main branch.

---

# Pull Requests and Branches

A Pull Request normally compares two branches.

Example:

```text
base:
main

compare:
feature/login
```

Meaning:

```text
feature/login
       ↓
      main
```

---

# Branch Status

A branch may be:

```text
Up to Date
Behind
Ahead
Diverged
```

Example:

```text
main:
A ─ B ─ C

feature:
A ─ B ─ D
```

The branches have diverged because each contains a different commit after `B`.

---

# Updating a Branch

If your branch is behind main:

```bash
git fetch origin
```

Then you can update it using merge:

```bash
git merge origin/main
```

or rebase:

```bash
git rebase origin/main
```

Choose according to the project's workflow.

---

# Merge-Based Workflow

Example:

```text
main:
A ─ B ─ C

feature:
A ─ B ─ D
```

Merge:

```text
A ─ B ─ C ─ M
     \       /
      D ────
```

`M` is the merge commit.

---

# Rebase-Based Workflow

Before:

```text
main:
A ─ B ─ C

feature:
A ─ B ─ D
```

After rebasing:

```text
main:
A ─ B ─ C

feature:
A ─ B ─ C ─ D'
```

The feature commit is replayed on top of the latest main.

---

# Choosing Branch Types

A simple naming system:

```text
feature/
fix/
docs/
test/
refactor/
chore/
```

Examples:

```text
feature/search
fix/login-error
docs/setup-guide
test/api-tests
refactor/database-layer
chore/update-dependencies
```

The exact convention should match the project.

---

# Refactoring Branch

A refactoring branch changes code structure without necessarily changing external behavior.

Example:

```text
refactor/cleanup-authentication
```

Typical purpose:

```text
Improve Structure
Remove Duplication
Improve Maintainability
```

---

# Test Branch

A test-related branch might be:

```text
test/add-login-tests
```

It can contain:

```text
Unit Tests
Integration Tests
Edge Cases
Regression Tests
```

---

# Chore Branch

A chore branch is commonly used for maintenance work.

Examples:

```text
chore/update-dependencies
chore/configuration
chore/cleanup
```

---

# Branch Strategy Example

Suppose your repository has:

```text
main
```

You want to add authentication.

Create:

```bash
git switch -c feature/authentication
```

After development:

```text
feature/authentication
        ↓
      Pull Request
        ↓
       Review
        ↓
        main
```

Then you want to fix a bug:

```bash
git switch main
git pull
git switch -c fix/authentication-error
```

After fixing:

```text
fix/authentication-error
        ↓
      Pull Request
        ↓
        main
```

---

# Multiple Developers

Imagine three developers:

```text
Developer A
feature/login

Developer B
feature/dashboard

Developer C
fix/api-error
```

All work independently:

```text
                 main
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     feature   feature     fix
      login   dashboard   api-error
```

Each branch can have its own Pull Request.

---

# Parallel Development

Branches allow:

```text
Developer A → Login
Developer B → Dashboard
Developer C → API
```

to work simultaneously.

Then:

```text
PR A ─┐
PR B ─┼──→ main
PR C ─┘
```

---

# Merge Conflicts

Conflicts become more likely when multiple branches modify the same code.

Example:

```text
Developer A
    ↓
auth.py

Developer B
    ↓
auth.py
```

Both modify the same lines.

Git may require manual conflict resolution.

---

# Reducing Conflicts

Good practices:

```text
Keep branches updated
Make small changes
Communicate with teammates
Avoid unnecessary file changes
Merge frequently
```

---

# Branch Deletion

After a Pull Request is merged, the remote branch may no longer be needed.

Delete local branch:

```bash
git branch -d feature/login
```

Delete remote branch:

```bash
git push origin --delete feature/login
```

GitHub can also provide an option to delete the branch after merging a Pull Request.

---

# Do Not Delete Main

Never delete important branches such as:

```text
main
```

unless you explicitly know the repository's workflow requires it.

---

# Branch Strategy for Your Learning Repository

For your GitHub Mastery repository, keep it simple:

```text
main
 ↓
feature/*
docs/*
fix/*
```

Example:

```text
main

docs/github-actions
docs/pull-requests
docs/open-source

feature/project-dashboard

fix/documentation-error
```

You do not need Git Flow for a small personal learning repository.

---

# Recommended Personal Workflow

Use:

```text
main
 ↓
Create focused branch
 ↓
Make change
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
Review
 ↓
Merge
 ↓
Delete branch
```

This gives you practical experience with the workflow used in collaborative development.

---

# Practice

Create a branch:

```bash
git switch main
git pull
git switch -c docs/branching-strategies
```

Add this document.

Commit:

```bash
git add .
git commit -m "Add branching strategies guide"
```

Push:

```bash
git push -u origin docs/branching-strategies
```

Create a Pull Request:

```text
Title:
Add branching strategies guide
```

After review, merge it into `main`.

---

# Challenge

Create three practice branches:

```text
docs/test-branch
feature/test-feature
fix/test-fix
```

You do not need to make real changes in all three.

The goal is to understand:

```text
How branches are created
How branches are named
How branches are pushed
How Pull Requests connect branches
How branches are merged
How branches are deleted
```

---

# Interview Questions

### What is a branching strategy?

A defined approach for creating, naming, managing, and merging Git branches.

### What is a feature branch?

A branch created to develop a specific feature independently from the main branch.

### What is GitHub Flow?

A simple workflow generally based on creating a branch, opening a Pull Request, reviewing it, and merging it into main.

### What is Git Flow?

A more structured branching model involving branches such as main, develop, feature, release, and hotfix.

### What is trunk-based development?

A development approach where developers integrate changes into the main branch frequently and generally keep branches short-lived.

### Why use branch protection?

To prevent unsafe or unreviewed changes from being merged into important branches.

### Why keep feature branches short-lived?

To reduce divergence, merge conflicts, and the size of Pull Requests.

---

# Summary

The basic branching model is:

```text
main
 ↓
feature/fix/docs branch
 ↓
Development
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
CI
 ↓
Review
 ↓
Merge
 ↓
Delete Branch
```

Remember:

```text
main
   = stable project branch

feature/*
   = new functionality

fix/*
   = bug fixes

docs/*
   = documentation

test/*
   = testing work

refactor/*
   = code restructuring

chore/*
   = maintenance
```

For most personal projects and many GitHub repositories, a simple branch + Pull Request workflow is enough.
