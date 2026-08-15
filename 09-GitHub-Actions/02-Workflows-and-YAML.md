
# GitHub Actions Workflows and YAML

## Introduction

GitHub Actions workflows are written using YAML.

YAML is a human-readable format commonly used for configuration files.

A GitHub Actions workflow tells GitHub:

- When to run automation.
- What jobs to execute.
- Which environment to use.
- Which steps to perform.
- Which commands or Actions to run.

---

# Workflow Location

GitHub Actions workflow files must be placed inside:

```text
.github/workflows/
```

Example:

```text
.github/
└── workflows/
    ├── hello.yml
    └── test.yml
```

Workflow files normally use:

```text
.yml
```

or:

```text
.yaml
```

---

# Basic Workflow Structure

A simple workflow looks like this:

```yaml
name: Example Workflow

on:
  workflow_dispatch:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Print Message
        run: echo "Hello GitHub Actions"
```

The main structure is:

```text
Workflow
   │
   ├── name
   ├── on
   └── jobs
         │
         └── Job
              │
              ├── runs-on
              └── steps
```

---

# The `name` Property

Example:

```yaml
name: Example Workflow
```

The `name` property gives the workflow a readable name.

This name appears in the GitHub Actions interface.

Example:

```text
Actions
   │
   └── Example Workflow
```

Choose a name that clearly describes what the workflow does.

---

# The `on` Property

The `on` section defines when the workflow should run.

Example:

```yaml
on:
  push:
```

This runs the workflow when changes are pushed to the repository.

Another example:

```yaml
on:
  pull_request:
```

This runs the workflow when a Pull Request event occurs.

---

# Manual Trigger

You can manually run a workflow using:

```yaml
on:
  workflow_dispatch:
```

This is useful when you want to start a workflow from GitHub manually.

---

# Multiple Triggers

A workflow can respond to multiple events.

Example:

```yaml
on:
  push:
  pull_request:
  workflow_dispatch:
```

This workflow can run when:

- Code is pushed.
- A Pull Request event occurs.
- The workflow is manually started.

---

# Jobs

The `jobs` section contains the work performed by the workflow.

Example:

```yaml
jobs:
  test:
```

Here:

```text
test
```

is the job ID.

A workflow can contain multiple jobs.

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

is an identifier for the job.

You can choose meaningful IDs such as:

```text
build
test
lint
deploy
security
```

---

# `runs-on`

Example:

```yaml
runs-on: ubuntu-latest
```

This specifies the runner environment used for the job.

Common GitHub-hosted runner environments include:

```text
Ubuntu
Windows
macOS
```

For many learning and development workflows, Ubuntu is a common choice.

---

# Steps

Steps define individual tasks inside a job.

Example:

```yaml
steps:
  - name: Print Message
    run: echo "Hello"
```

A job can contain multiple steps:

```yaml
steps:
  - name: Step One
    run: echo "First"

  - name: Step Two
    run: echo "Second"

  - name: Step Three
    run: echo "Third"
```

Steps normally execute sequentially within the job.

---

# The `run` Keyword

`run` executes a shell command.

Example:

```yaml
run: echo "Hello"
```

Another example:

```yaml
run: ls
```

Another:

```yaml
run: python --version
```

---

# The `uses` Keyword

`uses` allows a workflow to use a reusable Action.

Example:

```yaml
uses: actions/checkout@v4
```

The checkout Action retrieves the repository contents so later steps can work with them.

---

# `run` vs `uses`

| `run` | `uses` |
|------|--------|
| Executes a command | Uses an Action |
| Shell command | Reusable automation |
| Example: `echo "Hello"` | Example: `actions/checkout@v4` |

Example:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Show Files
    run: ls
```

---

# Step Names

Use descriptive step names.

Good:

```yaml
- name: Install Dependencies
  run: npm install
```

Better than:

```yaml
- name: Step 1
  run: npm install
```

Clear names make workflow logs easier to understand.

---

# YAML Indentation

YAML depends heavily on indentation.

Correct:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
```

Incorrect:

```yaml
jobs:
test:
runs-on: ubuntu-latest
```

Use consistent indentation.

Two spaces per indentation level is a common style.

---

# YAML Lists

Steps are represented as a list.

Example:

```yaml
steps:
  - name: First Step
    run: echo "First"

  - name: Second Step
    run: echo "Second"
```

The `-` represents an item in the list.

---

# Comments in YAML

Comments begin with:

```text
#
```

Example:

```yaml
# This workflow runs manually

name: Example
```

Comments are ignored by the workflow engine.

They can help explain complicated configuration.

---

# Multi-Line Commands

