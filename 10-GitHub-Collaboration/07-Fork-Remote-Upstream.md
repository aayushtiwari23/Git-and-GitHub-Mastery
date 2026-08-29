# Fork, Remote and Upstream

## Introduction

When contributing to an open-source project, you may not have permission to directly push changes to the original repository.

GitHub provides a workflow using:

```text
Fork
 ↓
Clone
 ↓
Remote
 ↓
Upstream
 ↓
Branch
 ↓
Pull Request
```

---

# What Is a Fork?

A fork is your GitHub copy of another repository.

Example:

```text
Original Repository
        ↓
       Fork
        ↓
Your GitHub Account
```

You can make changes to your fork without having write access to the original repository.

---

# Original Repository

The original project is commonly called:

```text
Upstream
```

Example:

```text
Original:
github.com/project/example
```

This is where the actual open-source project is maintained.

---

# Your Fork

Your fork is commonly used as:

```text
Origin
```

Example:

```text
Your Fork:
github.com/your-account/example
```

---

# Origin vs Upstream

Remember:

```text
origin
   ↓
Your Fork

upstream
   ↓
Original Repository
```

This is one of the most important concepts in open-source Git workflows.

---

# Clone Your Fork

After forking, clone your repository:

```bash
git clone <your-fork-url>
```

Enter the repository:

```bash
cd example
```

Check remotes:

```bash
git remote -v
```

You may see:

```text
origin  <your-fork-url> (fetch)
origin  <your-fork-url> (push)
```

---

# Add Upstream

Add the original repository:

```bash
git remote add upstream <original-repository-url>
```

Check again:

```bash
git remote -v
```

You should now have:

```text
origin
upstream
```

---

# Remote

A remote is a name that points to another Git repository.

Common remote names:

```text
origin
upstream
```

You can have more than two remotes, but these are the common names in fork-based workflows.

---

# Viewing Remotes

Use:

```bash
git remote -v
```

Example:

```text
origin    https://github.com/you/project.git (fetch)
origin    https://github.com/you/project.git (push)

upstream  https://github.com/project/project.git (fetch)
upstream  https://github.com/project/project.git (push)

```

---

# Fetch

Fetch downloads information about changes from a remote without automatically changing your current working files.

Example:

```bash
git fetch upstream
```

This updates your local knowledge of the upstream repository.

---

# Updating Main

Switch to your local main branch:

```bash
git switch main
```

Fetch upstream:

```bash
git fetch upstream
```

Merge upstream changes:

```bash
git merge upstream/main
```

Then push the updated main branch to your fork:

```bash
git push origin main
```

---

# Complete Sync Workflow

```text
Original Repository
        ↓
     upstream
        ↓
     git fetch
        ↓
   Local main
        ↓
      merge
        ↓
      origin
        ↓
    Your Fork
```

---

# Why Keep Your Fork Updated?

The original project may continue receiving changes:

```text
Original Repository

Commit A
Commit B
Commit C
Commit D
```

Your fork may still have:

```text
Commit A
Commit B
```

Keeping your fork synchronized reduces the chance of working from an outdated codebase.

---

# Create a Feature Branch

After updating main:

```bash
git switch main
git switch -c fix/example-bug
```

Now your work is isolated from main.

---

# Make Changes

Modify only the required files.

Check:

```bash
git status
```

Review your changes:

```bash
git diff
```

---

# Commit

Stage the required files:

```bash
git add .
```

Commit:

```bash
git commit -m "Fix example bug"
```

---

# Push to Your Fork

Push your branch to `origin`:

```bash
git push -u origin fix/example-bug
```

Remember:

```text
origin
 ↓
Your Fork
```

---

# Create Pull Request

Your branch now exists on your fork.

Create a Pull Request from:

```text
Your Fork
      ↓
Your Branch
      ↓
Original Repository
      ↓
main
```

The original project's maintainers can then review your contribution.

---

# Updating Your Branch

While your Pull Request is open, the original repository may receive new changes.

You can update your local main:

```bash
git switch main
git fetch upstream
git merge upstream/main
```

Then update your feature branch.

One approach:

```bash
git switch fix/example-bug
git merge main
```

Then push:

```bash
git push
```

Your existing Pull Request will update.

---

# Rebase Alternative

Another approach is rebasing your feature branch:

```bash
git switch main
git fetch upstream
git merge upstream/main

git switch fix/example-bug
git rebase main
```

If conflicts occur, resolve them and continue the rebase according to Git's instructions.

Then push the rewritten branch history:

```bash
git push --force-with-lease
```

Use force-push carefully.

---

# Merge vs Rebase

Merge:

```text
Feature
   ↓
Merge main
```

Usually preserves the existing branch history.

Rebase:

```text
Feature
   ↓
Replay commits on latest main
```

Creates a more linear history but rewrites commit history.

---

# Why `--force-with-lease`?

After rebasing, the remote branch history may no longer match your local history.

Git may require a force push.

Prefer:

```bash
git push --force-with-lease
```

over:

```bash
git push --force
```

`--force-with-lease` provides an additional safety check.

---

# Important Warning

Never blindly force-push a shared branch.

Especially avoid doing this to:

