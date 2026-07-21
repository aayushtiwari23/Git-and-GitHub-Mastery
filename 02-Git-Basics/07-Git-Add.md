# Git Add

## Introduction

The `git add` command is used to move changes from the Working Directory to the Staging Area. It tells Git which files should be included in the next commit.

Without using `git add`, Git will not include your changes when you create a commit.

It is one of the most frequently used commands in everyday development.

---

# Why Do We Need `git add`?

Git follows a two-step process before saving your work:

1. Stage the required changes.
2. Create a commit.

This gives developers complete control over what gets recorded in the project's history.

---

# Command Syntax

```bash
git add <file-name>
```

Example:

```bash
git add Program.cs
```

---

# Stage a Single File

Command:

```bash
git add README.md
```

Only the specified file is added to the Staging Area.

---

# Stage Multiple Files

Command:

```bash
git add README.md Program.cs
```

Both files are staged together.

---

# Stage All Files

Command:

```bash
git add .
```

Stages all new, modified, and deleted files in the current directory.

---

# Stage All Files (Alternative)

```bash
git add -A
```

Stages every change in the repository.

---

# Workflow Example

Suppose your project contains:

```text
StudentManagementSystem
│
├── README.md
├── Program.cs
├── Login.cs
└── Database.sql
```

You modify:

- Program.cs
- Login.cs

Check the status:

```bash
git status
```

Output:

```text
modified: Program.cs
modified: Login.cs
```

Stage only `Program.cs`:

```bash
git add Program.cs
```

Check again:

```bash
git status
```

Output:

```text
Changes to be committed:
    modified: Program.cs

Changes not staged:
    modified: Login.cs
```

Only `Program.cs` is ready to be committed.

---

# Professional Workflow

```text
Edit File
    │
git status
    │
git add
    │
git status
    │
git commit
    │
git push
```

---

# Common Options

## Add a Specific File

```bash
git add file.txt
```

---

## Add Every File

```bash
git add .
```

---

## Add Everything in the Repository

```bash
git add -A
```

---

## Add All Files with a Specific Extension

Example:

```bash
git add *.cs
```

Stages every C# file in the current directory.

---

# Developer Tip

Avoid blindly using:

```bash
git add .
```

Always run:

```bash
git status
```

first to verify which files have changed.

---

# Best Practices

- Review changes before staging.
- Stage only completed work.
- Keep commits focused on one feature or bug fix.
- Check the status after staging.

---

# Common Mistakes

- Staging unwanted files.
- Forgetting to stage new files.
- Running `git add .` without checking changes.
- Mixing unrelated changes in one commit.

---

# Interview Questions

### What is the purpose of `git add`?

It moves selected changes from the Working Directory to the Staging Area.

---

### Which command stages all files?

```bash
git add .
```

---

### Which command stages a single file?

```bash
git add filename
```

---

### Does `git add` create a commit?

No.

It only stages changes.

---

# Practice

1. Create two files.
2. Modify one existing file.
3. Stage only one file.
4. Run:

```bash
git status
```

5. Stage the remaining files.

6. Run:

```bash
git status
```

Observe the differences.

---

# Summary

The `git add` command is used to prepare changes for the next commit. It gives developers precise control over which files become part of the project's history, making it an essential command in every Git workflow.
