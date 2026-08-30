# GitHub Actions Basics

## Introduction

GitHub Actions is a GitHub automation platform used to automatically perform tasks when events occur in a repository.

It can automate:

```text
Testing
Building
Linting
Deployment
Security Checks
Releases
```

A simple workflow:

```text
Developer
    ↓
Push Code
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Run Workflow
    ↓
Tests
    ↓
Build
    ↓
Result
```

---

# Why GitHub Actions?

Without automation:

```text
Write Code
   ↓
Manually Run Tests
   ↓
Manually Build
   ↓
Manually Deploy
```

With GitHub Actions:

```text
Push Code
   ↓
Automatic Workflow
   ↓
Tests
   ↓
Build
   ↓
Deploy
```

This saves time and reduces repetitive work.

---

# CI/CD

GitHub Actions is commonly used for:

```text
CI
CD
```

CI means:

```text
Continuous Integration
```

CD can mean:

```text
Continuous Delivery
```

or:

```text
Continuous Deployment
```

---

# Continuous Integration

Continuous Integration means frequently integrating code changes and automatically checking them.

Example:

```text
Developer A ─┐
Developer B ─┼→ Repository
Developer C ─┘
                  ↓
              CI Workflow
                  ↓
                 Test
                  ↓
                Result
```

---

# Continuous Delivery

Continuous Delivery means keeping software in a state where it can be released reliably.

A simplified workflow:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Ready for Release
```

---

# Continuous Deployment

Continuous Deployment automatically deploys changes after required checks succeed.


Example:

```text
Push
 ↓
Test
 ↓
Build
 ↓
Deploy
```

---

# GitHub Actions Workflow

A workflow is an automated process defined in a YAML file.

Typical location:

```text
.github/workflows/
```

Example:

```text
.github/
└── workflows/
    └── ci.yml
```

---

# YAML

GitHub Actions workflows are written using YAML.

Example:

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run tests
        run: echo "Running tests"
```

---

# Workflow

A workflow is the complete automation definition.

Example:

```text
CI Workflow
    ↓
Job
    ↓
Steps
```

A repository can contain multiple workflows.

---

# Workflow File

Workflow files are stored inside:

```text
.github/workflows/
```

Examples:

```text
.github/workflows/ci.yml
.github/workflows/tests.yml
.github/workflows/deploy.yml
```

---

# Workflow Name

Example:

```yaml
name: CI
```

The name appears in the GitHub Actions interface.

---

# Events

The `on` section defines when a workflow should run.

Example:

```yaml
on:
  push:
```

This means the workflow can run when code is pushed.

---

# Pull Request Event

Example:

```yaml
on:
  pull_request:
```

This runs when relevant Pull Request activity occurs.

This is useful for testing code before it is merged.

---

# Multiple Events

Example:

```yaml
on:
  push:
  pull_request:
```

The workflow can run for both events.

---

# Manual Workflow

A workflow can also be configured for manual execution using:

```yaml
on:
  workflow_dispatch:
```

This allows you to start it manually from GitHub.

---

# Jobs

A workflow contains one or more jobs.

Example:

```yaml
jobs:
  test:
```

Conceptually:

```text
Workflow
   ↓
Jobs
```

---

# Job

A job is a collection of steps that execute together on a runner.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
```

---

# Runner

A runner is the environment where a GitHub Actions job executes.

Common hosted runner labels include:

```text
ubuntu-latest
windows-latest
macos-latest
```

---

# Steps

A job contains steps.

Example:

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: Run tests
    run: echo "Testing"
```

Conceptually:

```text
Job
 ↓
Step 1
 ↓
Step 2
 ↓
Step 3
```

---

# `uses`

The `uses` keyword runs an existing GitHub Action.

Example:

```yaml
- uses: actions/checkout@v4
```

This action checks out repository code into the runner.

---

# `run`

The `run` keyword executes a command.

Example:

```yaml
- name: Show files
  run: ls
```

On Linux, this runs:

```bash
ls
```

---

# Step Name

Example:

```yaml
- name: Run tests
```

The name helps identify the step in the Actions interface.

---

