#Advanced GitHub Actions

## Introduction

After  learning the fundamentals of GitHub Actions, we can combine them to build more advanced workflows.

Important concepts include:
```text
Contexts
Outputs
Artifacts
Matrices
Reusable Workflows
Environments
Concurrency
Manual Inputs
Dynamic Workflows
```

---

# Workflow Inputs

A workflow triggered manually with `workflow_dispatch` can accept inputs.

Example:

```yaml
name: Manual Deployment

on:
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

The user can select an environment when starting the workflow.

---

# Using Workflow Inputs

Inputs can be accessed using:

```yaml
${{ inputs.environment }}
```

Example:

```yaml
- name: Show Environment
  run: echo "Deploying to ${{ inputs.environment }}"
```

Flow:

```text
Run Workflow
     ↓
Select Environment
     ↓
Workflow Starts
     ↓
Use Input
```

---

# Boolean Inputs

Example:

```yaml
on:
  workflow_dispatch:
    inputs:
      run-tests:
        description: "Run tests?"
        required: true
        type: boolean
        default: true
```

Then:

```yaml
if: ${{ inputs.run-tests }}
```

The step runs only when the input is true.

---

# Choice Inputs

Example:

```yaml
type: choice
options:
  - development
  - staging
  - production
```

This prevents users from entering arbitrary values.

---

# Environment Inputs

Example:

```yaml
name: Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice
        required: true
        options:
          - staging
          - production

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying to ${{ inputs.environment }}"
```

---


#  Job Outputs

Outputs allow one job to provide information to another job.

Example:

```yaml
jobs:
  generate:
    runs-on: ubuntu-latest

    outputs:
      version: ${{ steps.version.outputs.version }}

    steps:
      - name: Generate Version
        id: version
        run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"
```

Another job can use:

```yaml
${{ needs.generate.outputs.version }}
```

---

# Passing Data Between Jobs

Conceptually:

```text
Job A
  ↓
Generate Output
  ↓
Job Output
  ↓
needs
  ↓
Job B
```

Example:

```yaml
jobs:
  generate:
    runs-on: ubuntu-latest

    outputs:
      message: ${{ steps.output.outputs.message }}

    steps:
      - id: output
        run: echo "message=Hello" >> "$GITHUB_OUTPUT"

  use:
    needs: generate
    runs-on: ubuntu-latest

    steps:
      - run: echo "${{ needs.generate.outputs.message }}"
```

---

# Concurrency

Concurrency prevents multiple workflow runs or jobs from running simultaneously when that is undesirable.

Example:

```yaml
concurrency:
  group: production-deployment
```

This creates a concurrency group.

---

# Why Use Concurrency?

Imagine:

```text
Deployment A
     ↓
Production
```

and immediately afterward:

```text
Deployment B
     ↓
Production
```

Running both simultaneously could cause problems.

Concurrency can help ensure that only the appropriate deployment runs at a time.

---

# Canceling Previous Runs

Example:

```yaml
concurrency:
  group: production-deployment
  cancel-in-progress: true
```

A newer run can cancel an older in-progress run in the same concurrency group.

Conceptually:

```text
Deployment A
     ↓
Running
     ↓
Deployment B starts
     ↓
A cancelled
     ↓
B continues
```

Use this carefully for deployment workflows.

---

# Concurrency by Branch

Example:

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
```

This creates separate concurrency groups based on workflow and Git reference.

---

# Environment Variables

Environment variables can be defined at different levels.

Workflow-level:

```yaml
env:
  APP_NAME: my-app
```

Job-level:

```yaml
jobs:
  build:
    env:
      MODE: production
```

Step-level:

```yaml
- name: Build
  env:
    DEBUG: true
  run: echo "Building"
```

---

# Scope

Conceptually:

```text
Workflow
   ↓
Job
   ↓
Step
```

A variable defined at a higher level can generally be available to lower levels unless overridden.

---

# Dynamic Configuration

Contexts can be combined with environment variables.

Example:

```yaml
env:
  BRANCH_NAME: ${{ github.ref_name }}
```

Then:

```yaml
- run: echo "Branch: $BRANCH_NAME"
```

This allows workflows to adapt to runtime information.

---

# Conditional Deployment

A production deployment can require:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

Combined with a manual input:

```yaml
if: ${{ inputs.environment == 'production' && github.ref == 'refs/heads/main' }}
```

This can provide an additional restriction.

---

# Reusable Workflows + Inputs

Reusable workflows can receive configuration.

Example:

