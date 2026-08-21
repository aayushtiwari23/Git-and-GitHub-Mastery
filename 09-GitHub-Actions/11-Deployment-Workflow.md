# GitHub Actions Deployment Workflow

## Introduction

Deployment means moving an application from development into an environment where users can access it.

A typical CI/CD flow is:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Deploy
 ↓
Application Running
```

GitHub Actions can automate this entire process.

---

# What is Deployment?

Deployment is the process of releasing an application to an environment.

Common environments are:

```text
Development
Staging
Production
```

Conceptually:

```text
Developer
   ↓
GitHub
   ↓
CI
   ↓
Build
   ↓
Test
   ↓
Deployment
   ↓
Production
```

---

# Development Environment

Development is where new features are created and tested.

Example:

```text
Developer
   ↓
Code Changes
   ↓
Development Environment
```

---

# Staging Environment

Staging is usually used as a production-like testing environment.

Example:

```text
Development
     ↓
   Staging
     ↓
 Production
```

Staging helps identify problems before production deployment.

---

# Production Environment

Production is the environment used by real users.

Because production is important, deployments should generally include:

```text
Testing
Validation
Permissions
Approvals
Monitoring
```

---

# Basic Deployment Workflow

A simple workflow might look like:

```text
Push Code
   ↓
Build
   ↓
Test
   ↓
Deploy
```

Example:

```yaml
name: Deployment

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: echo "Building application"

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying application"
```

---

# Why Use `needs`?

The deployment job should normally wait for the build job.

```yaml
deploy:
  needs: build
```

This creates:

```text
Build
  ↓
Deploy
```

If the build fails, deployment should not proceed.

---

# Build-Test-Deploy

A better pipeline is:

```text
Build
  ↓
Test
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

  deploy:
    needs: test
    ...
```

This creates dependencies between stages.

---

# Complete Example

```yaml
name: CI CD

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: echo "Building application"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "Running tests"

  deploy:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying application"
```

Flow:

```text
Push to main
     ↓
   Build
     ↓
   Test
     ↓
  Deploy
```

---

# Deployment Conditions

You usually don't want every branch to deploy to production.

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

This allows deployment only from the `main` branch.

---

# Environment Protection

GitHub environments can be used to protect deployments.

Example:

```yaml
environment:
  name: production
```

A production environment can have additional protection rules.

Conceptually:

```text
Deploy Job
    ↓
Production Environment
    ↓
Approval / Protection
    ↓
Deployment
```

---

# Environment Example

```yaml
deploy:
  needs: test
  runs-on: ubuntu-latest

  environment:
    name: production

  steps:
    - name: Deploy
      run: echo "Deploying to production"
```

---

# Environment Secrets

Production credentials can be associated with the production environment.

Conceptually:

```text
Production Environment
       ↓
Production Secrets
       ↓
Deployment Job
```

This helps keep deployment credentials separate from development credentials.

---

# Deployment Secrets

Example:

```yaml
- name: Deploy
  env:
    DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
  run: echo "Deployment command would run here"
```

Never hardcode the deployment token.

---

# Deployment Commands

The actual deployment command depends on the platform.

Examples include:

```text
Cloud Platform
Container Registry
Virtual Machine
Server
Static Hosting
Kubernetes
```

GitHub Actions provides the automation layer, while the deployment command or platform handles the actual release.

---

# Example: Static Website Deployment

Conceptually:

```text
HTML/CSS/JS
    ↓
Build
    ↓
Upload
    ↓
Hosting Platform
    ↓
Website Live
```

The exact deployment Action depends on the hosting provider.

---

# Example: Docker Deployment

A typical container workflow can look like:

```text
Source Code
    ↓
Build Docker Image
    ↓
Test Image
    ↓
Push Image
    ↓
Deploy Container
```

Example build command:

```bash
docker build -t my-app .
```

The image can then be pushed to an appropriate container registry.

---

# Example: Application Deployment

A generalized deployment workflow:

```yaml
name: Application Deployment

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: echo "Build application"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Test
        run: echo "Run tests"

  deploy:
    needs: test
    runs-on: ubuntu-latest

    environment:
      name: production

    steps:
      - name: Deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: echo "Deploy application"
```

---

# Continuous Deployment

Continuous Deployment means successful changes can automatically be deployed.

Conceptually:

```text
Code Push
   ↓
Build
   ↓
Tests
   ↓
Automatic Deployment
```

---

# Continuous Delivery

Continuous Delivery keeps software ready for deployment but may require a manual approval before production release.

Conceptually:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Ready for Deployment
 ↓
Manual Approval
 ↓
Production
```