# Basic Workflow

Create:

```text
.github/workflows/hello.yml
```

Use:

```yaml
name: Hello GitHub Actions

on:
  push:
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Say hello
        run: echo "Hello from GitHub Actions!"
```

---

# Workflow Structure

The basic structure is:

```text
Workflow
│
├── name
├── on
│
└── jobs
    │
    └── job
        │
        ├── runs-on
        │
        └── steps
            ├── uses
            ├── run
            └── run
```

---

# Trigger Flow

For a push-triggered workflow:

```text
git push
   ↓
GitHub Repository
   ↓
Event Detected
   ↓
Workflow Starts
   ↓
Job Starts
   ↓
Steps Execute
```

---

# Successful Workflow

If all steps succeed:

```text
Step 1 ✓
Step 2 ✓
Step 3 ✓
   ↓
Job Passed
   ↓
Workflow Passed
```

---

# Failed Workflow

If a step fails:

```text
Step 1 ✓
Step 2 ✗
Step 3
  ↓
Job Failed
  ↓
Workflow Failed
```

Later steps generally do not execute unless configured to continue or run under a condition.

---

# Workflow Status

GitHub can show statuses such as:

```text
Queued
In Progress
Success
Failure
Cancelled
Skipped
```

---

# CI Example

A Python project might use:

```yaml
name: Python CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest
```

The exact Python version and project commands should match the project.

---

# JavaScript Example

A Node.js project might use:

```yaml
name: Node CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Node
        uses: actions/setup-node@v4
        with:
          node-version: "22"

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

---

# Why Test on Pull Requests?

Suppose a developer creates:

```text
feature/login
```

and opens a Pull Request.

GitHub Actions can automatically run:

```text
Build
Tests
Lint
Security Checks
```

before merging.

Workflow:

```text
Pull Request
      ↓
GitHub Actions
      ↓
Tests
      ↓
Pass / Fail
```

---

# CI and Branch Protection

A repository can require CI checks before merging.

Example:

```text
Pull Request
     ↓
CI
     ↓
Tests ✓
Build ✓
Lint ✓
     ↓
Merge Allowed
```

If tests fail:

```text
Tests ✗
   ↓
Merge Blocked
```

if the repository requires that check.

---

# Multiple Jobs

A workflow can have multiple jobs.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing"

  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building"
```

Conceptually:

```text
Workflow
   │
   ├── Test Job
   │
   └── Build Job
```

---

# Jobs Can Run in Parallel

If jobs do not depend on each other:

```text
        Workflow
        /      \
       ↓        ↓
     Test     Build
```

They can run independently.

---

# Job Dependencies

A job can depend on another job using:

```yaml
needs:
```

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing"

  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying"
```

Workflow:

```text
Test
 ↓
Deploy
```

If the required job fails, the dependent job normally does not proceed.

---

# Actions Marketplace

GitHub provides a marketplace containing reusable Actions.

Examples of common tasks:

```text
Checkout Code
Set Up Languages
Run Linters
Build Projects
Deploy Applications
```

Always review third-party Actions carefully before using them.

---

# Versioning Actions

An Action reference may look like:

```yaml
uses: actions/checkout@v4
```

The `@v4` identifies the referenced version/tag.

Pinning Actions to appropriate versions is important for predictable workflows.

For higher-security environments, organizations may use stricter pinning practices.

---

# GitHub Actions Logs

When a workflow runs, GitHub provides logs.

Example:

```text
Workflow
 ↓
Job
 ↓
Step
 ↓
Logs
```

Logs help diagnose failures.

---

# Debugging a Failed Workflow

When CI fails:

```text
Open Actions
     ↓
Select Workflow
     ↓
Select Failed Run
     ↓
Open Failed Job
     ↓
Find Failed Step
     ↓
Read Logs
     ↓
Fix Problem
     ↓
