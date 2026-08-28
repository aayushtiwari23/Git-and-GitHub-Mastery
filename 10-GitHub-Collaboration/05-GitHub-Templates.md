
```

---

# Template Location

GitHub repository contribution templates are commonly stored under:

```text
.github/
```

For example:

```text
.github/
├── ISSUE_TEMPLATE/
└── PULL_REQUEST_TEMPLATE.md
```

---

# Issue Template Structure

Example:

```text
.github/
└── ISSUE_TEMPLATE/
    ├── bug_report.md
    └── feature_request.md
```

This gives contributors different Issue templates.

---

# Pull Request Template Structure

Example:

```text
.github/
└── PULL_REQUEST_TEMPLATE.md
```

The template can automatically appear when creating a Pull Request.

---

# YAML Issue Forms

GitHub also supports structured Issue Forms.

Instead of a simple Markdown document, an Issue Form can provide fields such as:

```text
Text Field
Textarea
Dropdown
Checkboxes
```

This creates a more structured Issue submission process.

---

# Issue Form Example

Conceptually:

```text
Bug Report

Description:
[________________]

Steps to Reproduce:
[________________]

Expected Behavior:
[________________]

Browser:
[ Dropdown ]

Confirmation:
☐ I searched existing issues
```

This makes required information easier to collect.

---

# Markdown Template vs Issue Form

Markdown:

```text
Simple
Flexible
Easy to create
```

Issue Form:

```text
Structured
Can provide required fields
Better for standardized reports
```

Choose based on the project's needs.

---

# Contribution Guidelines

Templates can work together with a contribution guide.

Important files may include:

```text
README.md
CONTRIBUTING.md
CODE_OF_CONDUCT.md
SECURITY.md
```

These files explain how people should interact with the project.

---

# CONTRIBUTING.md

A contribution guide can explain:

```text
How to fork
How to clone
How to create a branch
How to make changes
How to run tests
How to create a Pull Request
Coding standards
Commit conventions
```

Example workflow:

```text
Fork
 ↓
Clone
 ↓
Branch
 ↓
Code
 ↓
Test
 ↓
Commit
 ↓
Push
 ↓
Pull Request
```

---

# Code of Conduct

A Code of Conduct defines expected community behavior.

It can establish expectations such as:

```text
Respectful Communication
Professional Behavior
No Harassment
Constructive Feedback
```

This is particularly important for public open-source projects.

---

# Security Policy

A security policy explains how security vulnerabilities should be reported.

Instead of publishing sensitive vulnerability details publicly, contributors can follow the project's security-reporting process.

A common file is:

```text
SECURITY.md
```

---

# Template + Issue Workflow

With templates:

```text
Contributor
     ↓
New Issue
     ↓
Select Template
     ↓
Fill Required Information
     ↓
Submit
     ↓
Maintainer Reviews
```

---

# Template + Pull Request Workflow

```text
Developer
    ↓
Pull Request
    ↓
Template Appears
    ↓
Complete Description
    ↓
CI
    ↓
Review
    ↓
Merge
```

---

# Professional Repository Structure

A mature repository may look like:

```text
repository/
│
├── README.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
│
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   │
│   └── PULL_REQUEST_TEMPLATE.md
│
└── src/
```

Not every project needs every file.

---

# Practice

Add these files to your repository:

```text
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.md
│   └── feature_request.md
│
└── PULL_REQUEST_TEMPLATE.md
```

---

# Bug Template

Create:

```text
.github/ISSUE_TEMPLATE/bug_report.md
```

Paste:

```md
---
name: Bug Report
about: Report a problem
title: "[Bug] "
labels: bug
assignees: ""
---

## Description

Describe the bug clearly.

## Steps to Reproduce

1.
2.
3.

## Expected Behavior

What should happen?

## Actual Behavior

What actually happens?

## Environment

- OS:
- Browser:
- Version:

## Additional Information

