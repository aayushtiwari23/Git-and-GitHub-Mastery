# What is Version Control?

## Definition

Version Control is a system that records changes made to files over time. It allows developers to track modifications, restore previous versions, compare changes, and collaborate efficiently.

---

## Why is Version Control Important?

Without Version Control:

- Files get duplicated.
- Mistakes are difficult to recover.
- Team collaboration becomes confusing.
- Project history is lost.

With Version Control:

- Every change is recorded.
- Previous versions can be restored.
- Multiple developers can work together.
- Project history is always available.

---

## Example Without Version Control

```
Project
Project_Final
Project_Final_New
Project_Final_Latest
Project_Final_Final
```

Finding the correct version becomes confusing.

---

## Example With Version Control

```
Project

Commit 1
     │
Commit 2
     │
Commit 3
     │
Commit 4
```

Every version is saved with a meaningful commit message.

---

## Types of Version Control

### Local Version Control

Stores versions only on a single computer.

### Centralized Version Control (CVCS)

A central server stores the project.

Examples:

- SVN
- CVS

### Distributed Version Control (DVCS)

Every developer has a complete copy of the project and its history.

Examples:

- Git
- Mercurial

---

## Advantages

- Tracks every change
- Restores previous versions
- Improves collaboration
- Reduces development mistakes
- Maintains complete project history
- Supports parallel development

---

## Real-World Example

A team is building an E-Commerce website.

Day 1

```
Home Page
```

Day 5

```
Home Page
Product Page
```

Day 10

```
Payment Module
```

If the payment module introduces a bug, Version Control allows the team to return to the last stable version without losing earlier work.

---

## Key Terms

| Term | Meaning |
|------|---------|
| Version | A saved state of a project |
| Repository | Storage for project files and history |
| Commit | A recorded snapshot of changes |
| History | Record of all commits |

---

## Summary

Version Control is an essential part of software development. It helps developers manage project history, recover from mistakes, and collaborate efficiently while maintaining a complete record of every change.
