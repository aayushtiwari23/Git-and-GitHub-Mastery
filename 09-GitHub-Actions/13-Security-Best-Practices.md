# GitHub Actions Security Best Practices

## Introduction

GitHub Actions workflows can access source code, secrets, packages, and deployment systems.

Because of this, workflow security is extremely important.

A secure workflow follows:

```text
Minimum Access
      ↓
Protected Secrets
      ↓
Trusted Actions
      ↓
Controlled Workflows
      ↓
Safe Deployment
```

---

# Principle of Least Privilege

Give a workflow only the permissions it actually needs.

Example:

```yaml
permissions:
  contents: read
```

This allows the workflow to read repository contents without automatically granting unnecessary write permissions.

---

# Why Permissions Matter

A workflow may have access to:

```text
Repository
Secrets
Packages
Cloud Services
Deployment Systems
```

If an attacker compromises a workflow, excessive permissions can increase the damage.

Therefore:

```text
More Permissions
      ↓
Higher Potential Risk
```

---

# Define Permissions Explicitly

Example:

```yaml
name: Secure Workflow

on:
  push:

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Test
        run: echo "Running tests"
```

Use additional permissions only when required.

---

# Secrets

Never hardcode sensitive information.

Bad:

```yaml
env:
  API_KEY: "real-api-key"
```

Better:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Store sensitive values using GitHub Secrets or an appropriate external secret-management system.

---

# Never Print Secrets

Do not do this:

```yaml
run: echo "$API_KEY"
```

Instead:

```yaml
run: echo "API key is configured"
```

Even with log masking, secrets should never be intentionally exposed.

---

# Secret Exposure

If a secret is accidentally exposed:

```text
Secret Exposed
      ↓
Immediately Revoke / Rotate
      ↓
Investigate Exposure
      ↓
Clean Up
      ↓
Create Replacement

```

Do not assume deleting the secret from the latest commit makes it safe.

---

# Pull Requests from Forks

Be especially careful with Pull Requests originating from forks.

A Pull Request can contain code controlled by someone outside the repository.

Conceptually:

```text
External Code
      ↓
Pull Request
      ↓
Workflow
      ↓
Potentially Untrusted Code
```

Do not unnecessarily expose sensitive credentials to untrusted workflows.

---

# Third-Party Actions

GitHub Actions frequently use third-party Actions.

Example:

```yaml
- uses: some-user/some-action@v1
```

Remember:

```text
Third-Party Action
       ↓
Runs Inside Your Workflow
       ↓
May Receive Workflow Access
```

Only use Actions you trust.

---

# Pin Action Versions

Prefer a stable version reference rather than an uncontrolled moving reference.

Example:

```yaml
uses: actions/checkout@v4
```

For higher security requirements, Actions can be pinned to a specific commit SHA after verifying the referenced code.

Conceptually:

```text
Action
 ↓
Verified Commit
 ↓
Fixed Version
```

This reduces the risk of unexpected changes.

---

# Why Pinning Matters

Suppose a workflow uses:

```yaml
uses: example/action@main
```

The `main` branch can change over time.

Today:

```text
main → Safe Code
```

Later:

```text
main → Different Code
```

A fixed version or verified commit provides stronger reproducibility.

---

# Dependencies

Actions can depend on other software packages.

Keep dependencies updated and monitor them for known security issues.

Typical flow:

```text
Dependency
     ↓
Security Update
     ↓
Test
     ↓
Deploy
```

---

# Workflow Injection

One important security problem is command injection.

Never blindly insert untrusted user-controlled data into shell commands.

Unsafe pattern:

```yaml
run: echo "${{ github.event.issue.title }}"
```

If the value can contain shell-sensitive characters, it may create unexpected behavior depending on how it is used.

Treat event data as untrusted input.

---

# Safer Approach

Instead of directly inserting untrusted values into shell syntax, pass them through environment variables where appropriate.

Example:

```yaml
env:
  ISSUE_TITLE: ${{ github.event.issue.title }}

run: |
  echo "$ISSUE_TITLE"
```

The exact safe approach depends on how the value is being used.

