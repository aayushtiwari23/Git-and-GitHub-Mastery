
# GitHub Actions Environment Variables and Secrets

## Introduction

GitHub Actions workflows often need configuration values and sensitive information.

Examples:

- Application environment
- API keys
- Access tokens
- Database credentials
- Deployment credentials

GitHub Actions provides **environment variables** and **secrets** to manage these values.

---

# Environment Variables

An environment variable is a named value available to a process during workflow execution.

Example:

```yaml
env:
  APP_ENV: development
```

The variable can then be used by workflow steps.

---

# Workflow-Level Environment Variables

You can define an environment variable for the entire workflow.

Example:

```yaml
name: Environment Example

on:
  workflow_dispatch:

env:
  APP_ENV: development

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Show Environment
        run: echo "$APP_ENV"
```

The variable is available to the workflow's jobs and steps according to its scope.

---

# Job-Level Environment Variables

An environment variable can be limited to a specific job.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    env:
      APP_ENV: testing

    steps:
      - name: Show Environment
        run: echo "$APP_ENV"
```

The variable is available within that job.

---

# Step-Level Environment Variables

You can also define a variable for only one step.

Example:

```yaml
steps:
  - name: Show Mode
    env:
      MODE: development
    run: echo "$MODE"
```

The variable is available to that step.

---

# Environment Variable Scope

There are three common scopes:

```text
Workflow
   ↓
Job
   ↓
Step
```

A narrower scope can override a broader value.

Conceptually:

```text
Workflow ENV
      ↓
   Job ENV
      ↓
  Step ENV
```

---

# Example of Different Scopes

```yaml
name: Scope Example

on:
  workflow_dispatch:

env:
  MODE: workflow

jobs:
  example:
    runs-on: ubuntu-latest

    env:
      MODE: job

    steps:
      - name: Workflow Job Value
        run: echo "$MODE"

      - name: Step Value
        env:
          MODE: step
        run: echo "$MODE"
```

The second step uses its step-level value.

---

# Why Secrets Are Different

Environment variables are useful for normal configuration values.

Secrets are designed for sensitive information.

Examples:

```text
API Keys
Access Tokens
Passwords
Private Credentials
Cloud Credentials
```

Do not put real secrets directly into workflow YAML files.

---

# What is a GitHub Secret?

A GitHub Secret is an encrypted value stored by GitHub and made available to authorized workflows.

Conceptually:

```text
Secret Value
     ↓
GitHub Secret Storage
     ↓
Workflow
     ↓
Application
```

---

# Using a Secret

Secrets are accessed through:

```text
secrets
```

Example:

```yaml
steps:
  - name: Use API Key
    run: echo "API key is available"
    env:
      API_KEY: ${{ secrets.API_KEY }}
```

The actual secret value should not be written directly into the workflow file.

---

# Secret Expression Syntax

The general syntax is:

```text
${{ secrets.SECRET_NAME }}
```

Example:

```yaml
${{ secrets.API_KEY }}
```

Another:

```yaml
${{ secrets.DATABASE_PASSWORD }}
```

---

# Never Hardcode Secrets

Bad:

```yaml
env:
  API_KEY: "my-real-api-key"
```

This exposes the credential in the workflow file.

Better:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

The actual value remains stored as a GitHub Secret.

---

# Secrets in GitHub Actions

A common pattern is:

```yaml
name: Secret Example

on:
  workflow_dispatch:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Use Secret
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: echo "Secret is configured"
```

Notice that the secret itself is not written into the YAML file.

---

# Do Not Print Secrets

Never intentionally print a secret:

```yaml
run: echo "$API_KEY"
```

Even though GitHub attempts to redact supported secret values from logs, secrets should never be intentionally exposed.

Instead:

```yaml
run: echo "API key configured"
```

---

# Secret Masking

GitHub Actions can mask many secret values in logs.

Conceptually:

```text
Secret:
abc123secret

Log:
***
```

However, masking should not be treated as permission to print secrets.

The safest approach is:

```text
Never print secrets
```

---

# Repository Secrets

Secrets can be configured for a repository and then used by workflows that have access to them.

Example use cases:

```text
API credentials
Deployment credentials
Service tokens
Cloud credentials
```

---

# Environment Secrets

GitHub environments can also have environment-specific secrets.

For example:

```text
development
staging
production
```

Conceptually:

```text
Development
    ↓
Development Secrets

Staging
    ↓
Staging Secrets

Production
    ↓
Production Secrets
```

This helps separate credentials between environments.

---

# Why Separate Environments?

Imagine an application has:

```text
Development Database
Staging Database
Production Database
```

Using the same credentials everywhere increases risk.

A better structure is:

```text
Development → Dev Credentials
Staging     → Staging Credentials
Production  → Production Credentials
```

---

# Organization Secrets

Organizations can also manage secrets that are shared with selected repositories.

This is useful when multiple repositories use common services or credentials.

Conceptually:

```text
Organization
     │
     ├── Repository A
     ├── Repository B
     └── Repository C
