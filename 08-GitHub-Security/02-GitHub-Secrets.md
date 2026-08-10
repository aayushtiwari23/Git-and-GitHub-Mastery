
# GitHub Secrets

## Introduction

GitHub Secrets provide a secure way to store sensitive information used by repositories, GitHub Actions, and applications.

Instead of writing passwords, API keys, or access tokens directly in code or workflow files, you can store them as encrypted secrets.

---

# What Are GitHub Secrets?

GitHub Secrets are encrypted values that can be used by authorized GitHub Actions workflows and other supported GitHub features.

Examples:

```text
API_KEY
DATABASE_PASSWORD
AWS_ACCESS_KEY
DEPLOYMENT_TOKEN
```

The actual secret value should never be written directly into your source code.

---

# Why Use Secrets?

Secrets help prevent sensitive information from being exposed through:

- Source code
- Git commits
- Pull Requests
- Workflow files
- Public repositories

---

# Common Secret Types

Examples include:

```text
API Keys
Access Tokens
Database Credentials
Cloud Credentials
SSH Keys
Deployment Tokens
Third-Party Service Credentials
```

---

# GitHub Secrets vs Normal Variables

| Secret | Variable |
|--------|----------|
| Sensitive information | Non-sensitive configuration |
| Encrypted | Generally not treated as a secret |
| Used for credentials | Used for normal values |
| Should be protected | Usually safe to expose |

Example:

Secret:

```text
API_KEY
```

Normal variable:

```text
APP_ENV=production
```

---

# Repository Secrets

Repository secrets are available to workflows in a specific repository.

Typical workflow:

```text
Repository
    │
    ▼
Settings
    │
    ▼
Secrets and variables
    │
    ▼
Actions
    │
    ▼
New repository secret
```

---

# Creating a Repository Secret

Go to your repository:

```text
Settings
   ↓
Secrets and variables
   ↓
Actions
   ↓
New repository secret
```

Give the secret a name.

Example:

```text
API_KEY
```

Then enter its value and save it.

---

# Using a Secret in GitHub Actions

Example:

```yaml
name: Example Workflow

on:
  push:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Use API Key
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: echo "API key is available to the workflow"
```

The secret value is not written directly into the workflow file.

---

# Important Rule

Never do this:

```yaml
env:
  API_KEY: "123456789abcdef"
```

Instead:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

---

# Organization Secrets

Organizations can create secrets that can be shared with selected repositories.

Useful when multiple repositories use the same service.

Example:

```text
Organization
     │
     ├── Repository A
     ├── Repository B
     └── Repository C
            │
            ▼
      Shared Secret
```

---

# Environment Secrets

GitHub environments can also have their own secrets.

Examples:

```text
Development
Staging
Production
```

Each environment can have different credentials.

Example:

```text
Development → DEV_API_KEY
Staging     → STAGING_API_KEY
Production  → PROD_API_KEY
```

This helps prevent accidentally using production credentials during development.

---

# Secrets in Pull Requests

Be careful when workflows run on Pull Requests from forks.

Giving untrusted code access to sensitive secrets can create serious security risks.

Never assume that code submitted by an unknown contributor is safe to execute with access to your secrets.

---

# Secrets and Logs

GitHub attempts to prevent secrets from appearing in workflow logs.

However, you should still avoid printing secret values.

Never intentionally do:

```bash
echo $API_KEY
```

or:

```bash
printenv
```

when sensitive environment variables are available.

---

# If a Secret Is Exposed

If an API key or credential becomes public:

### Step 1

Immediately revoke or rotate the credential.

### Step 2

Create a replacement credential.

### Step 3

Update the GitHub Secret.

### Step 4

Investigate where the secret was exposed.

### Step 5

Check whether it was used.

Deleting the secret from the current file is not enough because it may remain in Git history.

---

# GitHub Secrets Workflow

```text
Create Credential
       │
       ▼
Store as GitHub Secret
       │
       ▼
GitHub Actions
       │
       ▼
Workflow Uses Secret
       │
       ▼
Application / Service
```

---

# Example: Deployment

A deployment workflow might require:

```text
DEPLOYMENT_TOKEN
```

Instead of putting the token in the workflow:

```yaml
env:
  DEPLOYMENT_TOKEN: ${{ secrets.DEPLOYMENT_TOKEN }}
```

The workflow can securely authenticate with the deployment service.

---

# Best Practices

- Never hardcode credentials.
- Use descriptive secret names.
- Rotate credentials regularly when appropriate.
- Use separate credentials for different environments.
- Limit access to secrets.
- Never print secrets in logs.
- Review workflows that have access to secrets.
- Revoke exposed credentials immediately.

---

# Common Mistakes

- Putting secrets directly in source code.
- Committing `.env` files containing real credentials.
- Printing secrets in CI logs.
- Giving untrusted workflows access to secrets.
- Using production credentials for development.
- Assuming deleting a secret from a file removes it from Git history.

---

# Interview Questions

### What are GitHub Secrets?

GitHub Secrets are encrypted values used to securely store sensitive information for repositories and GitHub Actions.

---

### Why shouldn't API keys be stored in source code?

Because source code may be accessible to other developers or the public, allowing attackers to obtain and misuse the credentials.

---

### How do you reference a repository secret in GitHub Actions?

```yaml
${{ secrets.SECRET_NAME }}
```

---

### What should you do if a secret is accidentally exposed?

Immediately revoke or rotate the credential, replace the secret, and investigate the exposure.

---

### What is the difference between a repository secret and an environment secret?

A repository secret is available to workflows in the repository, while an environment secret can be associated with a specific GitHub environment such as development, staging, or production.

---

# Practice

For your learning repository:

1. Open:

```text
Settings
→ Secrets and variables
→ Actions
```

2. Do **not** add a real API key just for practice.

3. Understand where repository secrets are created.

4. Create a test secret only if you have a harmless dummy value.

5. Create a GitHub Actions workflow that references:

```yaml
${{ secrets.TEST_SECRET }}
```

6. Never print the actual value.

---

# Summary

GitHub Secrets provide a secure mechanism for storing sensitive credentials used by repositories and automated workflows. They are especially important when working with GitHub Actions, cloud services, APIs, and deployment systems.

The most important rule is:

**Never put real secrets directly into your source code or Git history.**
