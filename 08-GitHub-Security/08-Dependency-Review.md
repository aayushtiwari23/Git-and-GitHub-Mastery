# GitHub Dependency Review

## Introduction

Dependency Review helps developers identify potentially risky dependency changes in Pull Requests before those changes are merged.

When a Pull Request changes project dependencies, Dependency Review can show what was added, removed, or updated and whether the changes introduce known security vulnerabilities.

---

# Why Dependency Review Matters

Modern applications depend on many external packages.

Example:

```text
Your Application
      │
      ├── React
      ├── Express
      ├── Axios
      ├── Database Driver
      └── Other Packages
```

Adding a new dependency can introduce:

- Security vulnerabilities
- License concerns
- Outdated packages
- Unwanted transitive dependencies

Dependency Review provides another security check before merging.

---

# How Dependency Review Works

```text
Pull Request
      │
      ▼
Dependency Changes Detected
      │
      ▼
Dependency Review
      │
      ├── Safe
      │
      └── Risk Found
              │
              ▼
        Review / Fix
              │
              ▼
            Merge
```

---

# Example

Suppose your project currently uses:

```text
library-a 1.0.0
library-b 2.0.0
```

A Pull Request adds:

```text
library-c 3.0.0
```

Dependency Review examines the dependency change.

If `library-c` has a known vulnerability, the Pull Request can receive a security warning or fail a configured check.

---

# Dependency Review vs Dependabot

These features are related but serve different purposes.

| Dependency Review | Dependabot |
|--------------------|------------|
| Examines dependency changes in Pull Requests | Monitors dependencies |
| Helps assess new dependency changes | Finds outdated/vulnerable dependencies |
| Can block risky dependency changes through checks | Can create update Pull Requests |
| Focuses on Pull Request changes | Focuses on dependency maintenance |

They work well together.

---

# Dependency Review Action

GitHub provides a Dependency Review Action that can be used in GitHub Actions workflows.

A workflow can look like:

```yaml
name: Dependency Review

on:
  pull_request:

permissions:
  contents: read

jobs:
  dependency-review:
    runs-on: ubuntu-latest

    steps:
      - name: Dependency Review
        uses: actions/dependency-review-action@v4
```

This checks dependency changes introduced by Pull Requests.

---

# Pull Request Workflow

```text
Developer
    │
    ▼
Modify Dependencies
    │
    ▼
Push Changes
    │
    ▼
Pull Request
    │
    ▼
Dependency Review
    │
    ├── No Problem
    │      │
    │      ▼
    │    Continue
    │
    └── Risk Detected
           │
           ▼
      Investigate
           │
           ▼
       Fix / Remove
           │
           ▼
        Review Again
```

---

# Transitive Dependencies

A project may directly depend on one package while that package depends on others.

Example:

```text
Application
    │
    ▼
Package A
    │
    ├── Package B
    │      │
    │      └── Package C
    │
    └── Package D
```

Packages B, C, and D are dependencies of the application indirectly.

These are called **transitive dependencies**.

Dependency security should consider them as well.

---

# Why Transitive Dependencies Matter

You may not have intentionally installed a vulnerable package.

For example:

```text
Your Project
     │
     ▼
Package A
     │
     ▼
Package B
     │
     ▼
Vulnerable Version
```

The vulnerability can still affect your application.

---

# License Considerations

Dependency Review can also help identify dependency license changes.

Software licenses define how code can legally be used, modified, and distributed.

Examples include:

```text
MIT
Apache-2.0
GPL
BSD
```

Organizations may have policies restricting certain licenses.

Always check the actual license requirements before using third-party software in a real project.

---

# Security Gates

Dependency Review can be used as a security gate.

Conceptually:

```text
Pull Request
      │
      ▼
Dependency Review
      │
 ┌────┴─────┐
 │          │
Pass       Fail
 │          │
 ▼          ▼
Merge     Fix Issue
```

This prevents certain dependency problems from automatically moving forward.

---

# Best Practices

- Review dependency changes carefully.
- Use Dependency Review on Pull Requests.
- Keep dependencies updated.
- Combine Dependency Review with Dependabot.
- Check transitive dependencies.
- Review security alerts.
- Consider organizational license policies.

---

# Common Mistakes

- Assuming only direct dependencies matter.
- Merging dependency changes without reviewing them.
- Ignoring vulnerability warnings.
- Automatically accepting every dependency update.
- Using third-party packages without checking their security and licensing requirements.

---

# Dependency Security Strategy

A strong dependency-management strategy can look like:

```text
                 Dependencies
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
   Dependabot   Dependency Review  Manual Review
        │             │             │
        └─────────────┼─────────────┘
                      ▼
               Security Checks
                      │
                      ▼
                   Testing
                      │
                      ▼
                    Merge
```

---

# Interview Questions

### What is Dependency Review?

Dependency Review is a GitHub security feature that examines dependency changes introduced by Pull Requests.

---

### Why is Dependency Review useful?

It helps identify potentially vulnerable or problematic dependencies before they are merged into a project.

---

### What is a transitive dependency?

A transitive dependency is a dependency required indirectly through another dependency.

---

### What is the difference between Dependency Review and Dependabot?

Dependency Review examines dependency changes in Pull Requests, while Dependabot helps monitor dependencies and create update Pull Requests.

---

### Can Dependency Review be used in GitHub Actions?

Yes. GitHub provides the Dependency Review Action for use in workflows.

---

# Practice

1. Create:

```text
.github/workflows/dependency-review.yml
```

2. Add:

```yaml
name: Dependency Review

on:
  pull_request:

permissions:
  contents: read

jobs:
  dependency-review:
    runs-on: ubuntu-latest

    steps:
      - name: Dependency Review
        uses: actions/dependency-review-action@v4
```

3. Commit:

```bash
git add .github/workflows/dependency-review.yml
```

4. Commit:

```bash
git commit -m "Add dependency review workflow"
```

5. Push:

```bash
git push
```

6. Create a Pull Request that changes a dependency.

7. Observe the Dependency Review check.

Do not intentionally add a known vulnerable dependency to a public project just to test the security system.

---

# Summary

Dependency Review helps developers evaluate dependency changes before they enter the main codebase.

It works especially well alongside:

```text
Dependabot
+
Secret Scanning
+
Code Scanning
+
Pull Request Reviews
+
Automated Tests
```

The goal is to catch dependency-related security problems as early as possible.
