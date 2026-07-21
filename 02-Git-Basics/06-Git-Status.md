# Git Status

## Introduction

One of the most frequently used Git commands is `git status`. It shows the current state of your repository by displaying modified files, staged files, untracked files, and the active branch.

Professional developers use this command repeatedly while working because it provides a quick overview of what has changed.

---

# What is `git status`?

The `git status` command displays:

- Current branch
- Staged files
- Modified files
- Untracked files
- Files ready to commit
- Whether your branch is ahead or behind the remote repository

Syntax:

```bash
git status
```

---

# Git Workflow with Status

```text
                git status

        Working Directory
                │
                ▼
        Shows Modified Files
                │
        git add
                ▼
         Shows Staged Files
                │
       git commit
                ▼
      Working Tree Clean
```

---

# Example 1: New Repository

Command:

```bash
git status
```

Output:

```text
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Meaning:

- Repository is empty.
- No files are being tracked.

---

# Example 2: New File

Suppose you create:

```text
README.md
```

Run:

```bash
git status
```

Output:

```text
Untracked files:

README.md
```

Meaning:

Git has detected the file, but it is not yet being tracked.

---

# Example 3: After Staging

Command:

```bash
git add README.md
git status
```

Output:

```text
Changes to be committed:

new file: README.md
```

Meaning:

The file is staged and ready to be committed.

---

# Example 4: After Commit

Command:

```bash
git commit -m "Add README"
git status
```

Output:

```text
On branch main

nothing to commit, working tree clean
```

Meaning:

Everything has been committed successfully.

---

# Common Status Messages

## Untracked Files

Git has never tracked these files.

---

## Changes Not Staged

Files were modified but have not been added to the Staging Area.

---

## Changes to Be Committed

Files are staged and ready for the next commit.

---

## Working Tree Clean

There are no pending changes.

Everything is committed.

---

# Real-World Workflow

```text
Create File
      │
git status
      │
Shows Untracked File
      │
git add
      │
git status
      │
Shows Staged File
      │
git commit
      │
git status
      │
Working Tree Clean
```

---

# Professional Workflow

Experienced developers often follow this sequence:

```bash
git status

git add .

git status

git commit -m "Meaningful commit message"

git status

git push
```

Checking the repository status before and after important commands helps avoid mistakes.

---

# Developer Tip

Run `git status` before every commit.

It helps you confirm:

- Which files are staged
- Which files are modified
- Whether you accidentally included unwanted changes

---

# Best Practices

- Check `git status` frequently.
- Review staged files before committing.
- Commit only related changes.
- Keep your working tree clean whenever possible.

---

# Common Mistakes

- Committing without checking `git status`.
- Forgetting to stage files.
- Accidentally committing unwanted files.
- Ignoring untracked files that should be added.

---

# Interview Questions

### What does `git status` do?

It displays the current state of the repository, including staged, modified, and untracked files.

---

### Which command checks the repository status?

```bash
git status
```

---

### What does "working tree clean" mean?

All tracked changes have been committed, and there are no pending modifications.

---

### What are untracked files?

Files that Git has detected but is not yet tracking.

---

# Practice

1. Create a new file.
2. Run:

```bash
git status
```

3. Stage the file.

4. Run:

```bash
git status
```

5. Commit the file.

6. Run:

```bash
git status
```

Observe how the output changes after each step.

---

# Summary

`git status` is one of the most important Git commands. It provides a complete overview of your repository and helps you understand what Git is tracking, what is staged, and what is ready to commit. Using it regularly is a fundamental habit of professional developers.