```text
main
master
Protected Branches
Someone else's branch
```

unless the project's workflow explicitly permits it.

---

# Remote Branches

A remote-tracking branch can look like:

```text
origin/main
upstream/main
```

These are references to branches known from the respective remotes.

Example:

```bash
git fetch upstream
```

may update:

```text
upstream/main
```

---

# Local vs Remote Branch

Local:

```text
main
```

Remote-tracking:

```text
origin/main
```

Upstream remote-tracking:

```text
upstream/main
```

These names represent different references.

---

# Visual Model

```text
              GitHub
                 │
        ┌────────┴────────┐
        ↓                 ↓
     upstream            origin
        ↓                 ↓
 Original Repo          Your Fork
        ↑                 ↑
        └──── Pull ───────┘
              Request
```

Your local repository sits between your computer and these remotes.

```text
Original Repo
      ↓
 upstream
      ↓
 Local Repository
      ↑
 origin
      ↑
 Your Fork
```

---

# Typical Commands

Clone:

```bash
git clone <your-fork-url>
```

Add upstream:

```bash
git remote add upstream <original-url>
```

Check remotes:

```bash
git remote -v
```

Fetch upstream:

```bash
git fetch upstream
```

Switch main:

```bash
git switch main
```

Update main:

```bash
git merge upstream/main
```

Push main:

```bash
git push origin main
```

Create branch:

```bash
git switch -c feature/example
```

Push branch:

```bash
git push -u origin feature/example
```

---

# Full Open Source Workflow

```text
Fork Original Repository
          ↓
Clone Your Fork
          ↓
Add upstream
          ↓
Fetch upstream
          ↓
Update main
          ↓
Create Branch
          ↓
Make Changes
          ↓
Test
          ↓
Commit
          ↓
Push to origin
          ↓
Create Pull Request
          ↓
Review
          ↓
Update if Required
          ↓
Merge
```

---

# Example

Original repository:

```text
github.com/example/project
```

Your fork:

```text
github.com/your-name/project
```

Clone:

```bash
git clone https://github.com/your-name/project.git
```

Enter:

```bash
cd project
```

Add upstream:

```bash
git remote add upstream https://github.com/example/project.git
```

Check:

```bash
git remote -v
```

Update:

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

Create branch:

```bash
git switch -c docs/update-readme
```

Make changes.

Commit:

```bash
git add README.md
git commit -m "Improve README documentation"
```

Push:

```bash
git push -u origin docs/update-readme
```

Create Pull Request.

---

# Common Mistakes

## Mistake 1

Confusing origin and upstream.

Remember:

```text
origin = your fork
upstream = original project
```

---

## Mistake 2

Working directly on main.

Prefer:

```bash
git switch -c feature/name
```

---

## Mistake 3

Never syncing your fork.

Update periodically:

```bash
git fetch upstream
```

---

## Mistake 4

Force-pushing carelessly.

Prefer:

```bash
git push --force-with-lease
```

when rewriting your own branch is actually necessary.

---

## Mistake 5

Creating a Pull Request from the wrong repository.

For a fork-based contribution:

```text
Your Fork
   ↓
Your Branch
   ↓
Original Repository
```

---

# Practice

Use a small repository for practice.

Set up:

```text
origin
upstream
```

Verify:

```bash
git remote -v
```

You should understand exactly which URL belongs to which remote.

Then:

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

Create:

```bash
git switch -c practice/fork-workflow
```

Make a documentation change.

Commit:

```bash
git add .
git commit -m "Practice fork workflow"
```

Push:

```bash
git push -u origin practice/fork-workflow
```

Create a Pull Request.

---

# Challenge

Draw this workflow in your notes:

```text
             ORIGINAL REPOSITORY
                     │
                  upstream
                     │
                     ↓
              LOCAL REPOSITORY
                     │
                  origin
                     │
                     ↓
                 YOUR FORK
                     │
                     ↓
                YOUR BRANCH
                     │
                     ↓
               PULL REQUEST
                     │
                     ↓
             ORIGINAL REPOSITORY
```

Then explain each part in your own words.

---

# Interview Questions

### What is a fork?

A GitHub copy of another repository under your account.

### What is origin?

The commonly used name for the remote representing your fork.

### What is upstream?

The commonly used name for the remote representing the original repository.

### What does `git fetch upstream` do?

It retrieves updated information from the upstream remote without automatically merging those changes into your current branch.

### Why use a feature branch?

To isolate your changes from the main branch.

### Why use `git push --force-with-lease` after a rebase?

Because rebasing rewrites commit history, and the remote branch may need to be updated to match the rewritten history. `--force-with-lease` adds a safety check.

---

# Summary

The key concept is:

```text
origin
 ↓
Your Fork

upstream
 ↓
Original Repository
```

And the contribution workflow is:

```text
Fork
 ↓
Clone
 ↓
Add upstream
 ↓
Fetch
 ↓
Sync main
 ↓
Create branch
 ↓
Make changes
 ↓
Commit
 ↓
Push to origin
 ↓
Pull Request to upstream
 ↓
Review
 ↓
Merge
```

Once you understand this workflow, you are ready to start making real open-source contributions.
