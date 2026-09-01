# GitHub Actions Events and Triggers

## Introduction

A GitHub Actions workflow needs a trigger that determines when it should run.

The trigger is defined using:

```yaml
on:
```

Conceptually:

```text
Event
  ↓
Trigger
  ↓
Workflow
  ↓
Job
  ↓
Steps
```

---

# What Is an Event?

An event is an activity that happens in a GitHub repository.

Examples:

```text
Push
Pull Request
Issue
Release
Schedule
Manual Execution
```

A workflow can listen for these events.

---

# Push Event

Run a workflow whenever code is pushed:

```yaml
on:
  push:
```

Example:

```yaml
name: Push Check

on:
  push:

jobs:
  check:
    runs-on: ubuntu-latest

    steps:
      - run: echo "A push occurred!"
```

---

# Push to a Specific Branch

You can restrict the trigger:

```yaml
on:
  push:
    branches:
      - main
```

Now the workflow is intended to run for pushes to `main`.

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

The workflow can run for pushes to either branch.

---

# Branch Patterns

You can use patterns.

Example:

```yaml
on:
  push:
    branches:
      - 'release/**'
```

This can match branches such as:

```text
release/1.0
release/2.0
release/production
```

---

# Ignore Specific Branches

You can exclude branches using:

```yaml
on:
  push:
    branches-ignore:
      - develop
```

This runs for pushes except those targeting `develop`.

Do not use both `branches` and `branches-ignore` for the same event configuration.

---

# Pull Request Event

Basic:

```yaml
on:
  pull_request:
```

This allows the workflow to respond to Pull Request activity.

---

# Pull Requests Targeting Main

```yaml
on:
  pull_request:
    branches:
      - main
```

This limits the workflow to Pull Requests targeting `main`.

Remember:

```text
base branch = main
```

The branch being merged into is the important branch for this filter.

---

# Pull Request Activity Types

A Pull Request can generate different types of activity.

Examples include:

```text
opened
synchronize
reopened
closed
```

You can select specific activity types.

Example:

```yaml
on:
  pull_request:
    types:
      - opened
      - synchronize
      - reopened
```

---

# What Is `synchronize`?

A Pull Request is synchronized when new commits are pushed to its source branch.

Example:

```text
Feature Branch
     ↓
Push Commit
     ↓
Existing Pull Request
     ↓
synchronize event
```

This is useful for running CI whenever a contributor updates a Pull Request.

---

# Manual Trigger

Use:

```yaml
on:
  workflow_dispatch:
```

This allows a user to manually start the workflow from GitHub.

Example:

```yaml
name: Manual Test

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Manually started!"
```

---

# Scheduled Trigger

Workflows can run on a schedule.

Example:

```yaml
on:
  schedule:
    - cron: '0 0 * * *'
```

The cron expression represents a scheduled time.

Scheduled workflows use UTC.

---

# Schedule Example

A workflow can be scheduled periodically.

Example:

```yaml
on:
  schedule:
    - cron: '0 0 * * 1'
```

This represents a weekly schedule.

Always verify the cron expression and remember that GitHub's schedule uses UTC.

---

# Multiple Triggers

A workflow can have multiple triggers:

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

The workflow can then be triggered by:

```text
Push to main
Pull Request targeting main
Manual execution
```

---

# Release Event

A workflow can respond to releases.

Example:

```yaml
on:
  release:
    types:
      - published
```

Conceptually:

```text
Create Release
      ↓
Publish Release
      ↓
Workflow
```

This can be useful for automated release-related tasks.

---

# Issue Event

A workflow can respond to issue activity.

Example:

```yaml
on:
  issues:
    types:
      - opened
```

Conceptually:

```text
New Issue
   ↓
Workflow
   ↓
Automation
```

---

# Issue Comment Event

Workflows can also respond to issue comments.

Example:

```yaml
on:
  issue_comment:
    types:
      - created
```

This can be useful for repository automation.

---

# Workflow Run Event

A workflow can respond to another workflow's execution.

Example:

