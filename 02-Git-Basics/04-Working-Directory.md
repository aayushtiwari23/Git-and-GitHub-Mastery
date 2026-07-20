# Working Directory

## Introduction

The Working Directory is the place where you create, edit, rename, and delete files in your project. It is the first stage of the Git workflow.

Whenever you make changes to a file, those changes initially exist only in the Working Directory. Git does not automatically save these changes in its history until you explicitly stage and commit them.

---

# What is the Working Directory?

The Working Directory is simply the folder containing your project files.

Example:

```text
StudentManagementSystem
│
├── README.md
├── Program.cs
├── Database.sql
└── appsettings.json
```

When you edit any of these files, the changes remain in the Working Directory.

---

# Position in Git Workflow

```text
Working Directory
       │
 git add
       ▼
Staging Area
       │
git commit
       ▼
Local Repository
       │
 git push
       ▼
Remote Repository (GitHub)
```

Every Git project follows this workflow.

---

# What Can You Do in the Working Directory?

You can:

- Create files
- Edit files
- Delete files
- Rename files
- Move files between folders

Git monitors these changes but does not save them automatically.

---

# Example

Suppose your project contains:

```text
Program.cs
```

You add a new login feature.

The file is now modified, but Git has not yet recorded the change.

To check the status:

```bash
git status
```

Example Output:

```text
modified: Program.cs
```

This means Git has detected the change, but it is still only in the Working Directory.

---

# Why Doesn't Git Save Automatically?

Git gives developers complete control.

Imagine you are working on five different features.

Only one feature is complete.

Git allows you to stage and commit only the completed work instead of everything.

This keeps the project history clean and meaningful.

---

# Developer Tip

Never assume your work is saved just because the file exists.

Until you create a commit, your changes are not part of Git's project history.

---

# Real-World Scenario

A developer updates:

- Login Page
- Dashboard
- Payment Module

Only the Login Page is finished.

Instead of committing everything, the developer stages only the completed Login Page and continues working on the remaining features.

This is a common workflow in professional software development.

---

# Best Practices

- Save your files before checking Git status.
- Review your changes before staging them.
- Commit only completed work.
- Write meaningful commit messages.

---

# Common Mistakes

- Assuming Git saves files automatically.
- Forgetting to stage modified files.
- Committing incomplete features.
- Ignoring `git status` before committing.

---

# Interview Questions

### What is the Working Directory?

The Working Directory is the folder where developers create and modify project files before staging them.

---

### Which command shows changes in the Working Directory?

```bash
git status
```

---

### Does Git automatically save changes made in the Working Directory?

No. Changes must be staged and committed.

---

# Practice

1. Create a new file.
2. Edit an existing file.
3. Run:

```bash
git status
```

4. Observe which files Git marks as modified.

---

# Summary

The Working Directory is the starting point of every Git workflow. It contains all current project files and records every modification until you choose to stage and commit those changes.