```yaml
on:
  workflow_call:
    inputs:
      node-version:
        required: true
        type: string
```

Caller:

```yaml
jobs:
  test:
    uses: ./.github/workflows/test.yml
    with:
      node-version: "20"
```

The reusable workflow can then use:

```yaml
${{ inputs.node-version }}
```

---

# Reusable Workflows + Secrets

A reusable workflow can accept secrets.

Example:

```yaml
on:
  workflow_call:
    secrets:
      DEPLOY_TOKEN:
        required: true
```

Caller:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/deploy.yml
    secrets:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

---

# Matrix + Reusable Workflows

Reusable workflows and matrix strategies can be combined.

Conceptually:

```text
Calling Workflow
       ↓
Matrix
 ┌─────┼─────┐
 ↓     ↓     ↓
18    20    22
 ↓     ↓     ↓
Reusable Test Workflow
```

This can reduce duplicated workflow code.

---

# Artifacts + Multiple Jobs

A larger pipeline can use artifacts to move build output between jobs.

```text
Build
  ↓
Artifact
  ↓
Test
  ↓
Artifact
  ↓
Deploy
```

This is useful when jobs run on separate runners.

---

# Deployment Environments

GitHub Actions environments can represent:

```text
development
staging
production
```

Production can have additional protection rules.

Conceptually:

```text
Build
 ↓
Test
 ↓
Staging
 ↓
Approval
 ↓
Production
```

---

# Environment Secrets

Environment-specific secrets allow different credentials for different environments.

Conceptually:

```text
Development
   ↓
DEV_SECRET

Staging
   ↓
STAGING_SECRET

Production
   ↓
PRODUCTION_SECRET
```

This is safer than using the same credential everywhere.

---

# Manual Production Deployment

A practical pattern is:

```text
Push
 ↓
Automatic CI
 ↓
Tests
 ↓
Build
 ↓
Manual Deployment
 ↓
Production
```

This provides automation while keeping production deployment controlled.

---

# Complete Manual Deployment Example

```yaml
name: Manual Deployment

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Deployment environment"
        required: true
        type: choice
        options:
          - staging
          - production

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Show Environment
        run: echo "Deploying to ${{ inputs.environment }}"

      - name: Deploy
        run: echo "Deployment completed"
```

---

# Dynamic Job Names

Job names can use expressions.

Example:

```yaml
jobs:
  deploy:
    name: Deploy to ${{ inputs.environment }}
```

The Actions interface can then show:

```text
Deploy to staging
```

or:

```text
Deploy to production
```

---

# Dynamic Matrix

A matrix can be used to test multiple configurations.

Example:

```yaml
strategy:
  matrix:
    node-version:
      - "18"
      - "20"
      - "22"
```

Then:

```yaml
name: Node ${{ matrix.node-version }}
```

This makes each job easier to identify.

---

# Debugging Workflows

When a workflow fails, inspect:

```text
Workflow Run
 ↓
Job
 ↓
Step
 ↓
Log
 ↓
Error Message
```

Start with the first meaningful error instead of reading every line.

---

# Debugging Strategy

Use:

```text
1. Identify failed job
2. Identify failed step
3. Read error
4. Check inputs
5. Check environment
6. Check permissions
7. Re-run after fixing
```

---

# Re-running Failed Jobs

GitHub Actions allows workflow runs to be re-run.

This is useful when:

```text
Temporary Failure
Network Problem
Runner Problem
External Service Issue
```

However, don't repeatedly re-run a deterministic failure without investigating it.

---

# Logs

Logs can reveal:

```text
Command Failures
Dependency Errors
Environment Problems
Permission Errors
Test Failures
```

Avoid printing:

```text
Secrets
Tokens
Passwords
Private Credentials
```

---

# Workflow Commands

GitHub Actions provides special workflow commands through stdout.

For example, a step can write outputs using:

```bash
echo "name=value" >> "$GITHUB_OUTPUT"
```

Environment variables can be written using:

```bash
echo "NAME=value" >> "$GITHUB_ENV"
```

---

# Step Outputs

Example:

```yaml
- name: Generate Version
  id: version
  run: echo "value=1.0.0" >> "$GITHUB_OUTPUT"

- name: Show Version
  run: echo "${{ steps.version.outputs.value }}"
```

The `id` connects the output to the later step.

---

# Environment Variables Through `$GITHUB_ENV`

Example:

```yaml
- name: Set Environment Variable
  run: echo "APP_MODE=production" >> "$GITHUB_ENV"

- name: Use Variable
  run: echo "$APP_MODE"
```

