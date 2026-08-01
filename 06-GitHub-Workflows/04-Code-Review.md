
# Code Review

## Introduction

A Code Review is the process of examining another developer's code before it is merged into the main branch. Its purpose is to improve code quality, identify bugs, ensure coding standards are followed, and share knowledge among team members.

Code reviews are a standard practice in professional software development and are required in most software companies.

---

# What is Code Review?

A Code Review is a systematic inspection of source code by one or more developers.

The reviewer checks:

- Code quality
- Correctness
- Readability
- Performance
- Security
- Maintainability
- Coding standards

Only after approval is the code merged into the project.

---

# Why is Code Review Important?

Code reviews help teams:

- Find bugs early.
- Improve code quality.
- Maintain coding standards.
- Share knowledge.
- Reduce technical debt.
- Improve software security.

---

# Typical Code Review Workflow

```text
Developer
    │
Write Code
    │
Commit Changes
    │
Push Branch
    │
Open Pull Request
    │
Reviewer Reviews Code
    │
Request Changes / Approve
    │
Developer Updates Code
    │
Approval
    │
Merge
```

---

# What Reviewers Check

## 1. Correctness

Does the code solve the problem correctly?

Example:

- Correct logic
- Expected output
- Proper validation

---

## 2. Readability

Is the code easy to understand?

Good example:

```python
total_price = quantity * price
```

Poor example:

```python
x = a * b
```

---

## 3. Code Style

Check whether the project follows:

- Naming conventions
- Formatting
- Project guidelines

---

## 4. Performance

Look for:

- Unnecessary loops
- Duplicate calculations
- Slow algorithms
- High memory usage

---

## 5. Security

Check for:

- SQL Injection
- Hardcoded passwords
- Sensitive data exposure
- Unsafe input handling

---

## 6. Maintainability

Good code should:

- Be modular.
- Be reusable.
- Be easy to modify.

---

## 7. Documentation

Reviewers check:

- Comments
- README updates
- API documentation
- Function descriptions

---

# Real-World Example

A developer creates a Login Feature.

Reviewer checks:

- Input validation
- Password encryption
- Error handling
- Code formatting
- Unit tests

After suggestions are addressed, the Pull Request is approved.

---

# Reviewer Checklist

Before approving:

- Code works correctly.
- No obvious bugs.
- Naming is clear.
- No duplicate code.
- Tests pass.
- Documentation is updated.
- Security issues are addressed.

---

# Best Practices for Authors

- Keep Pull Requests small.
- Write descriptive commit messages.
- Explain the purpose of the changes.
- Respond politely to review comments.
- Test before requesting a review.

---

# Best Practices for Reviewers

- Be respectful.
- Give constructive feedback.
- Explain why improvements are needed.
- Focus on the code, not the developer.
- Approve only after all issues are resolved.

---

# Common Mistakes

- Reviewing too quickly.
- Ignoring edge cases.
- Approving untested code.
- Making personal comments.
- Creating very large Pull Requests.

---

# Interview Questions

### What is Code Review?

Code Review is the process of examining source code before it is merged into the main branch to improve quality and detect issues.

---

### Why are Code Reviews important?

They improve software quality, reduce bugs, maintain coding standards, and encourage knowledge sharing.

---

### Who performs a Code Review?

Usually another developer, team lead, or project maintainer.

---

### What should reviewers focus on?

- Correctness
- Readability
- Performance
- Security
- Maintainability
- Coding standards

---

# Practice

1. Create a feature branch.
2. Make a small change.
3. Open a Pull Request.
4. Ask a teammate to review it.
5. Address the feedback.
6. Request another review.
7. Merge after approval.

---

# Summary

Code Reviews are an essential part of professional software development. They help teams improve code quality, identify bugs early, enforce coding standards, and build reliable software through collaborative feedback.
