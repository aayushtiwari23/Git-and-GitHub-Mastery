
# Pull Request Review

## Introduction

A Pull Request (PR) Review is the process of evaluating code submitted through a Pull Request before it is merged into the main branch. It ensures that new code is correct, follows project standards, and does not introduce bugs or security issues.

Professional software teams rely on Pull Request Reviews to maintain high-quality code and encourage collaboration.

---

# What is a Pull Request Review?

A Pull Request Review is the examination of proposed code changes by one or more reviewers.

Reviewers can:

- Approve the Pull Request
- Request changes
- Leave comments
- Suggest improvements

Only after approval is the Pull Request merged.

---

# Why are Pull Request Reviews Important?

They help teams:

- Improve code quality
- Catch bugs early
- Maintain coding standards
- Improve security
- Share knowledge
- Reduce technical debt

---

# Pull Request Review Workflow

```text
Developer
     │
Create Feature Branch
     │
Write Code
     │
Commit Changes
     │
Push Branch
     │
Open Pull Request
     │
Reviewer Reviews
     │
────────────────────────────
│ Approve                 │
│ Request Changes         │
│ Leave Comments          │
────────────────────────────
     │
Developer Updates Code
     │
Review Again
     │
Merge
```

---

# Types of Review Decisions

## 1. Comment

The reviewer leaves feedback without approving or rejecting the Pull Request.

Example:

```text
Can you improve this variable name?
```

---

## 2. Approve

The reviewer is satisfied.

The Pull Request is ready to merge.

---

## 3. Request Changes

The reviewer finds issues that must be fixed before merging.

Example:

- Missing validation
- Poor naming
- Failing tests
- Security issue

---

# What Should Reviewers Check?

## Code Correctness

- Does it solve the problem?
- Are edge cases handled?

---

## Readability

- Clear variable names
- Easy-to-read logic
- Consistent formatting

---

## Performance

- Efficient algorithms
- No unnecessary loops
- Minimal memory usage

---

## Security

Check for:

- SQL Injection
- XSS
- Hardcoded secrets
- Unsafe input handling

---

## Testing

Verify:

- Existing tests pass
- New tests added if required

---

## Documentation

Ensure:

- README updated
- Comments are accurate
- API documentation is complete

---

# GitHub Review Options

On GitHub, reviewers can choose:

```text
Comment

Approve

Request Changes
```

Each option becomes part of the Pull Request history.

---

# Merge Options

After approval, GitHub provides several merge methods.

## Create a Merge Commit

Preserves all commits and creates a merge commit.

```
main
 │
 ├── feature
 │
 └── Merge Commit
```

Best for preserving history.

---

## Squash and Merge

Combines all commits into one.

Example:

Before:

```text
Fix typo

Update README

Correct heading

Improve formatting
```

After:

```text
Improve README documentation
```

Produces a cleaner history.

---

## Rebase and Merge

Replays commits onto the latest branch without creating a merge commit.

Results in a linear project history.

---

# Real-World Example

A developer submits a Pull Request for a Payment Module.

Reviewer notices:

- Missing error handling
- Poor variable names
- No unit tests

The reviewer selects:

```text
Request Changes
```

Developer fixes the issues.

Reviewer approves.

The Pull Request is merged.

---

# Review Etiquette

For Authors:

- Keep Pull Requests small.
- Explain the purpose clearly.
- Respond politely.
- Address every review comment.
- Thank reviewers for their feedback.

For Reviewers:

- Be respectful.
- Explain suggestions clearly.
- Focus on improving the code.
- Avoid personal criticism.
- Approve only when satisfied.

---

# Best Practices

- Review code regularly.
- Keep Pull Requests focused on one feature.
- Use meaningful commit messages.
- Run tests before requesting a review.
- Resolve all review comments before merging.

---

# Common Mistakes

- Opening very large Pull Requests.
- Ignoring review comments.
- Merging without approval.
- Reviewing only formatting instead of functionality.
- Not testing changes before review.

---

# Interview Questions

### What is a Pull Request Review?

A Pull Request Review is the process of evaluating code before it is merged into the main branch.

---

### What review options does GitHub provide?

- Comment
- Approve
- Request Changes

---

### What is the purpose of requesting changes?

To ensure identified issues are fixed before merging.

---

### Which merge option creates a single commit?

Squash and Merge.

---

### Which merge option keeps a linear history?

Rebase and Merge.

---

# Practice

1. Create a feature branch.
2. Make a small change.
3. Push the branch.
4. Open a Pull Request.
5. Review the Pull Request yourself.
6. Add comments.
7. Approve or request changes.
8. Merge the Pull Request.

---

# Summary

Pull Request Reviews are a critical part of modern software development. They improve code quality, encourage collaboration, identify bugs early, and ensure that only well-tested, maintainable code is merged into the main branch.
