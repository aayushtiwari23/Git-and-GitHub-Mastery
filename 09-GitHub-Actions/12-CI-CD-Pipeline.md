# CI/CD Pipeline with GitHub Actions

## Introduction

CI/CD is one of the main reasons GitHub Actions is used in real software projects.

CI means:

**Continuous Integration**

CD means:

**Continuous Delivery** or **Continuous Deployment**

A typical pipeline looks like:

```text
Code
 ↓
Push
 ↓
Build
 ↓
Test
 ↓
Security Checks
 ↓
Package
 ↓
Deploy
```

---

# Continuous Integration

Continuous Integration means frequently integrating code changes into a shared repository and automatically validating those changes.

A CI pipeline may:

```text
Checkout Code
     ↓
Install Dependencies
     ↓
Build
     ↓
Run Tests
     ↓
Generate Reports
```

The goal is to discover problems early.

---

# Continuous Delivery

Continuous Delivery means keeping software in a deployable state.

After successful CI:

```text
Build
 ↓
Test
 ↓
Package
 ↓
Ready for Deployment
```

Deployment can then be triggered when appropriate.

---

# Continuous Deployment

Continuous Deployment goes one step further.

Successful changes can automatically be deployed:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Deploy
 ↓
Production
```

No manual deployment step is required for every successful change.

---

# CI vs CD

| CI | CD |
|---|---|
| Integrates code changes | Delivers/deploys software |
| Builds code | Packages/releases code |
| Runs tests | Deploys to environments |
| Finds problems early | Automates release process |

---

# Basic CI Pipeline

A simple CI workflow:

```yaml
name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Tests
        run: echo "Running tests"
```

Every push or Pull Request can trigger the workflow.

---

# CI Pipeline Stages

A typical pipeline can contain:

```text
1. Checkout
2. Setup Environment
3. Install Dependencies
4. Lint
5. Test
6. Build
7. Package
```

---

# Checkout

First, the workflow gets the repository code.

```yaml
- name: Checkout
  uses: actions/checkout@v4
```

---

# Setup Environment

For Python:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.12'
```

For Node.js:

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
```

---

# Install Dependencies

Python:

```yaml
- name: Install Dependencies
  run: pip install -r requirements.txt
```

Node.js:

```yaml
- name: Install Dependencies
  run: npm ci
```

---

# Linting

Linting checks source code for potential problems and style issues.

Example:

```yaml
- name: Run Linter
  run: npm run lint
```

A project can configure its own linting tool and command.

---

# Testing

Example:

```yaml
- name: Run Tests
  run: npm test
```

Or Python:

```yaml
- name: Run Tests
  run: pytest
```

If tests fail, the workflow can stop before deployment.

---

# Build

Example:

```yaml
- name: Build Application
  run: npm run build
```

The build can produce files that are later uploaded as artifacts or deployed.

---

# Artifact

After building:

```text
Build
 ↓
dist/
 ↓
Upload Artifact
```

Example:

```yaml
- name: Upload Build
  uses: actions/upload-artifact@v4
  with:
    name: application-build
    path: dist/
```

---

# CD Pipeline

A deployment pipeline may look like:

```text
Build
 ↓
Test
 ↓
Artifact
 ↓
Deploy to Staging
 ↓
Approval
 ↓
Deploy to Production
```

---

# Environments

Real applications commonly have:

```text
Development
Staging
Production
```

Each environment can have different:

```text
Configuration
Secrets
Permissions
Deployment Rules
```

---

# Development

Development is usually where new features are actively tested.

```text
Developer
   ↓
Development Environment
```

---

# Staging

Staging should resemble production as closely as practical.

```text
Build
 ↓
Staging
 ↓
Final Testing
```

---

# Production

Production is the environment used by real users.

```text
Staging
 ↓
Approval
 ↓
Production
```

Production deployments should receive stronger protection.

---

# Deployment Conditions

A workflow can restrict production deployment to a specific branch.

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

Conceptually:

```text
Any Branch
    ↓
Build + Test

main Branch
    ↓
Production Deployment
```

---

# Job Dependencies

Use:

```yaml
needs:
```

to control job order.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Build"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - run: echo "Test"

  deploy:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

Flow:

```text
Build
 ↓
Test
 ↓
Deploy
```

---

# Complete CI Pipeline

```yaml
name: CI Pipeline

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: npm

      - name: Install Dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build
```

This is a basic CI workflow.

---

# CI/CD Pipeline

A larger workflow could look like:

```text
              Push
               ↓
            Checkout
               ↓
            Install
               ↓
             Lint
               ↓
             Test
               ↓
             Build
               ↓
           Artifact
               ↓
          Staging Deploy
               ↓
          Verification
               ↓
          Production
```

---

# Separate Jobs

A cleaner architecture can separate responsibilities.

```text
Build
  ↓
Test
  ↓
Package
  ↓
Deploy
```

Example:

```yaml
jobs:
  build:
    ...

  test:
    needs: build
    ...

  package:
    needs: test
    ...

  deploy:
    needs: package
    ...
```

---

# Parallel Jobs

Not every job needs to wait for another.

For example:

```text
             Build
               ↓
       ┌───────┼───────┐
       ↓       ↓       ↓
     Lint     Test   Security
       └───────┼───────┘
               ↓
             Deploy