---

# Untrusted Input

Examples of potentially untrusted data include:

```text
Pull Request Titles
Issue Titles
Commit Messages
Branch Names
User Input
External Data
```

Do not assume these values are safe to place directly into shell commands.

---

# `pull_request` vs `pull_request_target`

These events have important security differences.

`pull_request` workflows generally run in the context of the pull request's merge state and are designed with forked contributions in mind.

`pull_request_target` runs in the context of the base repository.

Because of this, `pull_request_target` can have access to repository secrets and write permissions that ordinary Pull Request workflows may not have.

Do not use `pull_request_target` casually, especially when executing code from the Pull Request.

---

# Dangerous Pattern

Avoid workflows that effectively do:

```text
Untrusted Pull Request Code
        ↓
Access to Repository Secrets
        ↓
Privileged Execution
```

This can create a serious security vulnerability.

---

# Self-Hosted Runners

Self-hosted runners require additional security consideration.

Unlike GitHub-hosted runners, they run on infrastructure you manage.

Potential concerns include:

```text
Persistent Files
Network Access
Credentials
Installed Software
Malicious Workflows
Untrusted Pull Requests
```

Do not expose sensitive self-hosted runners to untrusted workflows.

---

# GitHub-Hosted vs Self-Hosted

| GitHub-Hosted | Self-Hosted |
|---|---|
| Managed runner environment | You manage the machine |
| Fresh environments are commonly used | Environment may persist |
| Less infrastructure maintenance | More control |
| Easier for many projects | Requires stronger security management |

---

# Runner Isolation

If using self-hosted runners:

```text
Untrusted Code
      ↓
Do Not Give
      ↓
Sensitive Runner
```

Consider dedicated runners and strong isolation for sensitive workloads.

---

# Environment Protection

Production deployments should be protected.

Example:

```text
Build
 ↓
Test
 ↓
Production Environment
 ↓
Approval
 ↓
Deploy
```

Environment protection rules can help prevent accidental deployments.

---

# Production Secrets

Do not use production credentials for ordinary testing.

Prefer:

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

This limits the impact of a compromised development workflow.

---

# Environment Restrictions

Production deployment can be restricted by:

```text
Branch
Environment
Approval
Permissions
Deployment Rules
```

This creates additional protection around critical operations.

---

# OIDC

GitHub Actions can use OpenID Connect (OIDC) to authenticate with supported cloud providers without storing long-lived cloud credentials as GitHub Secrets.

Conceptually:

```text
GitHub Actions
      ↓
OIDC Identity Token
      ↓
Cloud Provider
      ↓
Temporary Access
```

This can reduce the need for long-lived credentials.

---

# Why OIDC Is Useful

Traditional approach:

```text
Long-Lived Cloud Credential
        ↓
GitHub Secret
        ↓
Workflow
```

OIDC approach:

```text
Workflow
   ↓
Short-Lived Identity
   ↓
Cloud Provider
```

The exact setup depends on the cloud provider.

---

# Artifact Security

Artifacts can contain:

```text
Build Output
Logs
Reports
Packages
Source Files
```

Do not upload sensitive information unnecessarily.

Before uploading an artifact, ask:

```text
Does this contain credentials?
Does this contain private data?
Does this contain unnecessary files?
```

---

# Cache Security

Caches can contain reusable dependency data.

Do not intentionally store:

```text
Passwords
API Keys
Private Tokens
Sensitive Data
```

Be careful when workflows process untrusted code.

---

# Avoid Excessive Permissions

Bad approach:

```yaml
permissions: write-all
```

Prefer only what is required.

For example:

```yaml
permissions:
  contents: read
```

If a workflow needs to create something, grant the specific permission required.

---

# Review Workflow Changes

Treat changes to:

```text
.github/workflows/
```

as security-sensitive.

A workflow change can modify:

```text
Permissions
Secrets
Deployment
Repository Access
Execution
```

Review workflow changes carefully.

---

# CODEOWNERS

Organizations can use `CODEOWNERS` to require specific reviewers for sensitive files.

For example:

```text
.github/workflows/
```

