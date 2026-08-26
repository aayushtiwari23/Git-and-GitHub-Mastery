
# GitHub Pull Requests

## Introduction

A Pull Request (PR) is used to propose changes to a repository and have them reviewed before merging.

Typical workflow:

```text
Issue
  ↓
Branch
  ↓
Code Changes
  ↓
Commit
  ↓
Push
  ↓
Pull Request
  ↓
Code Review
  ↓
Checks
  ↓
Merge
```

---

# Why Pull Requests?

Instead of directly changing `main`:

```text
Developer
   ↓
main
```

use:

```text
Developer
   ↓
Feature Branch
   ↓
Pull Request
   ↓
Review
   ↓
main
```

This protects the main codebase.

---

# Creating a Branch

Example:

```bash
git switch -c feature/dark-mode
```

Make your changes:

```text
feature/dark-mode
```

Then commit:

```bash
git add .
git commit -m "Add dark mode"
```

Push:

```bash
git push -u origin feature/dark-mode
```

---

# Creating a Pull Request

After pushing the branch:

```text
Repository
   ↓
Pull Requests
   ↓
New Pull Request
```

Select:

```text
base: main
compare: feature/dark-mode
```

Then create the Pull Request.

---

# Base Branch

The base branch is where the changes will eventually be merged.

Usually:

```text
main
```

Example:

```text
base:
main
```

---

# Compare Branch

The compare branch contains your changes.

Example:

```text
compare:
feature/dark-mode
```

So:

```text
feature/dark-mode
        ↓
      main
```

---

# Pull Request Title

Use a clear title.

Good:

```text
Add dark mode support
```

Bad:

```text
Changes
```

Good:

```text
Fix login validation error
```

---

# Pull Request Description

A useful PR description explains:

```text
What changed?
Why was it changed?
How was it tested?
Is anything still pending?
```

Example:

```md
## Summary

Added dark mode support to the dashboard.

## Changes

- Added theme toggle
- Added dark theme styles
- Updated dashboard components

## Testing

- Tested theme switching
- Tested page refresh
- Tested mobile layout

## Related Issue

Fixes #12
```

---

# Linking an Issue

Use:

```text
Fixes #12
```

or:

```text
Closes #12
```

or:

```text
Resolves #12
```

This connects the Pull Request to the Issue.

---

# Draft Pull Request

A Draft Pull Request indicates that the work is not ready for final review.

Use it when:

```text
Work is incomplete
Feedback is needed early
Implementation is still changing
```

Typical flow:

```text
Draft PR
   ↓
Development
   ↓
Testing
   ↓
Ready for Review
   ↓
Review
```

---

# Pull Request Review

Reviewers examine:

```text
Code Quality
Correctness
Security
Performance
Tests
Documentation
Maintainability
```

They may:

```text
Approve
Request Changes
Comment
```

---

# Approve

Approval means the reviewer is satisfied with the proposed changes.

Conceptually:

```text
PR
 ↓
Review
 ↓
Approved
 ↓
Merge
```

---

# Request Changes

A reviewer can request changes.

Flow:

```text
PR
 ↓
Review
 ↓
Changes Requested
 ↓
Developer Fixes Code
 ↓
Push New Commit
 ↓
Review Again
```

The Pull Request remains open while the changes are being addressed.

---

# PR Comments

Comments can discuss specific parts of the code.

Example:

```text
Could this logic be moved into a separate function?
```

The developer can respond and make the required change.

---

# Code Review Workflow

```text
Developer
   ↓
Pull Request
   ↓
Reviewer
   ↓
Comments
   ↓
Developer Updates
   ↓
Reviewer Re-checks
   ↓
Approval
```

---

# Automated Checks

GitHub Actions can automatically run when a Pull Request is created or updated.

Example:

```text
Pull Request
      ↓
GitHub Actions
      ↓
Lint
      ↓
Tests
      ↓
Build
```

If a check fails:

```text
PR
 ↓
CI
 ↓
FAIL
 ↓
Fix
 ↓
Push
 ↓
CI Runs Again
```

---

# Required Status Checks

Repositories can configure certain checks as required before merging.

Example:

```text
Required:
✓ Build
✓ Tests
✓ Lint
```

If one fails:

```text
Merge
  ↓
Blocked
```

This helps protect important branches.

---

# Merge Options

Depending on repository settings, GitHub can offer different merge strategies.

Common strategies include:

```text
Merge Commit
Squash Merge
Rebase Merge
```

---

# Merge Commit

A merge commit combines the branch into the target branch while preserving the branch history.

Conceptually:

```text
A ─ B ─ C
     \
      D ─ E
           \
            M
```

`M` is the merge commit.

---

# Squash Merge

Squash merge combines the Pull Request's commits into one commit.

Before:

```text
Commit A
Commit B
Commit C
```

After:

```text
One Clean Commit
```

This can keep the main branch history simpler.

---

# Rebase Merge

Rebase applies the branch commits on top of the latest base branch and produces a linear history.

Conceptually:

```text
Before:

main:     A ─ B
               \
feature:        C ─ D

After:

main:     A ─ B ─ C ─ D
```

---

# Choosing a Merge Strategy

There is no single strategy for every project.

Common reasoning:

```text
Squash
→ Clean main history

Merge Commit
→ Preserve branch history

Rebase
→ Linear history
```

The project's contribution rules should determine which strategy is used.

---

# Merge Conflicts

A conflict occurs when Git cannot automatically combine changes.

Example:

```text
main:
Hello World

feature:
Hello GitHub
```

Both branches changed the same part of the file.

Git may report:

```text
CONFLICT
```

---

# Resolving a Conflict