The second step can access the variable.

---

# Shell Selection

GitHub Actions can run commands using different shells.

Example:

```yaml
- name: Bash
  shell: bash
  run: echo "Hello"
```

On Windows, PowerShell can be used:

```yaml
- name: PowerShell
  shell: pwsh
  run: Write-Host "Hello"
```

---

# Cross-Platform Scripts

Be careful when using shell-specific commands.

For example:

```text
Bash
PowerShell
Windows CMD
```

may have different syntax.

A workflow intended for multiple operating systems should account for these differences.

---

# Advanced Pipeline Architecture

A mature pipeline can look like:

```text
                 Push / PR
                     ↓
                  CI Start
                     ↓
              ┌──────┴──────┐
              ↓             ↓
            Lint          Security
              ↓             ↓
              └──────┬──────┘
                     ↓
                   Test
                     ↓
                   Build
                     ↓
                 Artifact
                     ↓
                  Staging
                     ↓
                Verification
                     ↓
              Production Approval
                     ↓
                 Production
```

---

# Reliability

A good workflow should be:

```text
Repeatable
Predictable
Observable
Secure
Maintainable
```

Avoid creating workflows that only work on one developer's machine.

---

# Performance

Ways to improve workflow performance include:

```text
Caching
Parallel Jobs
Matrix Optimization
Smaller Build Steps
Reusable Workflows
Avoiding Unnecessary Work
```

For example:

```text
Lint ─────┐
Test ─────┼──→ Build
Security ─┘
```

can be faster than:

```text
Lint
 ↓
Test
 ↓
Security
 ↓
Build
```

when the jobs are independent.

---

# Advanced Security Reminder

Advanced workflows can have more privileges.

Before adding:

```text
Secrets
Write Permissions
Production Access
Self-Hosted Runners
External Actions
```

ask:

```text
Does this workflow really need it?
```

---

# Best Practices

- Keep workflows readable.
- Use reusable workflows for repeated logic.
- Use matrix strategies for compatibility testing.
- Use artifacts for build outputs.
- Use caches for reusable dependencies.
- Use environments for deployment stages.
- Protect production.
- Use concurrency where necessary.
- Minimize permissions.
- Never expose secrets.
- Validate untrusted input.
- Keep Actions and dependencies maintained.

---

# Interview Questions

### What is concurrency?

A mechanism for controlling which workflow runs or jobs can execute simultaneously.

### What is `workflow_dispatch`?

A workflow trigger that allows a workflow to be manually started.

### What is `GITHUB_OUTPUT`?

A mechanism used by a step to expose output values to later steps or jobs.

### What is `GITHUB_ENV`?

A mechanism used to set environment variables for subsequent steps.

### Why use reusable workflows?

To centralize and reuse workflow-level automation.

### Why use matrix strategies?

To test multiple configurations without duplicating job definitions.

### Why use environments?

To separate deployment targets, secrets, and protection rules.

---

# Practice

Create:

```text
.github/workflows/advanced-practice.yml
```

Paste:

```yaml
name: Advanced GitHub Actions Practice

on:
  workflow_dispatch:
    inputs:
      environment:
        description: "Select environment"
        required: true
        type: choice
        options:
          - staging
          - production

      run-tests:
        description: "Run tests"
        required: true
        type: boolean
        default: true

concurrency:
  group: advanced-practice
  cancel-in-progress: false

permissions:
  contents: read

jobs:
  test:
    name: Test
    if: ${{ inputs.run-tests }}
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "Tests passed"

      - name: Generate Version
        id: version
        run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"

      - name: Show Version
        run: echo "Version ${{ steps.version.outputs.version }}"

  deploy:
    name: Deploy to ${{ inputs.environment }}
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying to ${{ inputs.environment }}"
```

Commit message:

```text
Add advanced GitHub Actions concepts guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Advanced GitHub Actions Practice
   ↓
Run workflow
```

Try:

```text
Environment → staging
Run tests → true
```

Then run it again with:

```text
Environment → production
Run tests → true
```

Observe the workflow behavior.

---

# Summary

Advanced GitHub Actions combines the concepts learned throughout this section:

```text
Triggers
 ↓
Conditions
 ↓
Contexts
 ↓
Outputs
 ↓
Artifacts
 ↓
Caching
 ↓
Matrix
 ↓
Reusable Workflows
 ↓
Environments
 ↓
Concurrency
 ↓
Security
 ↓
CI/CD
```

The goal is to build workflows that are:

```text
Automated
Secure
Fast
Reliable
Maintainable
```