You can run multiple commands using:

```yaml
run: |
  echo "First command"
  echo "Second command"
  echo "Third command"
```

The `|` allows multiple lines to be included in the same command block.

---

# Example Workflow

```yaml
name: Basic Workflow

on:
  workflow_dispatch:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Show Current Directory
        run: pwd

      - name: Show Files
        run: ls

      - name: Print Message
        run: echo "Workflow completed!"
```

---

# What Happens?

When the workflow runs:

```text
Workflow Starts
      ↓
Runner Starts
      ↓
Checkout Repository
      ↓
Show Directory
      ↓
Show Files
      ↓
Print Message
      ↓
Workflow Finishes
```

---

# Multiple Jobs

A workflow can contain multiple jobs.

Example:

```yaml
name: Multiple Jobs

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Building project"

  test:
    runs-on: ubuntu-latest

    steps:
      - name: Test
        run: echo "Testing project"
```

Conceptually:

```text
Workflow
   │
   ├── Build Job
   │
   └── Test Job
```

By default, independent jobs can run in parallel.

---

# Job Dependencies

You can make one job wait for another using:

```yaml
needs:
```

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Build complete"

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - run: echo "Test after build"
```

The workflow becomes:

```text
Build
  ↓
Test
```

Without `needs`, independent jobs may run in parallel.

---

# Environment Variables

You can define environment variables using:

```yaml
env:
  APP_NAME: MyApplication
```

Example:

```yaml
name: Environment Example

on:
  workflow_dispatch:

env:
  APP_NAME: MyApplication

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Print Application Name
        run: echo "$APP_NAME"
```

---

# Job-Level Environment Variables

Environment variables can also be defined for a specific job.

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    env:
      ENVIRONMENT: testing

    steps:
      - name: Show Environment
        run: echo "$ENVIRONMENT"
```

---

# Step-Level Environment Variables

Environment variables can also be limited to a single step.

Example:

```yaml
steps:
  - name: Show Environment
    env:
      MODE: development
    run: echo "$MODE"
```

The variable is available to that step.

---

# Workflow Execution Model

A useful mental model is:

```text
Event
  ↓
Workflow
  ↓
Jobs
  ↓
Runner
  ↓
Steps
  ↓
Commands / Actions
```

---

# Complete Example

```yaml
name: Project CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Show Git Version
        run: git --version

      - name: Show Python Version
        run: python --version

      - name: Run Test Message
        run: echo "Tests would run here"
```

---

# Common YAML Mistakes

### Incorrect indentation

```yaml
jobs:
test:
runs-on: ubuntu-latest
```

### Correct indentation

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
```

---

### Incorrect step structure

```yaml
steps:
name: Test
run: echo "Test"
```

### Correct

```yaml
steps:
  - name: Test
    run: echo "Test"
```

---

# Best Practices

- Use meaningful workflow names.
- Use descriptive job IDs.
- Use descriptive step names.
- Keep indentation consistent.
- Keep workflows readable.
- Avoid unnecessary complexity.
- Use trusted Actions.
- Keep sensitive values out of plain YAML.
- Give workflows only the permissions they need.

---

# Interview Questions

### What format are GitHub Actions workflows written in?

YAML.

---

### Where are workflow files stored?

```text
.github/workflows/
```

---

### What does `on` define?

It defines the events that trigger a workflow.

---

### What does `jobs` define?

It defines the tasks performed by the workflow.

---

### What does `runs-on` define?

It specifies the runner environment used by a job.

---

### What is the difference between a job and a step?

A job is a group of steps that runs on a runner. A step is an individual task within that job.

---

### What does `needs` do?

It creates a dependency between jobs, causing one job to wait for another.

---

# Practice

Create:

```text
.github/workflows/yaml-practice.yml
```

Paste:

```yaml
name: YAML Practice

on:
  workflow_dispatch:

jobs:
  practice:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Show Directory
        run: pwd

      - name: Show Files
        run: ls

      - name: Print Message
        run: echo "I am learning GitHub Actions!"
```

Commit it with:

```text
Add GitHub Actions YAML workflow guide
```

Then go to:

```text
Repository
   ↓
Actions
   ↓
YAML Practice
   ↓
Run workflow
```

Open the workflow run and inspect each step's logs.

---

# Summary

GitHub Actions workflows use YAML to describe automated processes.

The fundamental structure is:

```text
Workflow
   ↓
Trigger
   ↓
Jobs
   ↓
Runner
   ↓
Steps
   ↓
Commands / Actions
```

Once YAML workflow syntax becomes comfortable, you can start building real CI/CD automation.
