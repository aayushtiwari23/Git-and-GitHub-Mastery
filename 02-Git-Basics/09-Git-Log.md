# Git Log

## Introduction

Every project has a history, and Git records that history through commits. The `git log` command allows developers to view all commits made in a repository, helping them understand what changed, who made the changes, and when they were made.

It is one of the most commonly used commands in Git and is essential for debugging, reviewing project progress, and collaborating with teams.

---

# What is `git log`?

The `git log` command displays the commit history of the current repository.

It shows:

- Commit SHA
- Author
- Date
- Commit Message

Command:

```bash
git log
```

---

# Example Output

```text
commit 8d91f8c91a4c8d...

Author: Aayush Suraj Tiwari

Date: Mon Jul 27 18:40:52 2026

Add Git Commit Guide

commit 1b75e73d8ab...

Author: Aayush Suraj Tiwari

Date: Mon Jul 27 17:15:20 2026

Add Git Add Guide
```

Each commit represents a snapshot of your project.

---

# Understanding the Output

## Commit

A unique SHA identifier.

Example:

```text
8d91f8c91a4c8d...
```

---

## Author

The developer who created the commit.

---

## Date

The date and time when the commit was created.

---

## Commit Message

A short description explaining the purpose of the commit.

---

# Common `git log` Commands

## Show Complete History

```bash
git log
```

---

## Show Compact History

```bash
git log --oneline
```

Example:

```text
8d91f8c Add Git Commit Guide

1b75e73 Add Git Add Guide

39e7ab1 Add Git Status Guide
```

This format is easier to read.

---

## Show Last 5 Commits

```bash
git log -5
```

---

## Show Graph View

```bash
git log --graph
```

Useful for understanding branches and merges.

---

## Show One-Line Graph

```bash
git log --oneline --graph
```

Example:

```text
* 8d91f8c Add Git Commit Guide
* 1b75e73 Add Git Add Guide
* 39e7ab1 Add Git Status Guide
```

---

## Show Detailed Changes

```bash
git log -p
```

Displays the actual code changes for every commit.

---

# Professional Workflow

```bash
git status

git add .

git commit -m "feat: add authentication"

git log --oneline
```

Professional developers frequently use `git log --oneline` because it provides a clean overview of the project's history.

---

# Real-World Example

A bug appears in the Login Module.

Before making changes, the developer checks:

```bash
git log
```

The history shows that the login feature was added two days ago.

The developer identifies the relevant commit, reviews the changes, and fixes the issue more efficiently.

---

# Developer Tip

For daily development, prefer:

```bash
git log --oneline
```

It is faster to read and gives a concise history.

Use the full `git log` only when you need detailed information.

---

# Best Practices

- Write meaningful commit messages.
- Review commit history regularly.
- Keep commits small and focused.
- Use `--oneline` for quick inspection.

---

# Common Mistakes

- Writing unclear commit messages.
- Creating very large commits.
- Ignoring commit history during debugging.
- Making unrelated changes in one commit.

---

# Interview Questions

### What does `git log` do?

It displays the commit history of the current repository.

---

### Which command shows a compact commit history?

```bash
git log --oneline
```

---

### Which command displays commits as a graph?

```bash
git log --graph
```

---

### Why is commit history important?

It helps developers track changes, debug issues, understand project evolution, and collaborate effectively.

---

# Practice

1. Create two new commits.

2. View the complete history.

```bash
git log
```

3. View the compact history.

```bash
git log --oneline
```

4. View the graph.

```bash
git log --oneline --graph
```

Compare the output of each command.

---

# Summary

The `git log` command is an essential tool for exploring a project's history. It helps developers review commits, identify changes, debug issues, and understand how a project has evolved over time. Mastering `git log` is a key step toward becoming proficient with Git.
