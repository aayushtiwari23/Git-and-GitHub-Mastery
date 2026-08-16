
# GitHub Actions Jobs, Steps and Runners

## Introduction

Three of the most important concepts in GitHub Actions are:

```text
Jobs
Steps
Runners
```

Understanding how these three work together is essential for creating workflows.

---

# Basic Structure

The relationship is:

```text
Workflow
   │
   ├── Job
   │     │
   │     ├── Step
   │     ├── Step
   │     └── Step
   │
   └── Job
         │
         ├── Step
         └── Step
```

A workflow can contain multiple jobs.

Each job contains one or more steps.

Each job runs on a runner.

---

# What is a Job?

A job is a collection of steps that are executed together.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Install Dependencies
        run: echo "Installing"

      - name: Run Tests
        run: echo "Testing"
```

Here:

```text
test
```

is the job ID.

The job contains two steps.

---

# Job ID

Example:

```yaml
jobs:
  build:
```

The word:

```text
build
```

is the job ID.

Use clear names such as:

```text
build
test
lint
security
deploy
```

Avoid meaningless names such as:

```text
job1
abc
thing
```

---

# What is a Step?

A step is an individual task inside a job.

Example:

```yaml
steps:
  - name: Print Message
    run: echo "Hello"

  - name: Show Files
    run: ls
```

There are two steps:

```text
Step 1 → Print Message
Step 2 → Show Files
```

Steps normally execute sequentially within the same job.

---

# `run` Step

A step can execute a shell command using `run`.

Example:

```yaml
- name: Show Python Version
  run: python --version
```

Another:

```yaml
- name: Show Directory
  run: pwd
```

---

# `uses` Step

A step can use an existing Action.

Example:

```yaml
- name: Checkout Repository
  uses: actions/checkout@v4
```

The Action provides reusable automation.

---

# Job with Multiple Steps

Example:

```yaml
jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Step One
        run: echo "First"

      - name: Step Two
        run: echo "Second"

      - name: Step Three
        run: echo "Third"
```

Execution:

```text
Job Starts
    ↓
Step One
    ↓
Step Two
    ↓
Step Three
    ↓
Job Finishes
```

---

# What is a Runner?

A runner is the machine or environment that executes a job.

Conceptually:

```text
GitHub
   ↓
Workflow
   ↓
Job
   ↓
Runner
   ↓
Steps
```

The runner provides the environment needed to execute commands.

---

# GitHub-Hosted Runners

GitHub provides hosted runners for common operating systems.

Examples include:

```text
Ubuntu
Windows
macOS
```

Example:

```yaml
runs-on: ubuntu-latest
```

This requests a GitHub-hosted Ubuntu runner.

---

# Ubuntu Runner

Example:

```yaml
runs-on: ubuntu-latest
```

Ubuntu runners are commonly used for:

- Linux applications
- Python projects
- Node.js projects
- Java projects
- C/C++ projects
- General CI workflows

---

# Windows Runner

Example:

```yaml
runs-on: windows-latest
```

Useful when your project requires a Windows environment.

Examples:

```text
.NET
Windows-specific applications
PowerShell workflows
Windows testing
```

---

# macOS Runner

Example:

```yaml
runs-on: macos-latest
```

Useful for workflows requiring macOS.

For example:

```text
Apple platform development
macOS-specific testing
```

---

# Choosing a Runner

Choose the runner based on what your project needs.

```text
Linux Project
     ↓
Ubuntu Runner

Windows Project
     ↓
Windows Runner

macOS-specific Project
     ↓
macOS Runner
```

---

# Job Isolation

Jobs normally run in separate runner environments.

Example:

```text
Workflow
   │
   ├── Build Job
   │      ↓
   │   Runner A
   │
   └── Test Job
          ↓
       Runner B
```

Files created by one job are not automatically available in another job.

If you need to transfer files between jobs, you can use artifacts or other appropriate mechanisms.

---

# Multiple Jobs

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Building"

  test:
    runs-on: ubuntu-latest

    steps:
      - name: Test
        run: echo "Testing"
```

Conceptually:

```text
          Workflow
          /      \
         /        \
      Build       Test
        ↓           ↓
    Runner A    Runner B
```

If there is no dependency between the jobs, GitHub can run them independently.

---

# Job Dependencies

Use `needs` when one job depends on another.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Build complete"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Test
        run: echo "Testing after build"
```

The execution becomes:

```text
Build
  ↓
Test
```

---

# Multiple Dependencies

A job can depend on multiple jobs.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Build"

  security:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Security check"

  deploy:
    needs:
      - build
      - security

    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploy"
```

The structure becomes:

```text
Build ───────┐
             ├──→ Deploy
Security ────┘
```

Deployment waits for both jobs.

---

# Step Order

Steps within a job normally execute in the order they appear.

