

## Introduction

A GitHub Actions workflow is an automated process defined in a YAML file.

Workflow files are stored in:

```text
.github/workflows/
```

Example:

```text
.github/
└── workflows/
    ├── ci.yml
    ├── tests.yml
    └── deploy.yml
```

---

# Basic Workflow Structure

A workflow generally contains:

```text
Name
 ↓
Trigger
 ↓
Jobs
 ↓
Runner
 ↓
Steps
```

Example:

```yaml
name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run tests
        run: echo "Running tests"
```

---

# Workflow Name

Use:

```yaml
name: CI
```

The name identifies the workflow in GitHub Actions.

Example:

```yaml
name: Python Tests
```

---

# Workflow Trigger

The `on` section determines when the workflow runs.

Example:

```yaml
on:
  push:
```

The workflow runs when a push event occurs.

---

# Push Trigger

Basic:

```yaml
on:
  push:
```

You can restrict it to specific branches:

```yaml
on:
  push:
    branches:
      - main
```

Now the workflow runs for pushes to `main`.

---

# Pull Request Trigger

```yaml
on:
  pull_request:
```

This allows the workflow to run when Pull Request activity occurs.

Example:

```yaml
on:
  pull_request:
    branches:
      - main
```

This targets Pull Requests involving `main`.

---

# Manual Trigger

Use:

```yaml
on:
  workflow_dispatch:
```

This allows you to manually start the workflow from GitHub.

---

# Multiple Triggers

Example:

```yaml
on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:
```

The workflow can now run for:

```text
Push to main
Pull Request targeting main
Manual execution
```

---

# Jobs

The `jobs` section contains the jobs executed by the workflow.

Example:

```yaml
jobs:
  test:
```

A workflow can contain multiple jobs.

```yaml
jobs:
  test:
    ...

  build:
    ...
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

# Job ID

Example:

```yaml
jobs:
  test:
```

`test` is the job ID.

Another example:

```yaml
jobs:
  build-project:
```

Job IDs should be clear and meaningful.

---

# Runner

Every job needs an execution environment.

Example:

```yaml
runs-on: ubuntu-latest
```

Other common hosted runner labels include:

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

  - name: Show files
    run: ls -la
```

Each step performs an operation.

---

# `uses`

Example:

```yaml
- uses: actions/checkout@v4
```

This uses an existing Action.

`actions/checkout` is commonly used to make repository files available to the runner.

---

# `run`

Example:

```yaml
- name: Show Git version
  run: git --version
```

The `run` keyword executes a command.

---

# Multiple Commands

You can execute multiple commands using a multiline block:

```yaml
- name: Check environment
  run: |
    git --version
    pwd
    ls -la
```

The `|` allows multiple lines.

---

# Step Names

Example:

```yaml
- name: Install dependencies
```

Good step names make workflow logs easier to understand.

Prefer:

```text
Install dependencies
Run unit tests
Build application
Check formatting
```

instead of:

```text
Step 1
Step 2
Step 3
```

---

# Job Dependencies

Jobs can depend on other jobs.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Testing"

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building"
```

Workflow:

```text
Test
 ↓
Build
```

The build job depends on the test job.

---

# Parallel Jobs

If there is no dependency:

```yaml
jobs:
  test:
    ...

  lint:
    ...
```

The jobs can run independently.

Conceptually:

```text
        Workflow
        /      \
       ↓        ↓
     Test      Lint
```

---

# Job-Level Environment Variables

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    env:
      APP_ENV: test

    steps:
      - run: echo "$APP_ENV"
```

The variable is available to steps in that job.

---

# Workflow-Level Environment Variables

Example:

```yaml
name: CI

on:
  push:

env:
  APP_ENV: test

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "$APP_ENV"
```

The environment variable applies broadly within the workflow.

---

# Step-Level Environment Variables

Example:

```yaml
- name: Test
  env:
    APP_ENV: test
  run: echo "$APP_ENV"
```

This limits the variable to that step.

---

# Working Directory

A command can be executed from a specific directory.

Example:

```yaml
- name: Run tests
  working-directory: backend
  run: npm test
```

This is useful for repositories containing multiple projects.

---

# Timeout

A job can have a timeout.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 10
```

This helps prevent unexpectedly long-running jobs.

---

# Conditions

Steps and jobs can use conditions.

Example:

```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: echo "Deploying"
```

The step runs only when the condition is satisfied.

---

# Workflow Example

Create:

```text
.github/workflows/basic-ci.yml
```

Use:

```yaml
name: Basic CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

jobs:
  check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Show Git version
        run: git --version

      - name: Show current directory
        run: pwd

      - name: Show repository files
        run: ls -la

      - name: Run check
        run: echo "Basic CI check passed!"
```

---

# Workflow Execution

When you push to `main`:

```text
git push
   ↓
push event
   ↓
Basic CI workflow
   ↓
check job
   ↓
runner
   ↓
steps
```

When you create a Pull Request targeting `main`:

```text
Pull Request
      ↓
