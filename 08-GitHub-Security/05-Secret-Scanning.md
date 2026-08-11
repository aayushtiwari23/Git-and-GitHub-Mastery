# GitHub Secret Scanning

## Introduction

GitHub Secret Scanning helps detect sensitive credentials accidentally exposed in repositories.

Examples include:

- API keys
- Access tokens
- Cloud credentials
- Private keys
- Authentication tokens

Secret Scanning helps developers discover exposed credentials before they are misused.

---

# What is Secret Scanning?

Secret Scanning searches repositories for patterns that resemble known secrets.

Example:

```text
Source Code
     │
     ▼
Secret Scanning
     │
     ▼
Potential Secret Detected
     │
     ▼
Security Alert
     │
     ▼
Revoke / Rotate Secret
```

---

# Why Secret Scanning Matters

Suppose a developer accidentally commits:

```text
API_KEY=real-secret-value
```

If the repository is public, anyone may be able to see it.

An attacker could potentially use the credential to access a service.

Therefore:

```text
Secret Exposed
      ↓
Credential Revoked
      ↓
New Credential Created
      ↓
GitHub Secret Updated
```

---

# What Can Be Detected?

Depending on GitHub's supported secret patterns and configuration, Secret Scanning can detect credentials such as:

```text
API Tokens
Cloud Credentials
OAuth Tokens
Private Keys
Database Credentials
Service Tokens
```

GitHub maintains patterns for many supported providers.

---

# Secret Scanning vs `.gitignore`

These tools solve different problems.

| `.gitignore` | Secret Scanning |
|--------------|-----------------|
| Prevents selected files from being tracked | Detects potential secrets |
| Works locally with Git | Works through GitHub security features |
| Configured manually | Uses detection patterns |
| Preventive | Detection-focused |

Example `.gitignore`:

```text
.env
secrets/
*.key
```

Even with `.gitignore`, developers should still avoid placing secrets directly into source code.

---

# Secret Scanning Alerts

When GitHub detects a potential secret, it can generate a security alert.

The alert can help identify:

```text
What was detected
Where it was detected
When it was detected
What provider or secret type it resembles
```

The exact information depends on the detected secret and repository configuration.

---

# Push Protection

GitHub can also provide **push protection** for supported secrets.

Instead of waiting until a secret reaches the repository, push protection can warn or block a push when a potential secret is detected.

Conceptually:

```text
Developer
    │
    ▼
git push
    │
    ▼
Secret Detection
    │
 ┌──┴──────────┐
 │             │
No Secret   Secret Found
 │             │
 ▼             ▼
Push        Warning / Block
```

This helps prevent secrets from entering the repository in the first place.

---

# What To Do If a Secret Is Detected

If a real credential is exposed:

### Step 1: Revoke the Secret

Immediately disable the exposed credential.

### Step 2: Rotate the Credential

Generate a new credential.

### Step 3: Update Applications

Replace the old credential wherever it is being used.

### Step 4: Update GitHub Secrets

If the credential is used in GitHub Actions, update the corresponding GitHub Secret.

### Step 5: Investigate

Determine:

- Where it was exposed.
- How long it was exposed.
- Whether it was accessed.
- Whether other credentials were affected.

---

# Important Warning

Deleting the secret from the latest version of a file does **not** necessarily remove it from Git history.

For example:

```text
Commit A → Secret Added
Commit B → Secret Deleted
```

The secret may still exist in:

```text
Commit A
```

Therefore, the credential must still be revoked or rotated.

---

# Secret Scanning Workflow

```text
Developer Writes Code
        │
        ▼
Potential Secret
        │
        ▼
Push Protection
        │
   ┌────┴─────┐
   │          │
Safe       Detected
   │          │
   ▼          ▼
Push      Block / Alert
              │
              ▼
         Remove Secret
              │
              ▼
        Revoke / Rotate
```

---

# Best Practices

- Never hardcode real credentials.
- Use environment variables.
- Use GitHub Secrets for CI/CD credentials.
- Use `.gitignore` for sensitive local files.
- Enable Secret Scanning when available.
- Enable push protection when appropriate.
- Rotate exposed credentials immediately.
- Never print secrets in CI logs.

---

# Common Mistakes

- Assuming `.gitignore` protects secrets already committed.
- Deleting a secret without rotating it.
- Printing secrets in GitHub Actions logs.
- Sharing API keys in screenshots.
- Committing `.env` files containing real credentials.
- Ignoring security alerts.

---

# Secret Management Example

Instead of:

```python
API_KEY = "real-secret"
```

Use:

```python
import os

API_KEY = os.getenv("API_KEY")
```

Then provide the value securely through the appropriate environment or secret-management system.

---

# Security Layers

A secure repository can use multiple layers:

```text
                 Repository Security
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
   .gitignore     Secret Scanning   Push Protection
        │               │               │
        └───────────────┼───────────────┘
                        ▼
                 GitHub Secrets
                        │
                        ▼
                 Secure Workflows
```

No single security feature is enough by itself.

---

# Interview Questions

### What is GitHub Secret Scanning?

Secret Scanning is a GitHub security feature that helps detect credentials and other sensitive information exposed in repositories.

---

### What is push protection?

Push protection can detect supported secrets during a push and warn or prevent the secret from being committed.

---

### What should you do if an API key is accidentally committed?

Immediately revoke or rotate the key, replace it with a new credential, investigate the exposure, and remove the secret from the repository where appropriate.

---

### Does deleting a secret from a file remove it from Git history?

No. Previous commits may still contain the secret.

---

### Is `.gitignore` a replacement for Secret Scanning?

No. `.gitignore` helps prevent selected files from being tracked, while Secret Scanning detects potential secrets.

---

# Practice

For your learning repository:

1. Open the repository's:

```text
Settings
```

2. Explore:

```text
Security
```

3. Find the available secret-related security features.
4. Review Secret Scanning options.
5. Explore push protection if available.
6. Do **not** upload a real API key to test the feature.

---

# Summary

GitHub Secret Scanning helps detect exposed credentials and reduce the risk of secret leakage.

The most important rule is:

```text
If a real secret is exposed:

REVOKE → ROTATE → REPLACE → INVESTIGATE
```

Never rely only on deleting the secret from the latest version of the code.
