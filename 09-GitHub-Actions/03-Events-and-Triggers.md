
# GitHub Actions Events and Triggers

## Introduction

A GitHub Actions workflow needs a reason to start.

That reason is called an **event**.

Events allow you to control when a workflow should run.

For example:

```text
Code Push
    ↓
push Event
    ↓
Workflow Runs
```

---

# What is an Event?

An event is an activity that happens in a GitHub repository and can trigger a workflow.

Examples include:

```text
push
pull_request
issues
workflow_dispatch
schedule
release
```

The event is configured using the:

```yaml
on:
```

section of a workflow.

---

# Basic Event

Example:

```yaml
name: Push Workflow

on:
  push:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - run: echo "A push triggered this workflow!"
```

Whenever a push event occurs, the workflow can run.

---

# Common Events

Some commonly used GitHub Actions events include:

```text
push
pull_request
pull_request_target
issues
issue_comment
release
schedule
workflow_dispatch
workflow_call
workflow_run
```

GitHub supports many other events as well.

---

# Push Event

The `push` event runs when commits are pushed to the repository.

Example:

```yaml
on:
  push:
```

Workflow:

```text
Developer
    ↓
git push
    ↓
GitHub
    ↓
push Event
    ↓
Workflow
```

---

# Push to Specific Branches

You can limit a workflow to specific branches.

Example:

```yaml
on:
  push:
    branches:
      - main
```

Now the workflow runs when changes are pushed to:

```text
main
```

---

# Multiple Branches

Example:

```yaml
on:
  push:
    branches:
      - main
      - develop
```

The workflow runs for pushes to either branch.

---

# Branch Patterns

You can use patterns to match branches.

Example:

```yaml
on:
  push:
    branches:
      - 'release/**'
```

This can match branches such as:

```text
release/v1
release/v2
release/production
```

---

# Ignoring Branches

You can exclude branches using:

```yaml
on:
  push:
    branches-ignore:
      - development
```

This means pushes to `development` will not trigger the workflow through this configuration.

---

# Pull Request Event

The `pull_request` event runs when a Pull Request event occurs.

Example:

```yaml
on:
  pull_request:
```

This is commonly used for CI.

Example:

```text
Developer
    ↓
Feature Branch
    ↓
Pull Request
    ↓
CI Workflow
    ↓
Tests
    ↓
Build
```

---

# Pull Request Branch Filters

You can specify target branches.

Example:

```yaml
on:
  pull_request:
    branches:
      - main
```

The workflow runs for Pull Requests targeting:

```text
main
```

---

# Pull Request Activity Types

A Pull Request can generate different types of activity.

Examples include:

```text
opened
closed
synchronize
reopened
```

You can restrict a workflow using:

```yaml
on:
  pull_request:
    types:
      - opened
      - synchronize
```

This workflow responds to those selected Pull Request activities.

---

# `workflow_dispatch`

`workflow_dispatch` allows a workflow to be started manually.

Example:

```yaml
on:
  workflow_dispatch:
```

Workflow:

```text
GitHub Actions
      ↓
Select Workflow
      ↓
Run workflow
      ↓
Workflow Starts
```

This is extremely useful when you want manual control.

---

# Scheduled Workflows

You can run workflows automatically on a schedule using:

```yaml
on:
  schedule:
    - cron: '0 0 * * *'
```

The schedule uses a **cron expression**.

The example represents a scheduled run at a particular UTC time.

---

# Cron

Cron expressions contain five fields:

```text
minute hour day-of-month month day-of-week
```

Example:

```text
0 0 * * *
```

Conceptually:

```text
minute = 0
hour = 0
day = every day
month = every month
weekday = every day
```

GitHub Actions scheduled workflows use UTC for cron schedules.

---

# Scheduled Workflow Example

```yaml
name: Scheduled Check

on:
  schedule:
    - cron: '0 0 * * *'

jobs:
  check:
    runs-on: ubuntu-latest

    steps:
      - name: Run Check
        run: echo "Scheduled workflow running"
```

---

# Multiple Events

A workflow can have multiple triggers.

Example:

```yaml
on:
  push:
  pull_request:
  workflow_dispatch:
```

The workflow can now run because of:

```text
Push
   OR
Pull Request
   OR
Manual Run
```

---

# Combining Events with Branches

Example:

```yaml
on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main
```

This workflow responds to:

```text
Push → main
Pull Request → main
```

---

# Path Filters

You can also trigger workflows based on which files changed.

Example:

```yaml
on:
  push:
    paths:
      - 'src/**'
```

The workflow can run when files under:

```text
src/
```

are changed.

---

# Why Path Filters Matter

Imagine a repository contains:

```text
src/
docs/
images/
README.md
```

If a workflow only needs to test application code, running it every time documentation changes may be unnecessary.

A path filter can reduce unnecessary workflow runs.

---

# Path Ignore

You can also ignore specific paths.

Example:

```yaml
on:
  push:
    paths-ignore:
      - 'docs/**'
```

Changes only to documentation can be excluded from this trigger.