Example:

```yaml
steps:
  - name: First
    run: echo "1"

  - name: Second
    run: echo "2"

  - name: Third
    run: echo "3"
```

Execution:

```text
1
↓
2
↓
3
```

---

# What Happens When a Step Fails?

By default, a failing step causes the job to stop progressing normally.

Example:

```text
Step 1 → ✅
Step 2 → ❌
Step 3 → Not normally executed
```

This is useful for CI.

For example:

```text
Build
  ↓
Tests fail
  ↓
Deployment should not continue
```

---

# `if` Conditions

You can control whether a step or job runs using conditions.

Example:

```yaml
- name: Run Deployment
  if: ${{ success() }}
  run: echo "Deploying"
```

This runs when previous steps have succeeded.

---

# `always()`

A step can be configured to run regardless of the previous result.

Example:

```yaml
- name: Cleanup
  if: ${{ always() }}
  run: echo "Cleanup"
```

This can be useful for cleanup or diagnostic tasks.

Use conditions carefully, especially when workflows involve secrets or deployment operations.

---

# Shell Commands

Different runners provide different shell environments.

Example:

```yaml
run: echo "Hello"
```

On Ubuntu, commands generally execute using a shell environment appropriate for Linux.

Windows workflows can use PowerShell or other supported shells.

---

# Working Directory

A step can run from a specific directory.

Example:

```yaml
- name: Run Command
  working-directory: ./app
  run: ls
```

This runs the command inside:

```text
app/
```

---

# Environment Variables

Jobs and steps can use environment variables.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    env:
      MODE: testing

    steps:
      - name: Show Mode
        run: echo "$MODE"
```

---

# Complete Example

```yaml
name: Jobs and Runners

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Build Project
        run: echo "Building project"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "Running tests"

  security:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Security Check
        run: echo "Running security checks"
```

Execution:

```text
Build
  ↓
Test
  ↓
Security
```

Each job runs on its own runner environment.

---

# Jobs vs Steps

| Jobs | Steps |
|------|-------|
| Groups tasks | Individual tasks |
| Run on runners | Run inside a job |
| Can run in parallel | Usually execute sequentially |
| Can depend on other jobs | Can depend on previous steps |
| Can use different runners | Share the job's runner |

---

# Hosted vs Self-Hosted Runners

GitHub Actions can use:

```text
GitHub-Hosted Runners
```

or:

```text
Self-Hosted Runners
```

## GitHub-Hosted

GitHub provides and manages the runner environment.

Advantages:

- Easy to start.
- Minimal maintenance.
- Multiple operating system options.

---

## Self-Hosted

You provide and manage the machine that runs the workflow.

Example:

```text
Your Computer / Server
        ↓
Self-Hosted Runner
        ↓
GitHub Actions Job
```

Self-hosted runners can provide more control but require additional security and maintenance.

---

# Best Practices

- Keep jobs focused.
- Use meaningful job IDs.
- Use descriptive step names.
- Choose the correct runner.
- Use `needs` for dependencies.
- Avoid unnecessary job dependencies.
- Keep permissions minimal.
- Be careful with self-hosted runners.
- Do not expose secrets unnecessarily.

---

# Common Mistakes

- Assuming all jobs share the same filesystem.
- Forgetting `needs` when job order matters.
- Using the wrong runner.
- Creating one huge job instead of logical jobs.
- Giving self-hosted runners excessive access.
- Assuming a failed step will automatically continue.

---

# Interview Questions

### What is a job?

A job is a collection of steps that executes on a runner.

### What is a step?

A step is an individual task inside a job.

### What is a runner?

A runner is the machine or environment that executes a job.

### What does `needs` do?

It defines dependencies between jobs.

### Can multiple jobs run in parallel?

Yes, independent jobs can run in parallel.

### What happens when a step fails?

By default, the job does not continue normally to subsequent steps.

### What is a self-hosted runner?

A runner managed by the organization or repository owner rather than being provided as a GitHub-hosted runner.

---

# Practice

Create:

```text
.github/workflows/jobs-practice.yml
```

Paste:

```yaml
name: Jobs Practice

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
        run: echo "Tests completed"

  finish:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Finish
        run: echo "Workflow completed"
```

The execution should be:

```text
Build
  ↓
Test
  ↓
Finish
```

Commit message:

```text
Add GitHub Actions jobs steps and runners guide
```

Then open:

```text
Repository
   ↓
Actions
   ↓
Jobs Practice
   ↓
Run workflow
```

Open the run and inspect each job separately.

---

# Summary

The three fundamental concepts are:

```text
Job
 ↓
Steps
 ↓
Runner
```

A workflow can contain multiple jobs, each job can contain multiple steps, and each job runs on a runner.

Understanding this structure makes it much easier to build larger GitHub Actions workflows.