```yaml
on:
  workflow_run:
    workflows:
      - CI
    types:
      - completed
```

Conceptually:

```text
CI Workflow
     ↓
Completed
     ↓
Another Workflow
```

This can be useful for chaining automation.

---

# Path Filters

You can trigger workflows based on changed files.

Example:

```yaml
on:
  push:
    paths:
      - 'src/**'
```

The workflow is relevant when files under `src/` are changed.

---

# Multiple Paths

Example:

```yaml
on:
  push:
    paths:
      - 'src/**'
      - 'tests/**'
```

Changes under either path can trigger the workflow.

---

# Ignore Paths

You can exclude paths:

```yaml
on:
  push:
    paths-ignore:
      - 'docs/**'
```

Changes only affecting ignored paths may not trigger the workflow.

Do not use `paths` and `paths-ignore` together for the same event configuration.

---

# Branch + Path Filters

You can combine branch and path filtering.

Example:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'src/**'
```

This workflow is configured to run when a push:

```text
targets main
AND
matches the specified path filter
```

---

# Tags

You can also filter push events by tags.

Example:

```yaml
on:
  push:
    tags:
      - 'v*'
```

This can respond to tags such as:

```text
v1.0.0
v1.1.0
v2.0.0
```

---

# Branches vs Tags

Branches:

```yaml
branches:
  - main
```

Tags:

```yaml
tags:
  - 'v*'
```

They represent different Git references.

---

# Example Release Workflow Trigger

```yaml
name: Release Check

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - name: Show tag
        run: echo "Release tag triggered workflow"
```

---

# Event Filters

GitHub Actions gives you several ways to control when workflows run.

Common filters include:

```text
branches
branches-ignore
tags
tags-ignore
paths
paths-ignore
types
```

The available filters depend on the event.

---

# Trigger Logic

Suppose:

```yaml
on:
  push:
    branches:
      - main
    paths:
      - 'src/**'
```

Think of it as:

```text
Push
 ↓
Is branch main?
 ↓
YES
 ↓
Did relevant paths change?
 ↓
YES
 ↓
Run workflow
```

If the conditions do not match, the workflow does not run for that event.

---

# `workflow_dispatch` with Inputs

Manual workflows can accept inputs.

Example:

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment'
        required: true
        default: 'test'
```

The user can provide a value when manually starting the workflow.

---

# Example Manual Workflow

```yaml
name: Manual Deployment Test

on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment'
        required: true
        default: 'staging'

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Show environment
        run: echo "Deploying to ${{ inputs.environment }}"
```

---

# Why Use Manual Triggers?

Manual triggers are useful when:

```text
Deployment should be controlled manually
A maintenance task is needed
A workflow needs to be tested
An administrator wants to start a process
```

---

# Choosing the Right Trigger

### Code testing

Use:

```yaml
push
pull_request
```

### Deployment

Often:

```yaml
workflow_dispatch
```

or a controlled push/release trigger.

### Release automation

Consider:

```yaml
release
```

or tag-based `push`.

### Scheduled maintenance

Use:

```yaml
schedule
```

### Issue automation

Use:

```yaml
issues
```

---

# Practical CI Example

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

      - name: Run tests
        run: echo "Running tests..."
```

Workflow:

```text
Push to main
     ↓
CI
     ↓
Test
```

or:

```text
Pull Request → main
        ↓
       CI
        ↓
      Test
```

---

# Practical Documentation Workflow

Suppose your repository contains:

```text
docs/
src/
tests/
```

You only want your documentation workflow to run when documentation changes.

Use:

```yaml
name: Documentation Check

on:
  push:
    paths:
      - 'docs/**'

jobs:
  check:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - run: echo "Checking documentation"
```

---

# Event Selection Strategy

Before creating a workflow, ask:

```text
What should start this workflow?
```

Then select:

```text
Push?
Pull Request?
Release?
Schedule?
Manual?
Issue?
Other event?
```

Do not trigger expensive workflows unnecessarily.

---

# Workflow Trigger Design

Good:

```text
Pull Request
 ↓
Tests
 ↓
