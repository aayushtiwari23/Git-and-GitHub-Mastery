
# Git Revert (Deep Dive)

## Introduction

Git Revert is a safe Git command used to undo the changes introduced by a commit without removing it from the project's history. Instead of deleting commits, Git creates a new commit that reverses the selected commit.

Unlike Git Reset, Git Revert does **not rewrite commit history**, making it the preferred choice for shared repositories and collaborative development.

---

# What is Git Revert?

Git Revert creates a new commit that undoes the changes made by an earlier commit.

Original History:

```text
A → B → C
```

After reverting commit C:

```text
A → B → C → D
```

Where:

```text
D = Reverse of C
```

The original commit still exists.

---

# Why Use Git Revert?

Developers use Git Revert to:

- Undo bugs safely.
- Preserve commit history.
- Work safely in shared repositories.
- Reverse accidental commits.
- Maintain collaboration.

---

# Git Reset vs Git Revert

| Git Reset | Git Revert |
|------------|------------|
| Rewrites history | Preserves history |
| Removes commits | Creates a new commit |
| Best for local branches | Best for shared branches |
| Can affect collaborators | Safe for everyone |

---

# Basic Command

Undo the latest commit:

```bash
git revert HEAD
```

Git opens your editor to confirm the revert commit message.

---

# Revert a Specific Commit

First:

```bash
git log --oneline
```

Example:

```text
a1b2c3d Add Login

d4e5f6g Update README

7h8i9j0 Initial Commit
```

Revert:

```bash
git revert a1b2c3d
```

---

# Revert Multiple Commits

```bash
git revert commit1 commit2
```

Example:

```bash
git revert a1b2c3d d4e5f6g
```

Git creates separate revert commits.

---

# Revert Without Auto Commit

```bash
git revert --no-commit HEAD
```

Git applies the reverse changes but waits for you to create the commit manually.

Useful when combining multiple reverts.

---

# Handling Conflicts

Sometimes Git cannot automatically revert a commit.

Resolve the conflicts manually.

Then:

```bash
git add .
```

Continue:

```bash
git revert --continue
```

---

# Cancel a Revert

If needed:

```bash
git revert --abort
```

---

# Workflow

```text
Commit Made
      │
      ▼
Bug Found
      │
      ▼
git revert
      │
      ▼
New Revert Commit
      │
      ▼
History Preserved
```

---

# Real-World Example

A developer pushes:

```text
Add Payment Feature
```

Later, the feature causes production errors.

Instead of deleting history:

```bash
git revert HEAD
```

Git creates:

```text
Revert "Add Payment Feature"
```

The project returns to its previous working state while keeping a complete history.

---

# Git Revert vs Git Reset

Example History:

```text
A → B → C
```

Using Reset:

```text
A → B
```

Commit C disappears from branch history.

Using Revert:

```text
A → B → C → D
```

Commit D cancels the changes from C.

---

# Advantages

- Safe for teams.
- No history rewriting.
- Easy to audit.
- Works well with GitHub.
- Ideal for production repositories.

---

# Best Practices

- Use Revert for shared branches.
- Use Reset only for local work.
- Test after reverting.
- Write meaningful revert commit messages.
- Review the reverted changes before pushing.

---

# Common Mistakes

- Using Reset instead of Revert on shared branches.
- Forgetting to test after reverting.
- Reverting the wrong commit.
- Ignoring merge conflicts.

---

# Interview Questions

### What is Git Revert?

Git Revert creates a new commit that reverses the changes introduced by an earlier commit.

---

### Does Git Revert rewrite history?

No.

It preserves the complete commit history.

---

### Which command reverts the latest commit?

```bash
git revert HEAD
```

---

### Which command cancels an ongoing revert?

```bash
git revert --abort
```

---

# Git Revert (Deep Dive)

## Introduction

Git Revert is a safe Git command used to undo the changes introduced by a commit without removing it from the project's history. Instead of deleting commits, Git creates a new commit that reverses the selected commit.

