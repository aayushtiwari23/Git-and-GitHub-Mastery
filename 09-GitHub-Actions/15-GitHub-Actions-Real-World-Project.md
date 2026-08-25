
# GitHub Actions Real-World Project

## Project Overview

This project combines the GitHub Actions concepts learned so far into one realistic CI/CD workflow.

We will build a pipeline that performs:

```text
Push / Pull Request
        ↓
      CI
        ↓
     Lint
        ↓
     Test
        ↓
     Build
        ↓
    Artifact
        ↓
    Deploy
```

---

# Project Goal

Create a GitHub Actions workflow for a simple application.

The workflow should:

```text
1. Trigger automatically
2. Run tests
3. Build the project
4. Store the build
5. Support multiple versions
6. Use conditions
7. Use artifacts
8. Use secure permissions
9. Support manual execution
```

---

# Project Structure

Create:

```text
.github/
└── workflows/
    └── real-world-ci.yml
```

The repository can contain your normal application files.

---

# Workflow Trigger

Use:

```yaml
on:
  push:
  pull_request:
  workflow_dispatch:
```

This means the workflow can run when:

```text
Code is pushed
Pull Request is created/updated
Workflow is manually started
```

---

# Permissions

Start with minimum permissions:

```yaml
permissions:
  contents: read
```

The workflow only receives the repository access it needs.

---

# Matrix Testing

Use a matrix to test multiple Node.js versions.

```yaml
strategy:
  matrix:
    node-version:
      - "18"
      - "20"
      - "22"
```

This produces:

```text
Node 18
Node 20
Node 22
```

---

# Setup Node.js

Use:

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: ${{ matrix.node-version }}
    cache: npm
```

The matrix value determines which Node.js version is installed.

---

# Install Dependencies

Use:

```yaml
- name: Install Dependencies
  run: npm ci
```

`npm ci` is commonly used in CI environments when the project has a lockfile.

---

# Run Lint

Use:

```yaml
- name: Lint
  run: npm run lint
```

This assumes the project defines a lint script.

---

# Run Tests

Use:

```yaml
- name: Test
  run: npm test
```

The workflow should stop if the test command fails.

---

# Build

Use:

```yaml
- name: Build
  run: npm run build
```

The exact build command depends on the project.

---

# Upload Artifact

After building:

```yaml
- name: Upload Build
  uses: actions/upload-artifact@v4
  with:
    name: build-node-${{ matrix.node-version }}
    path: dist/
```

Each matrix job can upload its own build artifact.

---

# Complete CI Job

Example:

```yaml
jobs:
  ci:
    name: Node ${{ matrix.node-version }}

    strategy:
      fail-fast: false

      matrix:
        node-version:
          - "18"
          - "20"
          - "22"

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Install Dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build

      - name: Upload Build
        uses: actions/upload-artifact@v4
        with:
          name: build-node-${{ matrix.node-version }}
          path: dist/
```

---

# Manual Deployment

Add:

```yaml
workflow_dispatch:
```

You can also accept an environment:

```yaml
workflow_dispatch:
  inputs:
    environment:
      description: "Deployment environment"
      required: true
      type: choice
      options:
        - staging
        - production
```

---

# Deployment Job

The deployment job can depend on CI.

```yaml
deploy:
  needs: ci
  runs-on: ubuntu-latest

  steps:
    - name: Deploy
      run: echo "Deploying application"
```

Flow:

```text
Matrix CI
 ├── Node 18
 ├── Node 20
 └── Node 22
       ↓
    Deploy
```

---

# Production Condition

You can restrict production deployment:

```yaml
if: ${{ github.ref == 'refs/heads/main' && inputs.environment == 'production' }}
```

This means:

```text
Branch = main
AND
Environment = production
```

must both be true.

---

# Staging Condition

A staging deployment can use:

```yaml
if: ${{ inputs.environment == 'staging' }}
```

---

# Environment Protection

For real production deployments, use a GitHub Environment such as:

```text
production
```

Production can be configured with additional protection rules.

Conceptually:

```text
CI
 ↓
Production Environment
 ↓
Protection
 ↓
Approval
 ↓
Deployment
```

---

# Concurrency

Use:

```yaml
concurrency:
  group: production-deployment
  cancel-in-progress: false
```

This prevents multiple production deployments from running simultaneously within the same concurrency group.

---

# Security

The workflow should:

```text
Use minimal permissions
Avoid hardcoded credentials
Avoid printing secrets
Use trusted Actions
Protect production
```

---

# Real-World Workflow

A complete example:

```yaml
name: Real World CI CD

on:
  push:
  pull_request:

  workflow_dispatch:
    inputs:
      environment:
        description: "Deployment environment"
        required: true
        type: choice
        options:
          - staging
          - production

permissions:
  contents: read

concurrency:
  group: application-ci
  cancel-in-progress: false