---

# Branch and Path Filters

You can combine branch and path conditions.

Example:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'src/**'
```

The workflow responds to pushes to `main` involving files under `src/`.

---

# Tags

You can trigger workflows for tag pushes.

Example:

```yaml
on:
  push:
    tags:
      - 'v*'
```

This can match tags such as:

```text
v1.0
v2.0
v3.1
```

Tags are commonly used for releases.

---

# Release Workflows

A workflow can respond to release events.

Example:

```yaml
on:
  release:
    types:
      - published
```

This can be useful for release automation.

Example:

```text
Release Published
       ↓
Workflow
       ↓
Build
       ↓
Package
       ↓
Deploy
```

---

# `workflow_call`

A workflow can be designed to be called by another workflow using:

```yaml
on:
  workflow_call:
```

This allows reusable workflows to be created.

Conceptually:

```text
Workflow A
    ↓
Calls
    ↓
Reusable Workflow B
```

Reusable workflows are useful when multiple repositories or workflows need the same automation logic.

---

# `workflow_run`

The `workflow_run` event can trigger a workflow based on another workflow's execution.

Conceptually:

```text
Build Workflow
      ↓
Completed
      ↓
workflow_run
      ↓
Deployment Workflow
```

This can be useful for chaining automation workflows.

---

# Event Filters

GitHub Actions provides several ways to control triggers.

Common filters include:

```text
branches
branches-ignore
paths
paths-ignore
tags
tags-ignore
types
```

These help prevent workflows from running unnecessarily.

---

# Event Flow

A useful mental model is:

```text
Repository Activity
        ↓
Event
        ↓
Filters
        ↓
Trigger Matches?
      /     \
    Yes      No
     ↓        ↓
 Workflow   Nothing
   Runs
```

---

# Example: CI Workflow

A common CI configuration is:

```yaml
name: CI

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run Tests
        run: echo "Running tests"
```

This provides automation for both:

```text
Push → main
Pull Request → main
```

---

# Example: Manual Deployment

A deployment workflow may use:

```yaml
name: Deploy

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deployment would happen here"
```

The deployment only starts when manually requested.

---

# Choosing the Right Trigger

Use:

```text
push
```

when you want automation after code is pushed.

Use:

```text
pull_request
```

when you want checks before merging.

Use:

```text
workflow_dispatch
```

when you want manual control.

Use:

```text
schedule
```

for recurring automation.

Use:

```text
release
```

for release-related automation.

Use:

```text
workflow_call
```

for reusable workflows.

---

# Security Considerations

Be careful when choosing workflow triggers.

Some events can run workflows in contexts involving code or changes from other contributors.

Always consider:

- What code the workflow will execute.
- Which permissions the workflow has.
- Whether secrets are available.
- Whether untrusted code can influence the workflow.

Never expose sensitive credentials unnecessarily.

---

# Best Practices

- Trigger workflows only when necessary.
- Use branch filters where appropriate.
- Use path filters for large repositories.
- Use Pull Request workflows for CI checks.
- Use manual triggers for sensitive operations when appropriate.
- Use scheduled workflows for recurring maintenance.
- Keep workflow permissions minimal.
- Be careful when workflows process untrusted contributions.

---

# Common Mistakes

- Running expensive workflows for every unnecessary change.
- Forgetting branch filters.
- Misunderstanding cron schedules.
- Exposing secrets to workflows unnecessarily.
- Using the wrong Pull Request event type.
- Creating overly complicated trigger conditions.

---

# Interview Questions

### What is an event in GitHub Actions?

An event is an activity in GitHub that can trigger a workflow.

### What does the `on` section do?

It defines the events and conditions that trigger a workflow.

### What is `workflow_dispatch`?

It allows a workflow to be manually triggered from GitHub.

### What is a scheduled workflow?

A workflow that runs automatically according to a cron schedule.

### What are path filters?

Path filters control whether a workflow runs based on which files were changed.

### What is the difference between `push` and `pull_request`?

`push` responds to pushes, while `pull_request` responds to Pull Request activity.

---

# Practice

Create:

```text
.github/workflows/events-practice.yml
```

Paste:

```yaml
name: Events Practice

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

jobs:
  practice:
    runs-on: ubuntu-latest

    steps:
      - name: Show Trigger
        run: echo "GitHub Actions event triggered this workflow!"
```

Commit message:

```text
Add GitHub Actions events and triggers guide
```

Then:

```text
1. Commit the file.
2. Open Actions.
3. Open Events Practice.
4. Run it manually using Run workflow.
5. Read the workflow logs.
```

Later, when you create a Pull Request or push to `main`, you can observe how the different triggers behave.

---

# Summary

Events determine **when** GitHub Actions workflows run.

The most important concepts are:

```text
Events
   ↓
Filters
   ↓
Trigger Conditions
   ↓
Workflow
```

Common triggers include:

```text
push
pull_request
workflow_dispatch
schedule
release
workflow_call
workflow_run
```

Understanding events and triggers is essential for building efficient and reliable GitHub Actions automation.
