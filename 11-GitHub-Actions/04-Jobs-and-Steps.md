# GitHub Actions Jobs and Steps

## Introduction

A GitHub Actions workflow is made up of jobs, and each job contains steps.

```text
Workflow
   ↓
  Jobs
   ↓
 Steps
```

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run tests
        run: echo "Testing"
```

---

# What Is a Job?

A job is a group of steps that execute together on a runner.

Example:

```yaml
jobs:
  test:
```

Here:

```text
test = Job ID
```

A workflow can contain multiple jobs.

---

# Multiple Jobs

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

Structure:

```text
Workflow
   │
   ├── Test
   │
   └── Build
```

---

# Job ID

Example:

```yaml
jobs:
  test:
```

The job ID is:

```text
test
```

Good IDs:

```text
test
build
lint
deploy
security
```

Use meaningful names.

---

# Job Name

You can give a job a display name:

```yaml
jobs:
  test:
    name: Run Tests
    runs-on: ubuntu-latest
```

Here:

```text
test = job ID
Run Tests = display name
```

---

# Runner

A job executes on a runner.

Example:

```yaml
runs-on: ubuntu-latest
```

Common hosted runners:

```text
ubuntu-latest
windows-latest
macos-latest
```

---

# Steps

Steps are individual operations inside a job.

Example:

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: Test
    run: echo "Testing"

  - name: Build
    run: echo "Building"
```

Execution:

```text
Checkout
   ↓
Test
   ↓
Build
```

---

# `uses` Step

Example:

```yaml
- uses: actions/checkout@v4
```

This uses an existing Action.

A named version is commonly specified after `@`.

---

# `run` Step

Example:

```yaml
- name: Show Git version
  run: git --version
```

This executes a command on the runner.

---

# Multiple Commands

Use:

```yaml
- name: System information
  run: |
    git --version
    pwd
    ls -la
```

The commands execute in sequence.

---

# Step Order

Steps in the same job normally execute sequentially.

```text
Step 1
  ↓
Step 2
  ↓
Step 3
  ↓
Step 4
```

If an earlier step fails, later steps normally do not execute unless conditions are configured otherwise.

---

# Job Dependencies

Jobs can depend on other jobs.

Use:

```yaml
needs:
```

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Tests passed"

  build:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building"
```

Execution:

```text
Test
 ↓
Build
```

---

# Multiple Dependencies

A job can depend on multiple jobs.

Example:

```yaml
jobs:
  test:
    ...

  lint:
    ...

  build:
    needs:
      - test
      - lint
    ...
```

Conceptually:

```text
Test ────┐
         ├──→ Build
Lint ────┘
```

The build job waits for the required jobs.

---

# Parallel Jobs

Jobs without dependencies can run independently.

```yaml
jobs:
  test:
    ...

  lint:
    ...
```

Conceptually:

```text
        Workflow
        /      \
       ↓        ↓
     Test      Lint
```

This can reduce total workflow time.

---

# Job Outputs

A job can produce outputs that another job can use.

Conceptually:

```text
Job A
  ↓
Output
  ↓
Job B
```

Job outputs are useful when information needs to be passed between jobs.

---

# Step Outputs

Steps can also produce outputs that later steps can consume.

Conceptually:

```text
Step 1
  ↓
Output
  ↓
Step 2
```

---

# Environment Variables

Environment variables can be defined at different levels.

Workflow level:

```yaml
env:
  APP_ENV: test
```

Job level:

```yaml
jobs:
  test:
    env:
      APP_ENV: test
```

Step level:

```yaml
- name: Test
  env:
    APP_ENV: test
  run: echo "$APP_ENV"
```

---

# Working Directory

A command can use a specific directory.

```yaml
- name: Run backend tests
  working-directory: backend
  run: npm test
```

This is useful for repositories containing multiple applications.

---

# Conditions

A step can run only when a condition is true.

Example:

```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: echo "Deploying"
```

This restricts deployment to the `main` branch reference.

---

# Conditional Jobs

Jobs can also have conditions.

Example:

```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying"
```

---

# Continue on Error

A step can be configured to continue even when it fails.

Example:

```yaml
- name: Optional check
  continue-on-error: true
  run: echo "This may fail"
```

Use this carefully.

Important checks should generally fail the workflow when they fail.

---

# Timeout

Jobs can have a timeout.

Example:

```yaml
jobs:
  test:
    timeout-minutes: 10
    runs-on: ubuntu-latest

    steps:
      - run: echo "Testing"
```

This helps prevent jobs from running indefinitely.

---

# Step Timeout

A step can also have a timeout:

```yaml
- name: Long task
  timeout-minutes: 5
  run: echo "Running task"