jobs:
  ci:
    name: Node ${{ matrix.node-version }}

    strategy:
      fail-fast: false

      matrix:
        node-version:
          - "18"
          - "20"
          - "22"

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Install Dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm test

      - name: Build
        run: npm run build

      - name: Upload Build
        uses: actions/upload-artifact@v4
        with:
          name: build-node-${{ matrix.node-version }}
          path: dist/

  deploy:
    name: Deploy to ${{ inputs.environment }}
    needs: ci

    if: ${{ github.event_name == 'workflow_dispatch' }}

    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying to ${{ inputs.environment }}"
```

---

# Pipeline Flow

The workflow creates:

```text
                CI
                ↓
       ┌────────┼────────┐
       ↓        ↓        ↓
    Node 18   Node 20   Node 22
       ↓        ↓        ↓
     Lint      Lint      Lint
       ↓        ↓        ↓
     Test      Test      Test
       ↓        ↓        ↓
    Build     Build     Build
       ↓        ↓        ↓
  Artifact  Artifact  Artifact
       └────────┼────────┘
                ↓
             Deploy
```

---

# Important Improvement

The example deployment job only demonstrates the pipeline.

It does not actually deploy an application.

A real deployment requires a target such as:

```text
Cloud Platform
Web Server
Container Platform
Package Registry
Hosting Provider
```

The deployment command depends on the platform.

---

# Project Validation

After creating the workflow:

```text
1. Commit the workflow
2. Open Actions
3. Select Real World CI CD
4. Run workflow manually
5. Select staging
6. Observe the matrix jobs
7. Check the artifacts
8. Check the deploy job
```

---

# Expected Matrix Jobs

You should see:

```text
Node 18
Node 20
Node 22
```

Each should execute:

```text
Checkout
 ↓
Setup
 ↓
Install
 ↓
Lint
 ↓
Test
 ↓
Build
 ↓
Upload Artifact
```

---

# What Happens If a Test Fails?

Suppose:

```text
Node 18 → Pass
Node 20 → Fail
Node 22 → Pass
```

Because:

```yaml
fail-fast: false
```

the other matrix jobs can continue.

The overall CI job is still considered unsuccessful because one matrix variation failed.

---

# What Happens to Deployment?

Because:

```yaml
needs: ci
```

the deployment job depends on the CI job.

If the required CI job does not successfully complete, deployment should not proceed normally.

This creates a basic safety barrier:

```text
Tests
 ↓
Must Pass
 ↓
Deployment
```

---

# Why This Is Better Than Manual Testing

Without CI/CD:

```text
Developer
 ↓
Remember to Test
 ↓
Remember to Build
 ↓
Remember to Deploy
```

With GitHub Actions:

```text
Developer
 ↓
Push
 ↓
Automation
 ↓
Test
 ↓
Build
 ↓
Deploy
```

Automation reduces repetitive manual work.

---

# Real-World Extensions

This project can later be expanded with:

```text
Security Scanning
Dependency Scanning
Docker
Container Registry
Cloud Deployment
Staging Tests
Production Approval
Rollback
Notifications
Reusable Workflows
Infrastructure as Code
```

---

# Example Advanced Architecture

```text
                         Git Push
                            ↓
                       GitHub Actions
                            ↓
                   ┌────────┴────────┐
                   ↓                 ↓
                  Lint            Security
                   ↓                 ↓
                   └────────┬────────┘
                            ↓
                       Matrix Tests
                    ┌───────┼───────┐
                    ↓       ↓       ↓
                  Node18  Node20  Node22
                    └───────┼───────┘
                            ↓
                          Build
                            ↓
                         Artifact
                            ↓
                         Staging
                            ↓
                      Smoke Tests
                            ↓
                    Production Approval
                            ↓
                       Production
```

---

# Interview Questions

### What is a CI/CD pipeline?

An automated process that validates, builds, packages, and optionally deploys software.

### Why use matrix testing?

To test the application across multiple environments or versions without duplicating workflow definitions.

### Why use artifacts?

To preserve and transfer generated files between workflow jobs or for later inspection.

### Why use `needs`?

To control job dependencies and execution order.

### Why use `concurrency`?

To prevent unwanted simultaneous workflow runs or deployments.

### Why protect production?

Production affects real users and systems, so deployments should be controlled and verified.

### Why use minimum permissions?

To reduce the potential impact if a workflow or dependency is compromised.

---

# Final Challenge

Build your own version of this pipeline.

Requirements:

```text
[ ] Push trigger
[ ] Pull Request trigger
[ ] Manual trigger
[ ] Manual environment input
[ ] Minimal permissions
[ ] Matrix testing
[ ] Dependency installation
[ ] Linting
[ ] Testing
[ ] Build
[ ] Artifact
[ ] Job dependency
[ ] Conditional deployment
[ ] Concurrency
```

Do not simply copy the workflow.

Try changing:

```text
Node versions
Job names
Environment names
Conditions
Artifact names
```

and observe what changes in GitHub Actions.

---

# Summary

This project combines the major GitHub Actions concepts:

```text
Triggers
Conditions
Contexts
Matrix
Artifacts
Caching
Outputs
Environments
Concurrency
Security
CI/CD
```

The important idea is:

```text
Developer
   ↓
GitHub
   ↓
Automated Validation
   ↓
Build
   ↓
Artifact
   ↓
Deployment
```

This is the foundation of modern CI/CD automation.