Add screenshots, logs, or other useful information.
```

---

# Feature Template

Create:

```text
.github/ISSUE_TEMPLATE/feature_request.md
```

Paste:

```md
---
name: Feature Request
about: Suggest an improvement
title: "[Feature] "
labels: enhancement
assignees: ""
---

## Feature

Describe the feature.

## Problem

What problem does it solve?

## Proposed Solution

Describe your proposed solution.

## Alternatives

Describe alternatives you considered.

## Additional Information

Add any other relevant information.
```

---

# Pull Request Template

Create:

```text
.github/PULL_REQUEST_TEMPLATE.md
```

Paste:

```md
## Summary

Describe the changes made in this Pull Request.

## Changes

- 
- 
- 

## Testing

Describe how the changes were tested.

## Checklist

- [ ] Tests pass
- [ ] Documentation updated
- [ ] No unnecessary files added
- [ ] No secrets committed
- [ ] Changes are ready for review

## Related Issue

Fixes #
```

---

# Test Your Templates

After committing the files:

```text
Repository
   ↓
Issues
   ↓
New Issue
```

You should be able to see the available templates.

Try:

```text
Bug Report
```

Then:

```text
Feature Request
```

---

# Test Pull Request Template

Create a test branch:

```bash
git switch -c test/pr-template
```

Make a small documentation change.

Commit:

```bash
git add .
git commit -m "Test pull request template"
```

Push:

```bash
git push -u origin test/pr-template
```

Create a Pull Request.

The Pull Request description should contain your template.

---

# Template Best Practices

- Keep templates short.
- Ask only for useful information.
- Avoid unnecessary questions.
- Use clear headings.
- Make important fields obvious.
- Keep templates updated.
- Don't make contributors fill unnecessary information.

---

# Common Mistakes

Avoid templates that look like:

```text
20 questions
10 required sections
Huge instructions
Unnecessary checkboxes
```

The goal is:

```text
Useful Structure
      ↓
Not Bureaucracy
```

---

# Real-World Collaboration System

A professional repository can combine:

```text
README
   ↓
Contribution Guide
   ↓
Issue Templates
   ↓
Issues
   ↓
Project
   ↓
Branch
   ↓
Pull Request Template
   ↓
CI
   ↓
Code Review
   ↓
Merge
```

---

# Interview Questions

### What is an Issue Template?

A predefined format that helps contributors provide consistent information when creating Issues.

### What is a Pull Request Template?

A predefined description format that helps contributors explain their Pull Request consistently.

### What is an Issue Form?

A structured GitHub Issue submission interface containing fields such as text inputs, dropdowns, and checkboxes.

### Why use templates?

They improve consistency, reduce missing information, and make collaboration easier.

### What is `CONTRIBUTING.md`?

A document explaining how people should contribute to a repository.

### What is `CODE_OF_CONDUCT.md`?

A document describing expected behavior within a project's community.

### What is `SECURITY.md`?

A document explaining how security vulnerabilities should be reported.

---

# Challenge

Make your GitHub Mastery repository contributor-ready.

Your repository should contain:

```text
README.md
CONTRIBUTING.md
.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.md
│   └── feature_request.md
└── PULL_REQUEST_TEMPLATE.md
```

Then test:

```text
[ ] Create Bug Issue
[ ] Create Feature Issue
[ ] Create Test Pull Request
[ ] Verify templates appear
[ ] Close test Issue/PR if no longer needed
```

---

# Summary

Templates make GitHub collaboration more structured.

Remember:

```text
Issue Template
     ↓
Consistent Issue

PR Template
     ↓
Consistent Pull Request

CONTRIBUTING.md
     ↓
Contribution Instructions

CODE_OF_CONDUCT.md
     ↓
Community Expectations

SECURITY.md
     ↓
Security Reporting
```

A good repository doesn't only contain code.

It also explains **how people should work with that code**.
