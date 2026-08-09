
# Advanced Git Best Practices

## Introduction

Knowing Git commands is only one part of mastering Git. Professional developers also need to know when and how to use those commands safely.

This guide combines the advanced Git concepts covered in this chapter into practical best practices for real-world development.

---

# 1. Keep Commits Small

Make focused commits that represent one logical change.

Good:

```text
Add login validation
```

Then:

```text
Add login unit tests
```

Avoid:

```text
Update everything
```

Small commits are easier to review, revert, and debug.

---

# 2. Write Meaningful Commit Messages

Good:

```text
Fix incorrect password validation
```

Bad:

```text
changes
```

A good commit message should explain what changed.

---

# 3. Keep Feature Branches Focused

Create separate branches for separate tasks.

Examples:

```text
feature/login
feature/payment
bugfix/navbar
hotfix/security
```

Avoid putting unrelated changes into the same branch.

---

# 4. Keep Your Branch Updated

Before starting work:

```bash
git switch main

git pull origin main
```

Then update your feature branch using the workflow preferred by your team.

For example:

```bash
git switch feature-login

git rebase main
```

---

# 5. Use Rebase Carefully

Rebase is useful for keeping your history clean:

```bash
git rebase main
```

But avoid rebasing shared branches that other developers are actively using.

General rule:

```text
Private branch → Rebase

Shared branch → Prefer Merge or Revert
```

---

# 6. Use Revert for Shared Branches

If a bad commit has already been pushed to a shared branch:

```bash
git revert <commit>
```

This creates a new commit that reverses the changes without rewriting history.

---

# 7. Be Careful with Hard Reset

This command can remove uncommitted work:

```bash
git reset --hard
```

Always check:

```bash
git status
```

before using it.

If you accidentally lose commits, check:

```bash
git reflog
```

---

# 8. Learn Git Reflog

When something goes wrong:

```bash
git reflog
```

It can help recover:

- Lost commits
- Deleted branches
- Previous HEAD positions
- Accidental resets
- Failed rebases

---

# 9. Use Cherry-pick Selectively

Use:

```bash
git cherry-pick <commit>
```

when you need a specific commit without merging an entire branch.

Common use cases:

- Production hotfixes
- Backporting bug fixes
- Applying selected changes

Avoid using Cherry-pick unnecessarily because it can create duplicate commits.

---

# 10. Use Git Bisect for Difficult Bugs

When you know:

```text
Old commit = Good
Current commit = Bad
```

Use:

```bash
git bisect start
```

Then identify the good and bad commits.

Git uses binary search to locate the problematic commit efficiently.

---

# 11. Use Worktrees for Parallel Work

Instead of repeatedly switching branches:

```bash
git worktree add ../hotfix hotfix-payment
```

This allows you to work on multiple branches simultaneously.

---

# 12. Keep Secrets Out of Git

Never commit:

```text
Passwords
API Keys
Private Keys
Database Credentials
Tokens
.env files
```

Use:

- Environment variables
- GitHub Secrets
- Secret management systems

If a secret is accidentally committed, simply deleting it in a later commit may not be enough because it can remain in Git history.

---

# 13. Review Before Pushing

Check:

```bash
git status
```

Then:

```bash
git diff
```

For staged changes:

```bash
git diff --staged
```

Make sure you are committing only what you intended.

---

# 14. Pull Before Pushing

Before pushing collaborative work, make sure your branch is up to date according to your team's workflow.

For example:

```bash
git fetch origin
```

Then integrate the latest changes using the team's preferred merge or rebase strategy.

---

# 15. Protect the Main Branch

Professional repositories commonly protect important branches.

Recommended protections:

- Require Pull Requests.
- Require reviews.
- Require CI checks.
- Prevent direct pushes.
- Require status checks to pass.

---

# 16. Use Pull Requests

Avoid directly pushing feature work into `main`.

Professional workflow:

```text
Feature Branch
      │
      ▼
Pull Request
      │
      ▼
CI Checks
      │
      ▼
Code Review
      │
      ▼
Approval
      │
      ▼
Merge
```

---

# 17. Automate Testing

Use GitHub Actions or another CI system to automatically:

- Build the project.
- Run tests.
- Run linters.
- Check formatting.
- Perform security checks.

Automation reduces human error.

---

# 18. Keep Repository History Clean

Avoid unnecessary commits such as:

```text
test
test2
fix
fix again
final
final2
actual final
```

Before opening a Pull Request, consider cleaning your local commit history using Interactive Rebase when appropriate:

```bash
git rebase -i HEAD~5
```

---

# 19. Don't Rewrite Shared History

Avoid commands such as:

```bash
git reset --hard
git rebase
git push --force
```

on branches other developers are using unless your team explicitly follows a workflow that permits it.

History rewriting can cause problems for collaborators.

---

# 20. Understand Force Push

If you intentionally rewrote your own remote branch history, prefer:

```bash
git push --force-with-lease
```

over:

```bash
git push --force
```

`--force-with-lease` provides an additional safety check and helps prevent accidentally overwriting changes pushed by someone else.

---

# 21. Use Tags for Releases

Create version tags such as:

```text
v1.0.0
v1.1.0
v2.0.0
```

Example:

```bash
git tag v1.0.0
```

Push:

```bash
git push origin v1.0.0
```

Tags make releases easier to identify and reproduce.

---

# 22. Document Important Git Workflows

Every team should document:

- Branch naming
- Commit conventions
- Pull Request rules
- Review requirements
- Release process
- Deployment process

This makes collaboration easier.

---

# 23. Recommended Professional Workflow

```text
Issue
  │
  ▼
Feature Branch
  │
  ▼
Development
  │
  ▼
Small Commits
  │
  ▼
Push
  │
  ▼
Pull Request
  │
  ▼
CI Checks
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
Release
  │
  ▼
Deploy
  │
  ▼
Monitor
```

---

# Advanced Git Decision Guide

| Situation | Recommended Tool |
|-----------|------------------|
| Temporarily save unfinished work | `git stash` |
| Keep history linear | `git rebase` |
| Copy one specific commit | `git cherry-pick` |
| Recover lost commits | `git reflog` |
| Find a bug-introducing commit | `git bisect` |
| Undo shared changes safely | `git revert` |
| Move branch pointer | `git reset` |
| Use multiple branches simultaneously | `git worktree` |
| Include another repository | Git Submodule |

---

# Golden Rules

```text
1. Commit small logical changes.

2. Write meaningful commit messages.

3. Never commit secrets.

4. Review changes before pushing.

5. Use Pull Requests for collaboration.

6. Protect important branches.

7. Rebase private branches carefully.

8. Revert shared changes instead of rewriting history.

9. Use Reflog when recovering lost work.

10. Test before merging.
```

---

# Professional Checklist

Before pushing:

- [ ] `git status` checked
- [ ] Changes reviewed
- [ ] No secrets included
- [ ] Tests pass
- [ ] Commit messages are meaningful
- [ ] Correct branch is being pushed

Before merging:

- [ ] Pull Request opened
- [ ] CI checks pass
- [ ] Code reviewed
- [ ] Review comments resolved
- [ ] Documentation updated if necessary

---

# Summary

Advanced Git is not about memorizing commands. It is about understanding how to use Git safely and effectively.

The most important principles are:

- Keep history understandable.
- Protect shared branches.
- Make small, focused changes.
- Review code before merging.
- Automate testing.
- Know how to recover from mistakes.
- Choose the right Git tool for the situation.

Mastering these practices will make Git much more than a version-control tool. It becomes a reliable part of your professional software development workflow.