Workflow Trigger
      ↓
check job
      ↓
Steps
```

---

# Workflow Logs

After a workflow runs:

```text
GitHub Repository
       ↓
Actions
       ↓
Basic CI
       ↓
Workflow Run
       ↓
check
       ↓
Step
       ↓
Logs
```

Each step's output can be inspected.

---

# Workflow Status

A workflow can have statuses such as:

```text
Queued
In Progress
Completed
```

A completed run can have a conclusion such as:

```text
Success
Failure
Cancelled
Skipped
```

---

# Workflow Files

You can have multiple workflows:

```text
.github/workflows/
│
├── ci.yml
├── tests.yml
├── security.yml
└── deploy.yml
```

Each workflow can have a different purpose.

---

# CI Workflow

Example purpose:

```text
Build
 ↓
Test
 ↓
Lint
```

File:

```text
ci.yml
```

---

# Deployment Workflow

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

File:

```text
deploy.yml
```

---

# Security Workflow

Example:

```text
Code
 ↓
Security Checks
 ↓
Report
```

File:

```text
security.yml
```

---

# Scheduled Workflow

GitHub Actions can also run workflows on a schedule.

Example:

```yaml
on:
  schedule:
    - cron: "0 0 * * *"
```

This uses a cron expression.

The exact schedule should be chosen according to the required frequency and time zone considerations.

---

# Workflow Naming Convention

Good names:

```text
CI
Python Tests
Node.js CI
Security Scan
Deploy Production
Documentation Check
```

Avoid vague names:

```text
Test123
New Workflow
Thing
ABC
```

---

# Reusable Workflow Concept

Larger repositories may avoid duplicating workflow logic by using reusable workflows.

Conceptually:

```text
Workflow A
     ↓
Reusable Workflow
     ↑
Workflow B
```

This can make large GitHub Actions configurations easier to maintain.

---

# Workflow Best Practices

Use:

```text
Clear workflow names
Clear job names
Clear step names
Small focused workflows
Pinned/controlled Action versions
Minimal permissions
Secrets for sensitive values
```

Avoid:

```text
Huge workflows
Hard-coded credentials
Unnecessary permissions
Unreviewed third-party Actions
```

---

# Common Mistakes

## Mistake 1

Putting workflows in the wrong directory.

Correct:

```text
.github/workflows/
```

---

## Mistake 2

Invalid YAML indentation.

YAML depends heavily on indentation.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
```

---

## Mistake 3

Using the wrong trigger.

If you want a workflow to run on pushes:

```yaml
on:
  push:
```

---

## Mistake 4

Forgetting the runner.

A normal job needs an execution environment:

```yaml
runs-on: ubuntu-latest
```

---

## Mistake 5

Using commands that do not exist on the selected runner.

For example, a Linux runner commonly uses:

```bash
ls
pwd
```

while Windows environments have different command-line defaults.

---

# Practice

Create:

```text
.github/workflows/workflow-practice.yml
```

Use:

```yaml
name: Workflow Practice

on:
  push:
  workflow_dispatch:

jobs:
  practice:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Check Git
        run: git --version

      - name: Check directory
        run: pwd

      - name: List files
        run: ls -la

      - name: Finish
        run: echo "Workflow practice completed!"
```

Commit:

```text
Add workflow structure practice
```

Push it.

Then open:

```text
GitHub
 ↓
Actions
 ↓
Workflow Practice
```

Inspect every step and its logs.

---

# Challenge

Create a workflow containing:

```text
1 workflow
2 jobs
3+ steps per job
1 job dependency
1 environment variable
```

Expected structure:

```text
Workflow
│
├── test
│   ├── Checkout
│   ├── Test
│   └── Finish
│
└── build
    ├── Checkout
    ├── Build
    └── Finish
```

Make `build` depend on `test`:

```yaml
needs: test
```

---

# Interview Questions

### What is a GitHub Actions workflow?

An automated process defined in a YAML file.

### Where are workflows stored?

```text
.github/workflows/
```

### What does `on` do?

Defines events that can trigger the workflow.

### What is a job?

A group of steps executed in a runner environment.

### What is `runs-on`?

It specifies the runner environment for a job.

### What is a step?

An individual operation inside a job.

### What is `uses`?

It references an existing Action.

### What is `run`?

It executes a command.

### What does `needs` do?

It creates a dependency between jobs.

### Can jobs run in parallel?

Yes, when they do not depend on each other.

### What is `workflow_dispatch`?

A trigger that allows a workflow to be manually started.

---

# Summary

Remember this structure:

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
```

Important keywords:

```text
name
on
jobs
runs-on
steps
uses
run
needs
if
env
```

The basic GitHub Actions workflow is:

```text
Event
 ↓
Workflow
 ↓
Job
 ↓
Runner
 ↓
Steps
 ↓
Result
```

Once you understand this structure, the next step is learning the different **events and triggers** that control exactly when workflows execute.