```

---

# Shell

A `run` step uses a shell.

Example:

```yaml
- name: Linux command
  shell: bash
  run: echo "Hello"
```

Different runners can have different default shells.

Always make sure commands match the runner and shell.

---

# Job Permissions

A workflow or job can define permissions for the GitHub token.

Example:

```yaml
permissions:
  contents: read
```

Use the minimum permissions required by the workflow.

---

# Complete Example

```yaml
name: Jobs and Steps Demo

on:
  push:
  workflow_dispatch:

jobs:
  test:
    name: Test Project
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Git version
        run: git --version

      - name: List files
        run: ls -la

      - name: Run tests
        run: echo "Tests passed!"

  build:
    name: Build Project
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: echo "Build completed!"

      - name: Finish
        run: echo "Build job completed!"
```

Workflow:

```text
Push / Manual
     ↓
   Test
     ↓
   Build
```

---

# Important Concept: Jobs Have Separate Runners

Suppose:

```text
Job A
 ↓
Job B
```

Even though B depends on A, they normally execute as separate jobs with separate runner environments.

Do not assume files created in Job A automatically exist in Job B.

For sharing files between jobs, artifacts or another appropriate storage mechanism may be required.

---

# Job vs Step

### Job

A group of steps running together on a runner.

### Step

One operation inside a job.

Example:

```text
Job: Test
│
├── Checkout
├── Install Dependencies
└── Run Tests
```

---

# Workflow vs Job vs Step

Remember:

```text
Workflow
    ↓
   Job
    ↓
  Step
```

Example:

```text
CI Workflow
    ↓
 Test Job
    ↓
 Checkout Step
 Install Step
 Test Step
```

---

# Practical CI Structure

A real CI workflow may look like:

```text
                 CI
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      Lint       Test      Security
        │         │         │
        └─────────┼─────────┘
                  ↓
                Build
```

You can control the dependency structure using `needs`.

---

# Practice

Create:

```text
.github/workflows/jobs-practice.yml
```

Use:

```yaml
name: Jobs Practice

on:
  push:
  workflow_dispatch:

jobs:
  test:
    name: Test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Check Git
        run: git --version

      - name: Run tests
        run: echo "Tests passed!"

  build:
    name: Build
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Build
        run: echo "Build completed!"

      - name: Finish
        run: echo "All done!"
```

Push it.

Observe:

```text
Test
 ↓
Build
```

---

# Practice 2: Parallel Jobs

Modify the workflow to:

```yaml
jobs:
  test:
    ...

  lint:
    ...

  build:
    needs:
      - test
      - lint
    ...
```

The structure becomes:

```text
Test ────┐
         ├──→ Build
Lint ────┘
```

---

# Practice 3: Conditional Step

Add:

```yaml
- name: Main branch message
  if: github.ref == 'refs/heads/main'
  run: echo "This is the main branch"
```

Push to `main`.

Check the Actions logs.

---

# Challenge

Create a workflow with:

```text
3 jobs
```

Use:

```text
Test
Lint
Build
```

Requirements:

```text
Test and Lint can run independently.

Build must wait for:
Test
Lint
```

Expected:

```text
       Test
        │
        ├──────┐
        │      │
        │     Build
        │      ↑
        └──────┘
        ↑
      Lint
```

More clearly:

```text
Test ────┐
         ├──→ Build
Lint ────┘
```

---

# Interview Questions

### What is a job?

A group of steps executed together on a runner.

### What is a step?

An individual operation within a job.

### Can a workflow have multiple jobs?

Yes.

### Can jobs run in parallel?

Yes, when there are no dependencies between them.

### How do you create a job dependency?

Use:

```yaml
needs:
```

### What does `runs-on` specify?

The runner environment used by a job.

### What does `uses` do?

It invokes an existing Action.

### What does `run` do?

It executes a command.

### Can jobs share files automatically?

No. Separate jobs generally use separate runner environments, so files may need to be transferred using artifacts or another appropriate mechanism.

### What is `continue-on-error`?

It allows a configured step or job to continue without making the workflow fail in the normal way when that operation fails.

### Why use job dependencies?

To control execution order and ensure prerequisites complete before dependent jobs start.

---

# Summary

Remember:

```text
Workflow
   ↓
 Jobs
   ↓
Steps
```

Important job features:

```text
name
runs-on
needs
if
env
permissions
timeout-minutes
```

Important step features:

```text
name
uses
run
if
env
working-directory
shell
continue-on-error
timeout-minutes
```

The most important pattern:

```text
Test ────┐
         ├──→ Build ──→ Deploy
Lint ────┘
```

GitHub Actions becomes powerful when you combine jobs, steps, dependencies, and conditions to create reliable automation.