```

An organization secret can be made available according to configured repository access.

---

# Variables vs Secrets

GitHub also supports configuration variables.

A useful distinction is:

| Variables | Secrets |
|-----------|---------|
| Non-sensitive configuration | Sensitive information |
| Example: environment name | Example: API token |
| Can be referenced in workflows | Stored securely |
| Not intended for confidential values | Designed for confidential values |

Do not store sensitive credentials as ordinary variables.

---

# Environment Variables vs Secrets

Example:

```text
APP_ENV=production
```

This is usually configuration.

Whereas:

```text
API_KEY=real-secret-value
```

is sensitive and should be stored as a secret.

---

# Secret Naming

Use clear names.

Examples:

```text
API_KEY
DATABASE_URL
DATABASE_PASSWORD
DEPLOYMENT_TOKEN
CLOUD_ACCESS_KEY
```

Avoid confusing names such as:

```text
thing
secret1
abc
test123
```

---

# Using Secrets with Applications

Example:

```yaml
steps:
  - name: Run Application
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: python app.py
```

The application can read the environment variable during execution.

Conceptually:

```text
GitHub Secret
     ↓
Environment Variable
     ↓
Application
```

---

# Secrets and Pull Requests

Be especially careful when workflows run for Pull Requests from forks.

Untrusted code should not automatically receive access to sensitive credentials.

A useful security principle is:

```text
Untrusted Code
      ↓
Do Not Give Sensitive Secrets
```

---

# Secrets and GitHub Actions Permissions

Secrets are only one part of workflow security.

Also consider:

```text
Repository Permissions
Workflow Permissions
Pull Request Source
Third-Party Actions
Runner Security
```

A secret should only be made available when it is actually required.

---

# Environment Protection

GitHub environments can provide additional controls around sensitive deployments.

For example:

```text
Production
    ↓
Required Approval
    ↓
Deployment
```

This can help prevent accidental production deployments.

---

# Example Deployment Structure

```text
Code
 ↓
Pull Request
 ↓
Tests
 ↓
Build
 ↓
Production Environment
 ↓
Approval
 ↓
Production Secrets
 ↓
Deploy
```

This provides multiple layers of protection.

---

# Secret Rotation

Secrets should be rotated when necessary.

If a credential is exposed:

```text
Secret Exposed
      ↓
Revoke / Rotate
      ↓
Create Replacement
      ↓
Update GitHub Secret
      ↓
Test
```

Do not continue using a compromised credential.

---

# What If a Secret Was Committed?

If you accidentally commit a real secret:

```text
DO NOT
↓
Simply delete the file
```

Deleting the file in a later commit does not necessarily remove the secret from Git history.

Instead:

```text
1. Revoke or rotate the credential.
2. Investigate where it was exposed.
3. Remove the secret from the repository/history where appropriate.
4. Update the GitHub Secret.
5. Review related systems.
```

The first priority is always to invalidate the exposed credential.

---

# Example: Safe Workflow

```yaml
name: Secure Workflow

on:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Run Application
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: python app.py
```

This demonstrates:

```text
Minimum Permission
       +
Secret Reference
       +
Environment Variable
       +
Application
```

---

# Common Mistakes

Avoid:

```text
Hardcoding API keys
Committing .env files
Printing secrets
Using production secrets unnecessarily
Giving secrets to untrusted code
Using the same credentials everywhere
Ignoring exposed credentials
```

---

# Best Practices

- Store sensitive values as secrets.
- Never hardcode credentials.
- Never intentionally print secrets.
- Use separate credentials for different environments.
- Give workflows only required permissions.
- Avoid exposing secrets to untrusted code.
- Rotate compromised credentials immediately.
- Use environment protection for sensitive deployments.
- Keep non-sensitive configuration separate from secrets.

---

# Interview Questions

### What is a GitHub Secret?

A GitHub Secret is a protected value that can be securely provided to authorized workflows.

### How do you reference a GitHub Secret?

```text
${{ secrets.SECRET_NAME }}
```

### Should API keys be stored in workflow YAML?

No. Sensitive credentials should be stored as GitHub Secrets or an appropriate external secret-management system.

### What is an environment variable?

A named value made available to a process during workflow execution.

### What is the difference between a variable and a secret?

Variables are generally used for non-sensitive configuration, while secrets are intended for sensitive information.

### What should you do if an API key is accidentally committed?

Immediately revoke or rotate the key, then investigate and clean up the exposure appropriately.

---

# Practice

Create:

```text
.github/workflows/secrets-practice.yml
```

Paste:

```yaml
name: Secrets Practice

on:
  workflow_dispatch:

jobs:
  practice:
    runs-on: ubuntu-latest

    steps:
      - name: Check Secret Configuration
        env:
          DEMO_SECRET: ${{ secrets.DEMO_SECRET }}
        run: |
          if [ -n "$DEMO_SECRET" ]; then
            echo "Secret is configured."
          else
            echo "Secret is not configured."
          fi
```

Then create a repository secret named:

```text
DEMO_SECRET
```

Use any harmless test value for learning.

Do not print the value.

Run:

```text
Repository
   ↓
Actions
   ↓
Secrets Practice
   ↓
Run workflow
```

Check the logs.

---

# Summary

Environment variables store configuration values that workflows and applications can use.

Secrets protect sensitive information.

The basic pattern is:

```text
Configuration
     ↓
Environment Variables

Sensitive Information
     ↓
GitHub Secrets
```

A secure workflow should follow:

```text
Store Securely
      ↓
Use Only When Needed
      ↓
Never Print
      ↓
Rotate When Necessary
```

Proper handling of environment variables and secrets is essential for secure GitHub Actions workflows.
