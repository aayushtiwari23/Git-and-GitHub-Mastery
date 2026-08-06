# Git Bisect

## Introduction

Finding the exact commit that introduced a bug can be difficult in large projects with hundreds or thousands of commits. Instead of checking each commit manually, Git provides **Git Bisect**, a powerful debugging tool that uses the **Binary Search Algorithm** to quickly identify the faulty commit.

Git Bisect significantly reduces debugging time and is commonly used in professional software development.

---

# What is Git Bisect?

Git Bisect is a Git command that helps locate the commit responsible for introducing a bug.

It repeatedly checks commits between a known good commit and a known bad commit until the problematic commit is found.

---

# Why Use Git Bisect?

Developers use Git Bisect to:

- Find the commit that introduced a bug.
- Reduce debugging time.
- Avoid manually checking every commit.
- Debug large projects efficiently.
- Identify regressions.

---

# How Binary Search Works

Instead of checking every commit one by one:

```text
A → B → C → D → E → F → G → H
```

Git checks the middle commit first.

If the bug exists:

```text
Search Right Half
```

Otherwise:

```text
Search Left Half
```

This process repeats until only one commit remains.

---

# Time Complexity

Without Git Bisect:

```text
1000 commits
↓

Check up to 1000 commits
```

With Git Bisect:

```text
1000 commits
↓

About 10 checks
```

Binary Search complexity:

```text
O(log₂ n)
```

---

# Basic Workflow

```text
Known Good Commit
        │
        ▼
Start Bisect
        │
        ▼
Known Bad Commit
        │
        ▼
Git Chooses Middle Commit
        │
        ▼
Test Project
        │
 ┌───────────────┐
 │               │
Good          Bad
 │               │
 ▼               ▼
Repeat      Repeat
        │
        ▼
Faulty Commit Found
```

---

# Step 1: Start Bisect

```bash
git bisect start
```

---

# Step 2: Mark the Bad Commit

Usually the current commit contains the bug.

```bash
git bisect bad
```

---

# Step 3: Mark a Good Commit

Example:

```bash
git bisect good a1b2c3d
```

Git now checks out the middle commit automatically.

---

# Step 4: Test the Project

If the bug exists:

```bash
git bisect bad
```

If the bug does not exist:

```bash
git bisect good
```

Git continues selecting commits until the faulty one is identified.

---

# Step 5: Finish

After finding the bad commit:

```bash
git bisect reset
```

This returns you to your original branch.

---

# Example

Commit history:

```text
A B C D E F G H
```

Suppose:

- A works.
- H has a bug.

Commands:

```bash
git bisect start

git bisect bad

git bisect good A
```

Git selects:

```text
D
```

If D is good:

```bash
git bisect good
```

Git tests:

```text
F
```

Continue until Git reports:

```text
Commit E introduced the bug.
```

---

# Automating Git Bisect

If you have an automated test:

```bash
git bisect run ./test.sh
```

Git automatically checks each commit and runs the script until it finds the faulty commit.

This is extremely useful in CI/CD pipelines.

---

# Real-World Example

A web application crashes after a recent update.

There are 800 commits since the last stable release.

Instead of testing all 800 commits manually:

```bash
git bisect start

git bisect bad

git bisect good v2.1
```

After about 10 tests, Git identifies the exact commit that introduced the bug.

---

# Advantages

- Extremely fast debugging.
- Uses Binary Search.
- Saves hours of manual work.
- Works well on large repositories.
- Can be automated with test scripts.

---

# Best Practices

- Know one good commit and one bad commit.
- Use automated tests whenever possible.
- Reset Bisect after finishing.
- Test each selected commit carefully.

---

# Common Mistakes

- Choosing the wrong good commit.
- Forgetting to run:

```bash
git bisect reset
```

- Misclassifying commits as good or bad.
- Ignoring test failures unrelated to the bug.

---

# Git Bisect Commands

| Command | Purpose |
|---------|---------|
| `git bisect start` | Start Bisect |
| `git bisect good` | Mark current commit as good |
| `git bisect bad` | Mark current commit as bad |
| `git bisect reset` | End Bisect and return |
| `git bisect run` | Automate testing |

---

# Interview Questions

### What is Git Bisect?

Git Bisect is a Git debugging tool that uses binary search to locate the commit that introduced a bug.

---

### Which algorithm does Git Bisect use?

Binary Search.

---

### What is the time complexity of Git Bisect?

```text
O(log n)
```

---

### Which command starts Git Bisect?

```bash
git bisect start
```

---

### Which command ends Git Bisect?

```bash
git bisect reset
```

---

### Can Git Bisect run automated tests?

Yes.

Using:

```bash
git bisect run <script>
```

---

# Practice

1. Create several commits.
2. Introduce a small bug.
3. Start Git Bisect.
4. Mark one good commit and one bad commit.
5. Follow Git's prompts until the faulty commit is found.
6. Run:

```bash
git bisect reset
```

7. Verify you're back on your original branch.

---

# Summary

Git Bisect is an advanced debugging tool that uses binary search to efficiently locate the commit responsible for introducing a bug. It is especially valuable in large projects, where manually checking every commit would be time-consuming. Mastering Git Bisect makes debugging faster, more systematic, and more reliable.
