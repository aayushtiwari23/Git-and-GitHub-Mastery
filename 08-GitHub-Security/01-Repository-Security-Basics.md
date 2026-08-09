
# GitHub Repository Security Basics

## Introduction

GitHub security features help protect repositories, source code, credentials, dependencies, and development workflows from security threats.

Security should be considered from the beginning of a project, not after a vulnerability is discovered.

---

# Why Repository Security Matters

A repository may contain:

- Source code
- API keys
- Passwords
- Access tokens
- Database credentials
- Personal information
- Third-party dependencies

If sensitive information is exposed, attackers may be able to access systems or data.

---

# Important GitHub Security Features

GitHub provides several security features, including:

- Secret scanning
- Dependabot
- Code scanning
- Security advisories
- Dependency review
- Branch protection
- Security policies
- Private repositories

---

# 1. Keep Sensitive Information Out of Git

Never commit:

```text
API keys
Passwords
Access tokens
Private keys
Database credentials
.env files
```

Bad:

```text
API_KEY=123456789
PASSWORD=myPassword
```

These values should not be stored directly in source code.

---

# 2. Use Environment Variables

Instead of hardcoding credentials:

```text
API_KEY=your-secret-key
```

Store the value as an environment variable.

Your application can then read the variable at runtime.

Example:

```python
import os

api_key = os.getenv("API_KEY")
```

---

# 3. Use `.gitignore`

Create:

```text
.gitignore
```

Example:

```text
.env
*.key
*.pem
secrets/
node_modules/
__pycache__/
```

This prevents selected files from being accidentally committed.

---

# 4. GitHub Secrets

For GitHub Actions, sensitive values should be stored using GitHub Secrets.

Examples:

```text
API_KEY
DATABASE_PASSWORD
DEPLOYMENT_TOKEN
```

Workflow files can reference secrets without exposing their values.

Example:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

---

# 5. Secret Scanning

GitHub Secret Scanning helps detect credentials and other sensitive information that may have been committed to a repository.

It can help identify exposed:

- API tokens
- Authentication credentials
- Cloud credentials
- Private keys

If a secret is exposed, revoke or rotate it immediately.

---

# 6. Dependabot

Modern applications depend on external packages and libraries.

These dependencies can contain vulnerabilities.

Dependabot can:

- Detect vulnerable dependencies.
- Notify maintainers.
- Create pull requests for dependency updates.

Example:

```text
Project
 │
 ├── Dependency A
 ├── Dependency B
 └── Dependency C
          │
          ▼
     Vulnerability Found
          │
          ▼
      Dependabot
          │
          ▼
   Update Pull Request
```

---

# 7. Code Scanning

Code scanning analyzes source code to identify potential security problems.

It can detect issues such as:

- Vulnerable coding patterns
- Injection vulnerabilities
- Unsafe operations
- Security bugs

GitHub CodeQL is one technology used for code scanning.

---

# 8. Dependency Review

Dependency Review checks changes to dependencies in Pull Requests.

It can help identify whether a new dependency introduces known security vulnerabilities.

---

# 9. Security Policy

Repositories can contain:

```text
SECURITY.md
```

This file explains how security researchers and users should report vulnerabilities.

Example:

```md
# Security Policy

If you discover a security vulnerability,
please report it privately to the project maintainers.

Do not publicly disclose the vulnerability
before it has been investigated.
```

---

# 10. Branch Protection

Important branches such as:

```text
main
production
release
```

should be protected.

Possible protections include:

- Require Pull Requests.
- Require reviews.
- Require status checks.
- Prevent force pushes.
- Prevent branch deletion.

---

# 11. Private vs Public Repositories

## Public Repository

Anyone can view the repository.

Useful for:

- Open-source projects
- Portfolio projects
- Documentation
- Learning repositories

---

## Private Repository

Only authorized users can access it.

Useful for:

- Private projects
- Company code
- Proprietary applications
- Sensitive development

---

# Security Checklist

Before pushing code:

- [ ] No passwords included.
- [ ] No API keys included.
- [ ] `.gitignore` configured.
- [ ] Dependencies checked.
- [ ] Sensitive files excluded.
- [ ] GitHub security features enabled where appropriate.

---

# If You Accidentally Commit a Secret

Do **not** assume deleting the file fixes the problem.

A secret may still exist in Git history.

Immediately:

1. Revoke the exposed credential.
2. Generate a replacement credential.
3. Remove the secret from the repository.
4. Check Git history.
5. Investigate whether the credential was accessed.

The most important step is **revoking or rotating the secret**.

---

# Secure GitHub Workflow

```text
Write Code
    │
    ▼
Check for Secrets
    │
    ▼
Commit
    │
    ▼
Push
    │
    ▼
Pull Request
    │
    ├── Code Scanning
    ├── Dependency Review
    └── Secret Scanning
    │
    ▼
Code Review
    │
    ▼
Merge
```

---

# Best Practices

- Never hardcode secrets.
- Use `.gitignore`.
- Use GitHub Secrets for CI/CD credentials.
- Keep dependencies updated.
- Enable appropriate security scanning.
- Protect important branches.
- Review Pull Requests carefully.
- Rotate exposed credentials immediately.

---

# Interview Questions

### Why should API keys not be stored in GitHub repositories?

Because anyone with access to the repository or its history may obtain the credentials and potentially misuse them.

---

### What is Dependabot?

Dependabot is a GitHub feature that helps identify outdated or vulnerable dependencies and can create update Pull Requests.

---

### What is Secret Scanning?

Secret Scanning helps detect credentials and sensitive tokens exposed in repositories.

---

### What is `.gitignore`?

`.gitignore` specifies files and directories that Git should not track.

---

### What is `SECURITY.md`?

`SECURITY.md` provides instructions for responsibly reporting security vulnerabilities in a project.

---

# Practice

1. Create a `.gitignore` file.
2. Add:

```text
.env
secrets/
*.key
```

3. Create a `SECURITY.md` file.
4. Open your repository's **Security** section.
5. Explore available security features.
6. Never upload a real API key for testing.

---

# Summary

GitHub repository security protects source code, credentials, dependencies, and development workflows. The most important habits are simple: never commit secrets, use `.gitignore` and GitHub Secrets, keep dependencies updated, protect important branches, and use security scanning where appropriate.