---

# Continuous Integration vs Deployment

### Continuous Integration

Focuses on:

```text
Build
Test
Validate
```

### Continuous Delivery / Deployment

Focuses on:

```text
Release
Deploy
Deliver
```

A complete CI/CD pipeline can combine both.

---

# Deployment Pipeline

A professional pipeline might look like:

```text
Developer Push
      ↓
Checkout
      ↓
Install Dependencies
      ↓
Build
      ↓
Unit Tests
      ↓
Security Checks
      ↓
Package
      ↓
Staging
      ↓
Approval
      ↓
Production
```

---

# Staging Deployment

You can create a staging environment before production.

Example:

```text
Build
 ↓
Test
 ↓
Staging
 ↓
Validation
 ↓
Production
```

This reduces the chance of deploying an untested application directly to production.

---

# Production Approval

A production deployment can be protected by requiring approval.

Conceptually:

```text
Tests Passed
     ↓
Production Deployment
     ↓
Approval Required
     ↓
Approved
     ↓
Deploy
```

This is especially useful for important applications.

---

# Rollback

A deployment can sometimes introduce problems.

Rollback means returning to a previous known-good version.

Conceptually:

```text
Version 1
   ↓
Version 2
   ↓
Problem
   ↓
Rollback
   ↓
Version 1
```

---

# Why Rollback Matters

Suppose:

```text
Production
   ↓
New Release
   ↓
Application Error
```

A rollback mechanism allows the team to restore a stable version quickly.

---

# Deployment Strategies

Common deployment strategies include:

```text
Rolling Deployment
Blue-Green Deployment
Canary Deployment
Recreate Deployment
```

---

# Rolling Deployment

Instances are updated gradually.

Conceptually:

```text
Old Old Old Old
     ↓
New Old Old Old
     ↓
New New Old Old
     ↓
New New New New
```

This can reduce downtime.

---

# Blue-Green Deployment

Two environments are maintained:

```text
Blue  → Current Production
Green → New Version
```

After validation:

```text
Traffic
   ↓
Blue

Switch

Traffic
   ↓
Green
```

If necessary, traffic can be switched back.

---

# Canary Deployment

Only a small percentage of users receive the new version initially.

Example:

```text
Users
 ↓
95% → Old Version
5%  → New Version
```

If everything works:

```text
50% → New
50% → Old
```

Eventually:

```text
100% → New Version
```

---

# Deployment Security

Deployment workflows can access sensitive systems.

Therefore:

```text
Minimum Permissions
        ↓
Protected Environment
        ↓
Secure Secrets
        ↓
Trusted Actions
        ↓
Controlled Deployment
```

---

# Common Deployment Mistakes

Avoid:

```text
Deploying every branch to production
Hardcoding credentials
Skipping tests
Using excessive permissions
Using untrusted Actions
Deploying without rollback planning
Giving unnecessary secrets to jobs
```

---

# Best Practices

- Build before deploying.
- Test before deploying.
- Restrict production deployments.
- Use protected environments.
- Store credentials as secrets.
- Use minimum permissions.
- Keep deployment workflows simple.
- Have a rollback strategy.
- Test deployments in staging when appropriate.
- Review third-party Actions.

---

# Interview Questions

### What is deployment?

Deployment is the process of releasing an application to an environment where it can run and be used.

### What is CI/CD?

CI/CD is a set of practices for automatically building, testing, delivering, and deploying software.

### Why use `needs`?

`needs` creates dependencies between jobs.

### What is a production environment?

The environment where the application is available to real users.

### What is rollback?

Rollback means returning to a previous stable version after a problematic deployment.

### What is continuous deployment?

Continuous deployment automatically releases successfully validated changes to the target environment.

### What is the difference between continuous delivery and continuous deployment?

Continuous delivery keeps software ready for release, often with a manual production approval. Continuous deployment automatically releases validated changes.

### Why use GitHub environments?

They can help separate deployment environments and provide controls such as environment-specific secrets and deployment protection.

---

# Practice

Create:

```text
.github/workflows/deployment-practice.yml
```

Paste:

```yaml
name: Deployment Practice

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Build completed"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Test
        run: echo "Tests passed"

  deploy:
    needs: test
    runs-on: ubuntu-latest

    environment:
      name: production

    steps:
      - name: Deploy
        run: echo "Deployment completed"
```

Commit message:

```text
Add GitHub Actions deployment workflow guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Deployment Practice
   ↓
Run workflow
```

Check the job order:

```text
Build
  ↓
Test
  ↓
Deploy
```

---