```

This can reduce total pipeline time.

---

# Matrix Testing

Matrix strategies are useful for compatibility testing.

Example:

```text
Ubuntu + Python 3.11
Ubuntu + Python 3.12
Windows + Python 3.11
Windows + Python 3.12
```

All combinations can be tested before deployment.

---

# Caching

Caching can make CI faster.

```text
First Run
 ↓
Download Dependencies
 ↓
Cache

Later Runs
 ↓
Restore Cache
 ↓
Faster Install
```

---

# Artifacts

Artifacts preserve generated files.

Examples:

```text
Build Packages
Test Reports
Coverage Reports
Logs
Screenshots
```

---

# Secrets

Deployment credentials should not be hardcoded.

Use:

```yaml
${{ secrets.DEPLOY_TOKEN }}
```

Example:

```yaml
env:
  DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

Never intentionally print the token.

---

# Permissions

Use the minimum required permissions.

Example:

```yaml
permissions:
  contents: read
```

Add additional permissions only when necessary.

---

# Production Protection

Production deployments can be protected using:

```text
Environment Rules
Required Approvals
Deployment Restrictions
Secrets
Permissions
```

Conceptually:

```text
Tests Pass
   ↓
Production Environment
   ↓
Approval
   ↓
Deploy
```

---

# Rollback

A good deployment system should have a rollback strategy.

Conceptually:

```text
Version 2
   ↓
Production
   ↓
Problem Detected
   ↓
Rollback
   ↓
Version 1
```

A rollback might involve redeploying a previously known-good version.

---

# Failed Deployment

If deployment fails:

```text
Build
 ↓
Test
 ↓
Deploy
 ↓
FAILED
```

The system should prevent the failed deployment from being treated as successful.

Monitoring and rollback procedures should be part of production operations.

---

# Notifications

Pipelines can also integrate with notification systems.

Examples:

```text
Build Failed
     ↓
Notification
     ↓
Developer Team
```

The exact notification system depends on the project.

---

# Security in CI/CD

Important security areas include:

```text
Secrets
Permissions
Third-Party Actions
Pull Requests
Dependencies
Artifacts
Deployment Credentials
```

CI/CD automation can have significant access to repositories and infrastructure, so security should be designed from the beginning.

---

# Common Mistakes

Avoid:

```text
Deploying without tests
Hardcoding credentials
Giving workflows excessive permissions
Using production credentials in test workflows
Ignoring failed deployments
Skipping dependency security
Creating unnecessarily long pipelines
```

---

# Best Practices

- Keep CI fast.
- Run tests automatically.
- Use caching where appropriate.
- Store build outputs as artifacts.
- Separate environments.
- Protect production deployments.
- Use least-privilege permissions.
- Store credentials as secrets.
- Use reliable deployment and rollback strategies.
- Monitor pipeline failures.

---

# Real-World Example

Imagine a web application.

Developer pushes code:

```text
Git Push
   ↓
GitHub Actions
   ↓
Checkout
   ↓
Install Dependencies
   ↓
Lint
   ↓
Unit Tests
   ↓
Build
   ↓
Upload Artifact
   ↓
Deploy Staging
   ↓
Smoke Tests
   ↓
Production Approval
   ↓
Deploy Production
```

This is the basic idea behind a professional CI/CD pipeline.

---

# Interview Questions

### What is CI?

Continuous Integration is the practice of frequently integrating code changes and automatically validating them through builds and tests.

### What is CD?

Continuous Delivery or Continuous Deployment automates the process of preparing or deploying software after successful validation.

### Why use CI/CD?

It helps automate testing, building, releasing, and deploying software while reducing manual errors.

### What is an artifact?

A stored file or collection of files generated by a workflow.

### Why use environments?

To separate configurations, secrets, permissions, and deployment stages such as development, staging, and production.

### Why use `needs`?

To establish dependencies between jobs.

### Why should production deployments be protected?

Because production affects real users and systems, so accidental or untested deployments can cause significant problems.

---

# Practice

Create:

```text
.github/workflows/ci-cd-practice.yml
```

Paste:

```yaml
name: CI CD Practice

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: |
          mkdir build
          echo "Application Build" > build/app.txt

      - name: Upload Artifact
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: build/

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "All tests passed"

  deploy:
    needs: test
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying application"
```

Commit message:

```text
Add GitHub Actions CI CD pipeline guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
CI CD Practice
   ↓
Run workflow
```

Observe:

```text
Build
 ↓
Test
 ↓
Deploy
```

If you're not running from `main`, the deployment job may be skipped because of the branch condition.

---

# Summary

CI/CD combines the GitHub Actions concepts you've learned so far:

```text
Triggers
   ↓
Jobs
   ↓
Actions
   ↓
Environment Variables
   ↓
Secrets
   ↓
Artifacts
   ↓
Caching
   ↓
Conditions
   ↓
Matrix Testing
   ↓
Reusable Workflows
   ↓
Deployment
```

A mature CI/CD pipeline automates the path from:

```text
Code
 ↓
Validation
 ↓
Build
 ↓
Release
 ↓
Deployment
```

The goal is not simply to automate everything. The goal is to create a reliable, secure, repeatable process for delivering software.