Review
```

Good:

```text
Release
 ↓
Package
 ↓
Publish
```

Good:

```text
Schedule
 ↓
Maintenance
```

Avoid:

```text
Every possible event
       ↓
Huge workflow
       ↓
Unnecessary runs
```

---

# Practice

Create:

```text
.github/workflows/events-practice.yml
```

Use:

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
  check:
    runs-on: ubuntu-latest

    steps:
      - name: Show event
        run: echo "GitHub Actions event triggered this workflow"

      - name: Show branch
        run: echo "${{ github.ref }}"
```

Commit:

```text
Add GitHub Actions events practice
```

Push to GitHub.

---

# Test the Triggers

Test:

```text
1. Push to main
2. Create a Pull Request targeting main
3. Manually run the workflow
```

Open:

```text
Repository
 ↓
Actions
 ↓
Events Practice
```

Inspect each workflow run.

---

# Practice 2: Path Filter

Create:

```text
.github/workflows/docs-check.yml
```

Use:

```yaml
name: Documentation Check

on:
  push:
    paths:
      - 'docs/**'

jobs:
  docs:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Check documentation
        run: echo "Documentation changed!"
```

If your repository does not contain a `docs/` directory, create one for practice.

---

# Practice 3: Manual Input

Create:

```text
.github/workflows/manual-practice.yml
```

Use:

```yaml
name: Manual Practice

on:
  workflow_dispatch:
    inputs:
      message:
        description: 'Message'
        required: true
        default: 'Hello GitHub Actions'

jobs:
  run:
    runs-on: ubuntu-latest

    steps:
      - name: Print message
        run: echo "${{ inputs.message }}"
```

Manually run the workflow and provide your own message.

---

# Challenge

Create a workflow that:

```text
Runs on push to main
Runs on Pull Requests to main
Can be manually triggered
```

Then add:

```text
One job
Three steps
```

Expected structure:

```text
Event
 ├── Push
 ├── Pull Request
 └── Manual
       ↓
      Job
       ↓
   Step 1
       ↓
   Step 2
       ↓
   Step 3
```

---

# Common Mistakes

## Mistake 1

Using the wrong branch filter.

```yaml
branches:
  - main
```

means the branch filter applies to the relevant event's branch context.

---

## Mistake 2

Confusing Pull Request source and target branches.

For:

```text
feature/login → main
```

the Pull Request targets:

```text
main
```

---

## Mistake 3

Using both `branches` and `branches-ignore` for the same event configuration.

Choose the appropriate filtering approach.

---

## Mistake 4

Using both `paths` and `paths-ignore` for the same event configuration.

Choose the appropriate filtering approach.

---

## Mistake 5

Assuming every workflow needs every trigger.

Use only the events required by the workflow.

---

# Interview Questions

### What is a GitHub Actions event?

An activity that can cause a workflow to run.

### What does `on` define?

The events and trigger configuration for a workflow.

### How do you trigger a workflow when code is pushed?

Use:

```yaml
on:
  push:
```

### How do you run a workflow for Pull Requests?

Use:

```yaml
on:
  pull_request:
```

### How do you manually run a workflow?

Use:

```yaml
on:
  workflow_dispatch:
```

### How do you schedule a workflow?

Use:

```yaml
on:
  schedule:
```

### What is a path filter?

A condition that limits workflow triggering based on changed file paths.

### What is a branch filter?

A condition that limits workflow triggering based on branches.

### What does `types` do?

It selects specific activity types for supported events.

### Why use event filters?

To run workflows only when relevant events occur and avoid unnecessary automation.

---

# Summary

The most important triggers are:

```text
push
pull_request
workflow_dispatch
schedule
release
issues
workflow_run
```

And common filters are:

```text
branches
branches-ignore
tags
tags-ignore
paths
paths-ignore
types
```

Remember:

```text
Event
 ↓
Trigger Configuration
 ↓
Workflow
 ↓
Job
 ↓
Steps
```

Choosing the correct trigger is the first step in designing an efficient GitHub Actions workflow.
