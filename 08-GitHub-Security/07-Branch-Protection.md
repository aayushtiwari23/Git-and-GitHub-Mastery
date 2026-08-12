
# GitHub Branch Protection

## Introduction

Branch protection helps prevent accidental or unauthorized changes to important branches.

The most important branch in many repositories is:

```text
main
```

Protecting it can require Pull Requests, reviews, successful tests, and other checks before changes are merged.

---

# Why Protect a Branch?

Without protection, someone could directly push:

```text
Bad Code
   ↓
git push
   ↓
main
```

With protection:

```text
Feature Branch
      ↓
Pull Request
      ↓
Code Review
      ↓
Automated Tests
      ↓
Approval
      ↓
main
```

This makes the repository safer.

---

# Common Branch Protection Rules

Branch protection can be configured to require:

- Pull Requests.
- Code reviews.
- Status checks.
- Conversation resolution.
- Up-to-date branches.
- Restrictions on force pushes.
- Restrictions on branch deletion.

The exact options available depend on the GitHub plan and repository configuration.

---

# Protecting `main`

A typical workflow is:

```text
Developer
    │
    ▼
Feature Branch
    │
    ▼
Pull Request
    │
    ▼
Automated Checks
    │
    ▼
Code Review
    │
    ▼
Approval
    │
    ▼
Merge into main
```

Developers should normally avoid directly pushing unfinished work to `main`.

---

# Pull Request Requirement

A branch rule can require changes to go through a Pull Request.

Instead of:

```bash
git push origin main
```

developers work on:

```text
feature-login
```

and create:

```text
Pull Request
```

The changes can then be reviewed before merging.

---

# Required Reviews

A repository can require one or more approving reviews before a Pull Request can be merged.

Example:

```text
Pull Request
      ↓
Reviewer
      ↓
Approved
      ↓
Merge
```

This provides another layer of protection.

---

# Status Checks

Automated checks can be required before merging.

Examples:

```text
Build
Tests
Linting
Code Scanning
Security Checks
```

Example:

```text
Pull Request
      │
      ├── Build ✅
      ├── Tests ✅
      ├── CodeQL ✅
      └── Review ✅
             │
             ▼
          Merge
```

If a required check fails, the Pull Request may be prevented from merging until the problem is resolved.

---

# Force Push Protection

Force pushing can rewrite branch history.

For example:

```bash
git push --force
```

on an important shared branch can cause problems.

Branch protection can prevent force pushes to protected branches.

---

# Branch Deletion Protection

Important branches can also be protected from accidental deletion.

For example:

```text
main
production
release
```

should generally not be casually deleted.

---

# Up-to-Date Branches

A branch protection rule can require the Pull Request branch to be updated with the target branch before merging.

Example:

```text
main
 │
 ├── Commit A
 ├── Commit B
 │
 └── Feature Branch
        │
        ▼
   Update with main
        │
        ▼
      Merge
```

This helps ensure that the changes are tested against the latest version of the target branch.

---

# Branch Rules vs Branch Rulesets

GitHub provides branch-related controls through features such as:

- Branch protection rules
- Repository rulesets

Rulesets can provide more centralized and flexible repository policies.

The exact interface and available controls may vary as GitHub evolves.

---

# Example Professional Setup

For a production `main` branch:

```text
main
 │
 ├── Require Pull Request
 │
 ├── Require Review
 │
 ├── Require Status Checks
 │
 ├── Require Conversation Resolution
 │
 └── Block Force Push
```

This creates multiple safeguards around the most important branch.

---

# Branch Protection Workflow

```text
Developer
    │
    ▼
Create Feature Branch
    │
    ▼
Write Code
    │
    ▼
Commit
    │
    ▼
Push Feature Branch
    │
    ▼
Create Pull Request
    │
    ▼
Automated Checks
    │
    ▼
Code Review
    │
    ▼
Approval
    │
    ▼
Merge
    │
    ▼
main
```

---

# Best Practices

- Protect important branches.
- Require Pull Requests for `main`.
- Require appropriate reviews.
- Require automated tests.
- Require security checks where appropriate.
- Prevent force pushes to important branches.
- Prevent accidental branch deletion.
- Keep protection rules documented.

---

# Common Mistakes

- Allowing everyone to push directly to `main`.
- Allowing force pushes on production branches.
- Not requiring tests.
- Giving excessive administrative permissions.
- Creating rules that are so strict that developers bypass them.
- Forgetting to update rules when the development workflow changes.

---

# Interview Questions

### What is branch protection?

Branch protection is a set of GitHub rules that restricts how changes can be made to important branches.

---

### Why protect the `main` branch?

To prevent accidental, unreviewed, or broken changes from being directly introduced into the main codebase.

---

### What is a required status check?

A required status check is an automated check that must pass before a Pull Request can be merged.

---

### Why should force pushes be restricted?

Force pushes can rewrite shared branch history and potentially remove or overwrite other developers' work.

---

### Why require Pull Requests?

Pull Requests provide a controlled process for code review, automated testing, discussion, and approval before merging.

---

# Practice

For a learning repository:

1. Open your GitHub repository.
2. Go to:

```text
Settings
```

3. Explore the repository's branch-related settings.
4. Review available branch protection or ruleset options.
5. Understand how Pull Request requirements work.
6. Explore required status checks.
7. Do not lock yourself out of your own learning repository with overly strict rules.

---

# Summary

Branch protection helps keep important GitHub branches safe from accidental or unauthorized changes.

A strong setup usually combines:

```text
Pull Requests
+
Code Reviews
+
Automated Tests
+
Security Checks
+
Force-Push Protection
```

The goal is simple:

**Important code should be reviewed and verified before it reaches an important branch.**
