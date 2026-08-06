# Git Reflog

## Introduction

Git Reflog (Reference Log) is one of Git's most powerful recovery tools. It records every change made to the HEAD and branch references in your local repository, allowing you to recover commits, branches, and changes that may seem lost.

Even if a commit is no longer visible in the normal Git history, it can often be recovered using Reflog.

---

# What is Git Reflog?

Git Reflog is a local log that tracks where the HEAD and branch references have pointed over time.

It records actions such as:

- Commits
- Checkouts
- Merges
- Rebases
- Resets
- Cherry-picks
- Branch creation
- Branch deletion

Unlike `git log`, Reflog also tracks changes that are no longer part of the current branch history.

---

# Why Use Git Reflog?

Developers use Reflog to:

- Recover deleted commits.
- Restore deleted branches.
- Undo accidental resets.
- Recover after a failed rebase.
- Find lost work.
- Restore previous HEAD positions.

---

# Git Log vs Git Reflog

| Git Log | Git Reflog |
|----------|------------|
| Shows commit history | Shows HEAD and reference history |
| Shared history | Local history only |
| Does not show discarded commits | Can show discarded commits |
| Used for project history | Used for recovery |

---

# View the Reflog

```bash
git reflog
```

Example:

```text
7a9f2d1 HEAD@{0}: commit: Add login validation

5c3e8b4 HEAD@{1}: checkout: moving from main to feature-login

2d8c4a7 HEAD@{2}: rebase finished

1a2b3c4 HEAD@{3}: reset: moving to HEAD~1
```

Each entry represents a previous state of `HEAD`.

---

# Recover a Lost Commit

Suppose you accidentally reset your branch.

Use:

```bash
git reflog
```

Find the desired entry:

```text
HEAD@{3}
```

Restore it:

```bash
git reset --hard HEAD@{3}
```

---

# Recover Using Commit Hash

Instead of `HEAD@{}`:

```bash
git reset --hard a1b2c3d
```

---

# Recover a Deleted Branch

Suppose you accidentally delete:

```text
feature-payment
```

Find the last commit using:

```bash
git reflog
```

Recreate the branch:

```bash
git branch feature-payment a1b2c3d
```

---

# Recover After an Accidental Rebase

View the Reflog:

```bash
git reflog
```

Find the state before the rebase.

Restore it:

```bash
git reset --hard HEAD@{4}
```

---

# Recover After a Hard Reset

Even after:

```bash
git reset --hard HEAD~3
```

The discarded commits usually remain in the Reflog until they expire.

Restore them:

```bash
git reset --hard HEAD@{1}
```

---

# Typical Recovery Workflow

```text
Accident
     │
     ▼
git reflog
     │
Find Previous HEAD
     │
git reset --hard HEAD@{n}
     │
Recovered
```

---

# Real-World Example

A developer accidentally runs:

```bash
git reset --hard HEAD~5
```

Five commits disappear from the visible history.

Instead of panicking:

```bash
git reflog
```

The developer finds:

```text
HEAD@{6}
```

Then restores everything:

```bash
git reset --hard HEAD@{6}
```

All lost commits return.

---

# Important Notes

- Reflog exists only on your local machine.
- It is not shared with GitHub or other collaborators.
- Old Reflog entries eventually expire and may be removed by Git's garbage collection.
- Recover lost work as soon as possible.

---

# Advantages

- Recover lost commits.
- Restore deleted branches.
- Undo accidental resets.
- Recover after failed rebases.
- Acts as Git's recovery mechanism.

---

# Best Practices

- Check Reflog before assuming work is lost.
- Avoid unnecessary `--hard` resets.
- Create backup branches before major history changes.
- Learn Reflog before using advanced Git commands.

---

# Common Mistakes

- Assuming `git reset --hard` permanently deletes work.
- Forgetting that Reflog is local only.
- Waiting too long to recover deleted commits.
- Confusing `git log` with `git reflog`.

---

# Interview Questions

### What is Git Reflog?

Git Reflog is a local history of HEAD and branch reference movements that helps recover lost commits and branches.

---

### Which command displays the Reflog?

```bash
git reflog
```

---

### Can Git Reflog recover deleted commits?

Yes, as long as the Reflog entry still exists.

---

### Is Git Reflog shared with GitHub?

No.

It exists only in the local repository.

---

### Which command restores a previous HEAD position?

```bash
git reset --hard HEAD@{n}
```

---

