
# Git Cherry-pick

## Introduction

Git Cherry-pick is an advanced Git command that allows you to copy one or more specific commits from one branch and apply them to another branch. Unlike merging, which brings all commits from a branch, cherry-pick lets you choose exactly which commits to transfer.

It is commonly used for hotfixes, bug fixes, and selectively applying features without merging an entire branch.

---

# What is Git Cherry-pick?

Git Cherry-pick copies the changes introduced by a specific commit and creates a new commit on the current branch.

The original commit remains unchanged in its original branch.

---

# Why Use Git Cherry-pick?

Developers use Cherry-pick to:

- Copy a single bug fix.
- Apply a hotfix to another branch.
- Reuse a feature without merging.
- Recover important commits.
- Backport fixes to older versions.

---

# Basic Workflow

```text
main
 │
 ├────A────B────C
 │
feature
 │
 ├────D────E────F
```

Cherry-pick commit **E**:

```text
main
 │
 ├────A────B────C────E'
```

Notice that **E'** is a new commit with the same changes.

---

# Basic Command

```bash
git cherry-pick <commit-hash>
```

Example:

```bash
git cherry-pick a1b2c3d
```

---

# Finding a Commit Hash

Use:

```bash
git log --oneline
```

Example:

```text
a1b2c3d Fix login validation

9e8f7a6 Update README

5f4e3d2 Add dashboard
```

Cherry-pick:

```bash
git cherry-pick a1b2c3d
```

---

# Cherry-pick Multiple Commits

```bash
git cherry-pick commit1 commit2 commit3
```

Example:

```bash
git cherry-pick a1b2c3d 9e8f7a6
```

---

# Cherry-pick a Range of Commits

```bash
git cherry-pick A^..D
```

Example:

```bash
git cherry-pick a1b2c3d^..d4e5f6g
```

This copies all commits from A through D.

---

# Handling Conflicts

Sometimes Git cannot apply a commit automatically.

Git pauses the process and reports a conflict.

Resolve the conflict manually.

Then stage the resolved files:

```bash
git add .
```

Continue:

```bash
git cherry-pick --continue
```

---

# Abort Cherry-pick

If needed:

```bash
git cherry-pick --abort
```

Git restores the branch to its previous state.

---

# Skip the Current Commit

```bash
git cherry-pick --skip
```

Git skips the problematic commit and continues.

---

# Real-World Example

Suppose your project has:

```text
main

release-v1.0

feature-payment
```

A critical security bug is fixed in:

```text
feature-payment
```

Instead of merging the entire feature branch into the release branch:

```bash
git switch release-v1.0

git cherry-pick a1b2c3d
```

Only the security fix is copied into the release version.

---

# Cherry-pick Workflow

```text
feature
 │
 ├────A────B────C
          │
          │ Cherry-pick
          ▼
main
 │
 ├────X────Y────B'
```

---

# Advantages

- Copy only required commits.
- Avoid unnecessary merges.
- Useful for production hotfixes.
- Preserve branch independence.
- Flexible workflow.

---

# Best Practices

- Verify the correct commit hash before cherry-picking.
- Test the project after cherry-picking.
- Use meaningful commit messages.
- Resolve conflicts carefully.
- Document why a commit was cherry-picked if working in a team.

---

# Common Mistakes

- Cherry-picking the wrong commit.
- Cherry-picking duplicate commits.
- Forgetting to test after applying changes.
- Ignoring merge conflicts.
- Using Cherry-pick when a merge is more appropriate.

---

# Cherry-pick vs Merge

| Cherry-pick | Merge |
|-------------|-------|
| Copies selected commits | Combines entire branches |
| Creates new commits | Preserves original commits |
| Good for hotfixes | Good for full integration |
| Selective | Complete branch merge |

---

# Interview Questions

### What is Git Cherry-pick?

Git Cherry-pick copies a specific commit from one branch and applies it to another branch.

---

### Which command copies a single commit?

```bash
git cherry-pick <commit-hash>
```

---

### How do you continue after resolving a conflict?

```bash
git cherry-pick --continue
```

---

### Which command cancels a Cherry-pick?

```bash
git cherry-pick --abort
```

---

### When should Cherry-pick be used?

When only selected commits need to be transferred instead of merging an entire branch.

---

# Practice

1. Create two branches.
2. Make two commits on one branch.
3. Copy one commit to the other branch using:

```bash
git cherry-pick <commit-hash>
```

4. Verify the history:

```bash
git log --oneline
```

5. Compare both branches.

---

# Summary

Git Cherry-pick is a powerful Git command that copies selected commits from one branch to another. It is especially useful for hotfixes, backporting bug fixes, and selectively applying changes without merging an entire branch. Mastering Cherry-pick is valuable for both professional software development and technical interviews.