Typical process:

```text
Pull Latest main
      ↓
Merge/Rebase
      ↓
Conflict
      ↓
Edit File
      ↓
Resolve Conflict
      ↓
Commit
      ↓
Push
      ↓
PR Updated
```

---

# Conflict Markers

Git may insert:

```text
<<<<<<< HEAD
Current branch content
=======
Incoming content
>>>>>>> feature-branch
```

You must decide which content should remain.

Then remove the conflict markers.

---

# Updating a Pull Request

If `main` changes while your PR is open:

```text
main
 ↓
New Commits
 ↓
PR Branch Becomes Outdated
```

You may need to update your branch.

One approach:

```bash
git fetch origin
git merge origin/main
```

Resolve any conflicts.

Then:

```bash
git push
```

The Pull Request automatically updates.

---

# PR Review Checklist

Before requesting review:

```text
[ ] Code works
[ ] Tests pass
[ ] No unnecessary files
[ ] No secrets committed
[ ] Code is formatted
[ ] Documentation updated
[ ] PR description is clear
[ ] Related Issue linked
```

---

# Small Pull Requests

Prefer focused Pull Requests.

Good:

```text
PR #10
Add login validation
```

Less ideal:

```text
PR #11
Login + Dashboard + Payment + Database + UI redesign
```

Smaller PRs are easier to:

```text
Review
Test
Understand
Debug
Merge
```

---

# PR Size

A useful workflow is:

```text
Small Change
   ↓
Small PR
   ↓
Fast Review
   ↓
Fast Merge
```

This doesn't mean every PR must be tiny. The goal is to keep the scope understandable.

---

# Pull Request Templates

Repositories can provide a standard PR template.

Example:

```md
## Summary

Describe the change.

## Changes

- 
- 
- 

## Testing

- 
- 

## Checklist

- [ ] Tests pass
- [ ] Documentation updated
- [ ] No secrets added

## Related Issue

Fixes #
```

This helps contributors provide consistent information.

---

# CODEOWNERS

`CODEOWNERS` can automatically request reviews from responsible people for specific files.

Example:

```text
*.js @developer
.github/workflows/ @maintainer
```

This is especially useful for sensitive files.

---

# Branch Protection

Important branches can require:

```text
Pull Request
Required Reviews
Status Checks
```

before merging.

Conceptually:

```text
Developer
   ↓
Feature Branch
   ↓
PR
   ↓
Review
   ↓
CI
   ↓
All Requirements Pass
   ↓
Merge
```

---

# PR Security

Never put sensitive information into a Pull Request.

Avoid:

```text
Passwords
API Keys
Tokens
Private Credentials
```

If accidentally committed:

```text
Revoke
 ↓
Rotate
 ↓
Investigate
```

Do not rely only on deleting the commit.

---

# Fork-Based Contributions

Open-source contributors may not have direct write access to the repository.

They can use:

```text
Original Repository
       ↓
Fork
       ↓
Contributor Changes
       ↓
Pull Request
       ↓
Original Repository
```

---

# Fork Workflow

Typical process:

```text
Fork Repository
      ↓
Clone Fork
      ↓
Create Branch
      ↓
Make Changes
      ↓
Commit
      ↓
Push
      ↓
Pull Request
```

---

# Maintainer Workflow

The maintainer receives the PR:

```text
Contributor
    ↓
Pull Request
    ↓
Automated Checks
    ↓
Review
    ↓
Changes Requested
        OR
      Approval
        ↓
      Merge
```

---

# Real-World GitHub Workflow

```text
              Issue
                ↓
             Branch
                ↓
             Coding
                ↓
             Commit
                ↓
              Push
                ↓
          Pull Request
                ↓
        ┌───────┴────────┐
        ↓                ↓
      Review             CI
        ↓                ↓
        └───────┬────────┘
                ↓
            Approved
                ↓
              Merge
                ↓
           Issue Closed
```

---

# Practice

Use one of the Issues you created earlier.

For example:

```text
Issue #1
Add project documentation
```

Create a branch:

```bash
git switch -c docs/project-documentation
```

Make the documentation change.

Commit:

```bash
git add .
git commit -m "Improve project documentation"
```

Push:

```bash
git push -u origin docs/project-documentation
```

Create a Pull Request.

Title:

```text
Improve project documentation
```

Description:

```md
## Summary

Improved the repository documentation.

## Changes

- Added clearer project information
- Improved setup instructions
- Improved usage information

## Testing

Reviewed the documentation for formatting and clarity.

## Related Issue

Fixes #1
```

---

# Your PR Checklist

Before merging:

```text
[ ] Correct branch selected
[ ] PR title is clear
[ ] Description is complete
[ ] Issue linked
[ ] CI passes
[ ] No secrets committed
[ ] Changes reviewed
[ ] Conflicts resolved
```

Then merge the Pull Request.

After merging:

```text
Pull Request
    ↓
Merged
    ↓
Issue #1
    ↓
Closed
```

---

# Important Commands

Create branch:

```bash
git switch -c feature/name
```

Check status:

```bash
git status
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "Describe change"
```

Push:

```bash
git push -u origin feature/name
```

Update local repository:

```bash
git fetch origin
```

Merge latest main:

```bash
git merge origin/main
```

---

# Summary

Pull Requests are the bridge between development and collaboration.

Remember:

```text
Issue
 ↓
Branch
 ↓
Code
 ↓
Commit
 ↓
Push
 ↓
Pull Request
 ↓
CI
 ↓
Code Review
 ↓
Approval
 ↓
Merge
 ↓
Issue Closed
```

This workflow is fundamental to professional Git and GitHub development.