Push Again
```

---

# Common CI Failures

Examples:

```text
Dependency Installation Failed
Tests Failed
Build Failed
Syntax Error
Lint Error
Wrong Runtime Version
Missing Environment Variable
Permission Problem
```

Do not immediately rerun without understanding the failure.

---

# Re-running a Workflow

Sometimes a failure is temporary.

GitHub can provide options to rerun workflows.

But if the same code consistently fails:

```text
Rerun
Rerun
Rerun
```

will not fix the underlying problem.

Investigate the logs.

---

# Workflow Security

GitHub Actions can access repository resources, so workflows should be treated as code.

Avoid:

```text
Untrusted Scripts
Unreviewed Actions
Exposing Secrets
Unsafe Pull Request Workflows
```

Review workflow changes carefully.

---

# Secrets

Sensitive values should not be hard-coded.

Bad:

```yaml
run: echo "my-secret-password"
```

Instead, use GitHub's supported secret-management mechanisms.

Conceptually:

```text
Secret
  ↓
GitHub
  ↓
Workflow
  ↓
Application
```

Never commit actual credentials to the repository.

---

# Environment Variables

Workflows can use environment variables.

Example:

```yaml
env:
  APP_ENV: production
```

Then a step can access:

```bash
echo "$APP_ENV"
```

The exact syntax depends on the runner's shell and operating system.

---

# Environment

An environment can represent deployment contexts such as:

```text
development
staging
production
```

A workflow can use environments to manage deployment-related controls and secrets.

---

# CI/CD Pipeline

A simple pipeline:

```text
Code Push
    ↓
Checkout
    ↓
Install Dependencies
    ↓
Lint
    ↓
Test
    ↓
Build
    ↓
Deploy
```

This is a foundation for automated software delivery.

---

# Practice

Create:

```text
.github/workflows/hello.yml
```

Paste:

```yaml
name: Hello GitHub Actions

on:
  push:
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Show repository files
        run: ls -la

      - name: Print message
        run: echo "GitHub Actions is working!"
```

Commit:

```text
Add first GitHub Actions workflow
```

Push it to GitHub.

---

# Run the Workflow

After pushing:

```text
Repository
   ↓
Actions
   ↓
Hello GitHub Actions
   ↓
Workflow Run
```

Open the run and inspect:

```text
Jobs
Steps
Logs
```

You should see:

```text
✓ Checkout repository
✓ Show repository files
✓ Print message
```

---

# Practice 2

Modify the workflow:

```yaml
- name: Show Git version
  run: git --version
```

Then commit:

```text
Add Git version check to workflow
```

Push again.

GitHub Actions should automatically execute the workflow again because of the `push` trigger.

---

# Practice 3

Add:

```yaml
- name: Show current directory
  run: pwd
```

Then:

```yaml
- name: Show files
  run: ls -la
```

Observe the logs.

The goal is to understand that commands are executing on the runner.

---

# Challenge

Create:

```text
.github/workflows/ci.yml
```

Use:

```yaml
name: Basic CI

on:
  push:
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Show Git version
        run: git --version

      - name: Show repository files
        run: ls -la

      - name: Run test command
        run: echo "Tests would run here"
```

Create a Pull Request and observe:

```text
PR
 ↓
CI
 ↓
Check
 ↓
Pass
```

---

# Interview Questions

### What is GitHub Actions?

A GitHub automation platform used to automate tasks such as testing, building, deployment, and other repository workflows.

### What is a workflow?

An automated process defined in a GitHub Actions YAML file.

### Where are workflow files stored?

```text
.github/workflows/
```

### What is a job?

A group of steps executed together on a runner.

### What is a runner?

The environment where a GitHub Actions job executes.

### What is a step?

An individual operation inside a job.

### What does `uses` do?

It references a reusable GitHub Action.

### What does `run` do?

It executes a command on the runner.

### What does `on` define?

The events that trigger a workflow.

### What is CI?

Continuous Integration, where code changes are integrated frequently and automatically checked.

### What is CD?

Continuous Delivery or Continuous Deployment, depending on the context.

---

# Summary

Remember:

```text
.github/workflows/
        ↓
    Workflow
        ↓
       Jobs
        ↓
      Runner
        ↓
      Steps
        ↓
      Result
```

And the basic CI/CD idea:

```text
Push / Pull Request
        ↓
GitHub Actions
