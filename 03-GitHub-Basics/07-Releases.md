# Releases

## Introduction

A GitHub Release is a packaged version of your project that is ready for users to download and use. Releases are commonly used to distribute software, applications, libraries, and updates.

Instead of downloading the latest source code, users can download a stable version of the project.

---

# What is a Release?

A Release is a specific version of your project based on a Git Tag.

It usually contains:

- Application files
- Source code
- Release notes
- Version number
- Changelog

---

# Why Use Releases?

Developers create Releases to:

- Publish stable versions
- Share downloadable files
- Track project versions
- Announce new features
- Fix bugs

---

# Version Numbering

Most projects follow Semantic Versioning.

Example:

```text
v1.0.0
v1.1.0
v1.2.0
v2.0.0
```

Meaning:

- Major Version → Breaking changes
- Minor Version → New features
- Patch Version → Bug fixes

---

# How to Create a Release

### Step 1

Open your GitHub repository.

### Step 2

Click **Releases**.

### Step 3

Click **Create a new release**.

### Step 4

Create a new Tag.

Example:

```text
v1.0.0
```

### Step 5

Add a Release Title.

Example:

```text
Version 1.0.0
```

### Step 6

Write Release Notes.

Example:

- Added Login Module
- Improved Performance
- Fixed Bugs

### Step 7

Click **Publish Release**.

---

# Real-World Example

Suppose you've completed version 1.0 of a Student Management System.

Instead of asking users to download the source code, you publish:

```text
Student-Management-System v1.0.0
```

Users can directly download the release from GitHub.

---

# Release vs Commit

| Release | Commit |
|----------|--------|
| Stable project version | Individual project snapshot |
| Public download | Development history |
| Uses Tags | No Tag required |
| Shared with users | Used by developers |

---

# Best Practices

- Follow Semantic Versioning.
- Write clear Release Notes.
- Publish only tested versions.
- Include major changes and bug fixes.

---

# Common Mistakes

- Publishing unfinished software.
- Skipping Release Notes.
- Using confusing version numbers.
- Releasing without testing.

---

# Interview Questions

### What is a GitHub Release?

A Release is a published version of a project that users can download.

---

### What is a Tag?

A Tag marks a specific commit as a version.

---

### Why are Releases important?

They provide stable versions of software for users.

---

### What is Semantic Versioning?

A versioning system using:

```text
Major.Minor.Patch
```

Example:

```text
v2.1.5
```

---

# Practice

1. Create a sample repository.
2. Make a few commits.
3. Create a Release.
4. Use the Tag:

```text
v1.0.0
```

5. Add Release Notes.
6. Publish the Release.

---

# Summary

GitHub Releases provide a professional way to distribute stable versions of software. They help developers manage versions, share updates with users, and maintain a clear project history.
