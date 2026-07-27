# Merge Conflicts

## Introduction

A Merge Conflict occurs when Git cannot automatically combine changes from two branches. This usually happens when the same part of the same file has been modified in different branches.

Merge conflicts are common in team projects and every developer should know how to resolve them.

---

# What is a Merge Conflict?

A merge conflict occurs when Git does not know which version of the code should be kept.

Example:

```text
main
 │
 ├── README.md → Version A
 │
feature-login
 │
 └── README.md → Version B
```

Both branches modified the same lines.

Git stops the merge and asks the developer to resolve the conflict.

---

# Why Do Merge Conflicts Happen?

Common reasons:

- Two developers edit the same line.
- One branch deletes a file while another modifies it.
- Multiple changes are made to the same section of a file.
- The branches are far behind each other.

---

# Example Conflict

Suppose `README.md` contains:

```text
Git is easy to learn.
```

Branch **main** changes it to:

```text
Git is powerful.
```

Branch **feature-login** changes it to:

```text
Git is beginner friendly.
```

When merging:

```bash
git merge feature-login
```

Git shows:

```text
CONFLICT (content): Merge conflict in README.md
Automatic merge failed.
```

---

# Conflict Markers

Git adds special markers inside the file.

```text
<<<<<<< HEAD
Git is powerful.
=======
Git is beginner friendly.
>>>>>>> feature-login
```

Meaning:

- `<<<<<<< HEAD` → Current branch
- `=======` → Separator
- `>>>>>>> feature-login` → Incoming branch

---

# How to Resolve a Merge Conflict

### Step 1

Open the conflicted file.

---

### Step 2

Read both versions.

---

### Step 3

Choose the correct content.

Example:

```text
Git is a powerful and beginner-friendly version control system.
```

---

### Step 4

Remove all conflict markers.

---

### Step 5

Save the file.

---

### Step 6

Stage the resolved file.

```bash
git add README.md
```

---

### Step 7

Complete the merge.

```bash
git commit
```

---

# Merge Conflict Workflow

```text
Merge Branch
      │
Conflict Found
      │
Open File
      │
Resolve Conflict
      │
git add
      │
git commit
```

---

# Real-World Example

Developer A updates the login screen.

Developer B updates the same login screen at the same time.

When Git tries to merge both branches, it cannot decide which version to keep.

The developers review both changes, combine them, and complete the merge.

---

# Best Practices

- Pull the latest changes regularly.
- Keep branches small.
- Merge frequently.
- Communicate with team members.
- Resolve conflicts carefully.

---

# Common Mistakes

- Deleting the wrong code.
- Leaving conflict markers in the file.
- Committing without testing.
- Ignoring teammate changes.

---

# Interview Questions

### What is a Merge Conflict?

A situation where Git cannot automatically merge changes because the same part of a file was modified differently.

---

### Which command usually causes a merge conflict?

```bash
git merge
```

---

### How do you resolve a merge conflict?

Edit the file, remove the conflict markers, save it, run:

```bash
git add <file>

git commit
```

---

### Are merge conflicts normal?

Yes.

They are common in collaborative software development.

---

# Practice

1. Create two branches.
2. Modify the same line in both branches.
3. Merge them.
4. Resolve the conflict.
5. Commit the resolved changes.

---

# Summary

Merge conflicts are a normal part of Git collaboration. Understanding how to identify, resolve, and test conflicts is an essential skill for every software developer working with Git.