can be assigned to trusted maintainers.

Conceptually:

```text
Workflow Change
      ↓
Required Review
      ↓
Approved
      ↓
Merge
```

---

# Branch Protection

Protect important branches such as:

```text
main
production
release
```

Possible protections include:

```text
Required Pull Requests
Required Reviews
Required Status Checks
```

This reduces accidental or unauthorized changes.

---

# Status Checks

CI workflows can act as required checks.

Example:

```text
Pull Request
      ↓
CI
      ↓
Tests
      ↓
Security Scan
      ↓
All Checks Pass
      ↓
Merge Allowed
```

---

# Security Scanning

Security checks can be integrated into CI.

Examples:

```text
Dependency Scanning
Secret Scanning
Static Analysis
Container Scanning
```

The exact tools depend on the project.

---

# Dependency Updates

Keep workflow Actions and project dependencies updated.

Example:

```text
Old Dependency
      ↓
Security Advisory
      ↓
Update
      ↓
Test
      ↓
Deploy
```

Do not blindly update everything without testing.

---

# Audit Workflow Access

Regularly review:

```text
Repository Permissions
Secrets
Environments
Actions
Runners
Deployments
```

Remove access that is no longer required.

---

# Common Security Mistakes

Avoid:

```text
Hardcoded secrets
Printing secrets
Using excessive permissions
Trusting all third-party Actions
Executing untrusted input directly in shell commands
Giving forked Pull Requests sensitive credentials
Using production secrets in development
Ignoring self-hosted runner risks
Using privileged workflows unnecessarily
```

---

# Security Checklist

Before considering a workflow production-ready:

```text
[ ] Permissions are minimal
[ ] Secrets are stored securely
[ ] Secrets are never printed
[ ] Third-party Actions are trusted
[ ] Action versions are controlled
[ ] Untrusted input is handled safely
[ ] Production environments are protected
[ ] Self-hosted runners are secured
[ ] Artifacts do not contain unnecessary secrets
[ ] Caches do not contain sensitive information
[ ] Workflow changes are reviewed
[ ] Dependencies are monitored
```

---

# Secure Workflow Example

```yaml
name: Secure CI

on:
  pull_request:

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Tests
        run: echo "Tests completed"
```

The important part is not the complexity.

It is:

```text
Minimal Permissions
+
Trusted Actions
+
No Unnecessary Secrets
+
Safe Input Handling
```

---

# Interview Questions

### What is the principle of least privilege?

Give a workflow only the permissions required for its job.

### Why should secrets never be hardcoded?

Because anyone who can access the workflow source may be able to obtain the credential.

### Why are third-party Actions a security concern?

Actions execute code inside your workflow and may have access to the workflow environment.

### What is workflow injection?

A security problem where untrusted input is interpreted as part of a command or workflow expression.

### Why is `pull_request_target` sensitive?

It executes in the base repository context and can have access to privileges and secrets that ordinary Pull Request workflows do not have.

### What is OIDC?

A mechanism that allows GitHub Actions to authenticate with supported external services using short-lived identity credentials instead of long-lived stored credentials.

### Why are self-hosted runners risky?

They are infrastructure you manage and may retain files, credentials, network access, and installed software between jobs.

---

# Practice

Create:

```text
.github/workflows/security-practice.yml
```

Paste:

```yaml
name: Security Practice

on:
  workflow_dispatch:

permissions:
  contents: read

jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Show Permissions
        run: echo "Workflow uses minimal repository permissions"

      - name: Security Check
        run: echo "Security check completed"
```

Commit message:

```text
Add GitHub Actions security best practices guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Security Practice
   ↓
Run workflow
```

Check that the workflow completes successfully.

---

# Summary

GitHub Actions security is mainly about controlling access and reducing trust.

Remember:

```text
Least Privilege
      ↓
Protect Secrets
      ↓
Trust Dependencies
      ↓
Validate Inputs
      ↓
Protect Environments
      ↓
Secure Runners
      ↓
Review Workflow Changes
```

A secure CI/CD pipeline should not only work correctly.

It should also limit what can happen when something goes wrong.