Unlike Git Reset, Git Revert does **not rewrite commit history**, making it the preferred choice for shared repositories and collaborative development.

---

# What is Git Revert?

Git Revert creates a new commit that undoes the changes made by an earlier commit.

Original History:

```text
A → B → C
```

After reverting commit C:

```text
A → B → C → D
```

Where:

```text
D = Reverse of C
```

The original commit still exists.

---

# Why Use Git Revert?

Developers use Git Revert to:

- Undo bugs safely.
- Preserve commit history.
- Work safely in shared repositories.
- Reverse accidental commits.
- Maintain collaboration.

---

# Git Reset vs Git Revert

| Git Reset | Git Revert |
|------------|------------|
| Rewrites history | Preserves history |
| Removes commits | Creates a new commit |
| Best for local branches | Best for shared branches |
| Can affect collaborators | Safe for everyone |

---

# Basic Command

Undo the latest commit:

```bash
git revert HEAD
```

Git opens your editor to confirm the revert commit message.

---

# Revert a Specific Commit

First:

```bash
git log --oneline
```

Example:

```text
a1b2c3d Add Login

d4e5f6g Update README

7h8i9j0 Initial Commit
```

Revert:

```bash
git revert a1b2c3d
```

---

# Revert Multiple Commits

```bash
git revert commit1 commit2
```

Example:

```bash
git revert a1b2c3d d4e5f6g
```

Git creates separate revert commits.

---

# Revert Without Auto Commit

```bash
git revert --no-commit HEAD
```

Git applies the reverse changes but waits for you to create the commit manually.

Useful when combining multiple reverts.

---

# Handling Conflicts

Sometimes Git cannot automatically revert a commit.

Resolve the conflicts manually.

Then:

```bash
git add .
```

Continue:

```bash
git revert --continue
```

---

# Cancel a Revert

If needed:

```bash
git revert --abort
```

---

# Workflow

```text
Commit Made
      │
      ▼
Bug Found
      │
      ▼
git revert
      │
      ▼
New Revert Commit
      │
      ▼
History Preserved
```

---

# Real-World Example

A developer pushes:

```text
Add Payment Feature
```

Later, the feature causes production errors.

Instead of deleting history:

```bash
git revert HEAD
```

Git creates:

```text
Revert "Add Payment Feature"
```

The project returns to its previous working state while keeping a complete history.

---

# Git Revert vs Git Reset

Example History:

```text
A → B → C
```

Using Reset:

```text
A → B
```

Commit C disappears from branch history.

Using Revert:

```text
A → B → C → D
```

Commit D cancels the changes from C.

---

# Advantages

- Safe for teams.
- No history rewriting.
- Easy to audit.
- Works well with GitHub.
- Ideal for production repositories.

---

# Best Practices

- Use Revert for shared branches.
- Use Reset only for local work.
- Test after reverting.
- Write meaningful revert commit messages.
- Review the reverted changes before pushing.

---

# Common Mistakes

- Using Reset instead of Revert on shared branches.
- Forgetting to test after reverting.
- Reverting the wrong commit.
- Ignoring merge conflicts.

---

# Interview Questions

### What is Git Revert?

Git Revert creates a new commit that reverses the changes introduced by an earlier commit.

---

### Does Git Revert rewrite history?

No.

It preserves the complete commit history.

---

### Which command reverts the latest commit?

```bash
git revert HEAD
```

---

### Which command cancels an ongoing revert?

```bash
git revert --abort
```

---

### When should Git Revert be preferred?

When working on shared branches or collaborative repositories.

---

# Practice

1. Create three commits.
2. View history:

```bash
git log --oneline
```

3. Revert the latest commit:

```bash
git revert HEAD
```

4. Observe the new revert commit.

5. Push the changes to GitHub.

---

# Summary

Git Revert is the safest way to undo changes in Git because it preserves project history while reversing unwanted commits. It is the preferred method for shared repositories, production systems, and collaborative development, making it an essential Git command for every professional developer.
### When should Git Revert be preferred?

When working on shared branches or collaborative repositories.

---

