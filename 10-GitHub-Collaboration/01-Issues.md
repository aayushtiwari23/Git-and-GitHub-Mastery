
Example:

```text
Milestone: Version 1.0

Issues:
#1 Login
#2 Dashboard
#3 User Profile
#4 Testing
```

Conceptually:

```text
Milestone
   ↓
Multiple Issues
   ↓
Project Goal
```

---

# Assignees

An Issue can be assigned to someone responsible for completing it.

Example:

```text
Issue #15
Add API documentation

Assigned to:
Developer
```

This makes responsibility clear.

---

# Issue Comments

Comments can be used for:

```text
Questions
Progress Updates
Technical Discussion
Testing Results
Decisions
```

Example:

```text
Update:
The bug has been reproduced.
I'm working on a fix.
```

---

# Issue Lifecycle

A typical lifecycle:

```text
Open
 ↓
Assigned
 ↓
In Progress
 ↓
Pull Request
 ↓
Review
 ↓
Completed
 ↓
Closed
```

---

# Linking an Issue to a Pull Request

A Pull Request can reference an Issue.

For example:

```text
Fixes #15
```

When the Pull Request is merged, GitHub can automatically close the referenced Issue.

Other supported closing keywords include:

```text
Fixes
Closes
Resolves
```

Example:

```text
Fixes #15
```

---

# Example Development Flow

Issue:

```text
#15 Fix login validation
```

Developer creates a branch:

```text
fix/login-validation
```

Then makes changes.

After that:

```text
Branch
  ↓
Commit
  ↓
Push
  ↓
Pull Request
  ↓
Fixes #15
```

After the Pull Request is merged:

```text
Issue #15
     ↓
Closed
```

---

# Good Issue Writing

A good Issue should be:

```text
Clear
Specific
Actionable
Reproducible
```

Avoid vague descriptions.

Bad:

```text
Website broken
```

Better:

```text
Login button does not submit the form when email contains uppercase characters
```

---

# Bug Report Template

A useful bug report can follow:

```md
## Description

Describe the problem.

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

Add screenshots, logs, or other relevant information.
```

---

# Feature Request Template

Example:

```md
## Feature

Describe the feature.

## Problem

What problem does this solve?

## Proposed Solution

Describe the expected behavior.

## Alternatives

What alternatives were considered?

## Additional Information

Add relevant details.
```

---

# Issues vs Projects

Issues track individual work items.

Projects organize larger collections of work.

```text
Issue
 ↓
Individual Task

Project
 ↓
Collection of Tasks
```

---

# Issues vs Discussions

GitHub Discussions are generally better for:

```text
Questions
Ideas
Community Discussions
Open-ended Conversations
```

Issues are generally better for:

```text
Bugs
Features
Tasks
Actionable Work
```

---

# Issue Number

Every Issue receives a number.

Example:

```text
#1
#2
#3
```

You can reference an Issue using:

```text
#3
```

---

# Mentioning Issues

Inside comments, Pull Requests, and other supported GitHub text fields:

```text
#25
```

references Issue 25 in the repository context.

---

# Cross-Repository References

GitHub can also reference Issues from another repository using an appropriate repository reference.

Conceptually:

```text
owner/repository#123
```

This is useful when multiple repositories are related.

---

# Issue Search

GitHub provides search filters for Issues.

You can search by:

```text
State
Label
Assignee
Milestone
Author
Text
```

For example:

```text
is:open
```

finds open Issues.

---

# Open vs Closed

Open:

```text
Work remains
```

Closed:

```text
Work completed
```

Do not close an Issue simply because someone started working on it.

---

# Issue Best Practices

- Use descriptive titles.
- Add enough context.
- Use labels consistently.
- Assign responsibility.
- Link related Pull Requests.
- Close completed work.
- Avoid duplicate Issues.
- Keep discussions focused.

---

# Practice

Create three Issues in your GitHub repository.

### Issue 1

Title:

```text
Add project documentation
```

Label:

```text
documentation
```

Description:

```md
## Task

Improve the repository documentation.

## Requirements

- Add project overview
- Add setup instructions
- Add usage instructions
- Add contribution instructions
```

---

### Issue 2

Title:

```text
Add automated testing
```

Description:

```md
## Task

Add automated tests to the project.

## Requirements

- Add test framework
- Add basic tests
- Run tests through GitHub Actions
```

---

### Issue 3

Title:

```text
Improve GitHub Actions workflow
```

Description:

```md
## Task

Improve the existing GitHub Actions workflow.

## Requirements

- Review workflow structure
- Improve CI process
- Check permissions
- Improve documentation
```

---

# Challenge

Take Issue #1 and actually work on it.

Create a branch:

```text
docs/project-documentation
```

Make a change.

Commit it:

```text
Improve project documentation
```

Push the branch.

Create a Pull Request.

In the Pull Request description write:

```text
Fixes #1
```

Then:

```text
Issue
 ↓
Branch
 ↓
Commit
 ↓
Pull Request
 ↓
Review
 ↓
Merge
 ↓
Issue Closed
```

This is the real GitHub collaboration workflow.

---

# Summary

GitHub Issues provide a structured way to manage work.

Remember:

```text
Issue
 ↓
Task
 ↓
Branch
 ↓
Commit
 ↓
Pull Request
 ↓
Review
 ↓
Merge
 ↓
Close Issue
```

This workflow is one of the most important GitHub skills to understand before moving into serious open-source contribution.
