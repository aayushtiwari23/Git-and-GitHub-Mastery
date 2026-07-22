# Git Diff

## Introduction

The `git diff` command is used to compare changes in your project. It helps developers see exactly what has been added, removed, or modified before creating a commit.

It is one of the most important commands for reviewing code and avoiding accidental commits.

---

# What is `git diff`?

The `git diff` command compares different versions of files.

It can compare:

- Working Directory ↔ Staging Area
- Staging Area ↔ Last Commit
- Commit ↔ Commit
- Branch ↔ Branch

This makes it an essential tool for understanding project changes.

---

# Basic Syntax

```bash
git diff
```

This compares the Working Directory with the Staging Area.

---

# Example

Suppose your `README.md` originally contains:

```text
Git is a version control system.
```

You change it to:

```text
Git is a distributed version control system.
```

Run:

```bash
git diff
```

Output:

```diff
-Git is a version control system.
+Git is a distributed version control system.
```

---

# Compare Staged Changes

```bash
git diff --staged
```

or

```bash
git diff --cached
```

Shows the changes that are staged and ready to commit.

---

# Compare Two Commits

```bash
git diff commit1 commit2
```

Example:

```bash
git diff a1b2c3 d4e5f6
```

---

# Compare Two Branches

```bash
git diff main feature-login
```

Useful before merging branches.

---

# Compare a Specific File

```bash
git diff README.md
```

Shows changes only for that file.

---

# Professional Workflow

```bash
git status

git diff

git add .

git diff --staged

git commit -m "docs: update README"

git push
```

Reviewing changes before committing is a common practice in professional software development.

---

# Real-World Example

A developer modifies five files while implementing a new feature.

Before committing, they run:

```bash
git diff
```

They notice an accidental debug statement in one file.

The mistake is removed before the commit is created, keeping the repository clean.

---

# Developer Tip

Always review your changes with:

```bash
git diff
```

before using:

```bash
git add .
```

Then review staged changes using:

```bash
git diff --staged
```

This habit helps prevent accidental commits.

---

# Best Practices

- Review changes before staging.
- Review staged changes before committing.
- Keep commits small and focused.
- Remove temporary or debug code before committing.

---

# Common Mistakes

- Committing without reviewing changes.
- Leaving debugging code in commits.
- Ignoring unexpected modifications.
- Staging unnecessary files.

---

# Interview Questions

### What does `git diff` do?

It compares changes between different versions of files in a Git repository.

---

### Which command compares staged changes?

```bash
git diff --staged
```

---

### Can `git diff` compare branches?

Yes.

Example:

```bash
git diff main feature-login
```

---

### Why is `git diff` important?

It helps developers review changes, detect mistakes, and ensure only intended modifications are committed.

---

# Practice

1. Modify an existing file.
2. Run:

```bash
git diff
```

3. Stage the file.

```bash
git add .
```

4. Run:

```bash
git diff --staged
```

5. Observe how the output changes.

---

# Summary

The `git diff` command is an essential review tool in Git. It allows developers to inspect changes before they are staged or committed, reducing mistakes and improving code quality. Reviewing differences before every commit is a habit followed by experienced developers.
