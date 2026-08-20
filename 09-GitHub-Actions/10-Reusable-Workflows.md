
# GitHub Actions Reusable Workflows

## Introduction

A reusable workflow is a workflow that can be called by another workflow.

Instead of duplicating the same workflow configuration across multiple files, you can create one reusable workflow and call it whenever needed.

Conceptually:

```text
Workflow A
    ↓
Reusable Workflow
    ↓
Common Automation
```

---
# Why Use Reusable Workflows?

Imagine you have multiple repositories:

```text
Project A
Project B
Project C
```

All of them need the same:

```text
Build
Test
Lint
```

Instead of copying the same workflow three times:

```text
Project A → Build/Test/Lint
Project B → Build/Test/Lint
Project C → Build/Test/Lint
```

you can create:

```text
Reusable Workflow
       ↑
   ┌───┼───┐
   ↓   ↓   ↓
  A    B   C
```

---

# `workflow_call`

A workflow becomes reusable when it uses:

```yaml
on:
  workflow_call:
```

Example:

```yaml
name: Reusable Test

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "Running tests"
```

This workflow is designed to be called by another workflow.

---

# Calling a 
Reusable Workflow

A workflow can call another workflow using:

```yaml
uses:
```

Example:

```yaml
jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
```

Here:

```text
.github/workflows/reusable-test.yml
```

is the reusable workflow.

---

# Basic Example

### Reusable Workflow

Create:

```text
.github/workflows/reusable-test.yml
```

```yaml
name: Reusable Test

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: echo "Tests executed"
```

### Calling Workflow

Create:

```text
.github/workflows/main.yml
```

```yaml
name: Main Workflow

on:
  workflow_dispatch:

jobs:
  call-tests:
    uses: ./.github/workflows/reusable-test.yml
```

Flow:

```text
Main Workflow
      ↓
Reusable Test Workflow
      ↓
Run Tests
```

---

# Inputs

Reusable workflows can accept inputs.

Example:

```yaml
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
```

The calling workflow can provide a value.

---

# Passing Inputs

Caller:

```yaml
jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      environment: production
```

Reusable workflow:

```yaml
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
```

The reusable workflow can access:

```text
inputs.environment
```

---

# Complete Input Example

Reusable workflow:

```yaml
name: Reusable Deployment

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying to ${{ inputs.environment }}"
```

Caller:

```yaml
name: Deployment

on:
  workflow_dispatch:

jobs:
  deploy:
    uses: ./.github/workflows/reusable-deployment.yml
    with:
      environment: staging
```

The reusable workflow receives:

```text
environment = staging
```

---

# Input Types

Reusable workflow inputs can have types.

Common types include:

```text
string
boolean
number
```

Example:

```yaml
on:
  workflow_call:
    inputs:
      debug:
        required: false
        type: boolean
```

---

# Boolean Input

Example caller:

```yaml
with:
  debug: true
```

Reusable workflow:

```yaml
${{ inputs.debug }}
```

This allows workflows to change behavior based on a true/false value.

---

# Required Inputs

Example:

```yaml
inputs:
  environment:
    required: true
    type: string
```

The caller must provide the input.

---

# Optional Inputs

Example:

```yaml
inputs:
  environment:
    required: false
    type: string
```

The caller does not have to provide the value.

---

# Default Values

You can define a default value for optional inputs.

Example:

```yaml
inputs:
  environment:
    required: false
    type: string
    default: development
```

If the caller doesn't provide a value:

```text
environment = development
```

---

# Secrets

Reusable workflows can also work with secrets.

A reusable workflow can declare required secrets.

Example:

```yaml
on:
  workflow_call:
    secrets:
      API_TOKEN:
        required: true
```

The caller can provide the secret.

---

# Passing a Secret

Caller:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deployment.yml

    secrets:
      API_TOKEN: ${{ secrets.API_TOKEN }}
```

Reusable workflow:

```yaml
on:
  workflow_call:
    secrets:
      API_TOKEN:
        required: true
```

The reusable workflow can then use the secret where appropriate.

---

# Never Print Secrets

Even inside reusable workflows, never intentionally print secret values.

Bad:

```yaml
run: echo "${{ secrets.API_TOKEN }}"
```

Better:

```yaml
run: echo "Token is configured"
```

---

# `secrets: inherit`

For reusable workflows within the same organization or appropriate context, secrets can sometimes be passed using:

```yaml
secrets: inherit
```

Example:

```yaml
jobs:
  deploy:
    uses: organization/repository/.github/workflows/deploy.yml@main
    secrets: inherit
```

This should be used carefully because it can expose more secrets than necessary.

Prefer passing only the secrets the reusable workflow actually needs when practical.

---

# Local Reusable Workflows

A workflow can call another workflow in the same repository.

Example:

```yaml
uses: ./.github/workflows/reusable.yml
```

This is useful when several workflows in one repository share common logic.

---

# Reusable Workflows from Another Repository

Reusable workflows can also be stored in another repository.

Conceptually:

```yaml
uses: organization/repository/.github/workflows/reusable.yml@main
```

This allows organizations to centralize common workflow logic.

---

# Versioning External Reusable Workflows

When calling a workflow from another repository, specify an appropriate reference.

Example:

```yaml
uses: organization/ci-workflows/.github/workflows/test.yml@v1
```

Possible references can include:

```text
Branch
Tag
Commit SHA
```

For security-sensitive workflows, carefully consider how the referenced version is controlled.

---

# Reusable Workflow vs Action

These are different concepts.

### Action

A reusable automation component used as a step.

```text
Job
 ↓
Step
 ↓
Action
```

Example:

```yaml
- uses: actions/checkout@v4
```

### Reusable Workflow

A complete workflow called as a job.

```text
Workflow
 ↓
Job
 ↓
Reusable Workflow
```

Example:

```yaml
jobs:
  test:
    uses: ./.github/workflows/test.yml
```

---

# Reusable Workflow vs Composite Action

A composite Action combines multiple steps into one reusable Action.

Conceptually:

```text
Composite Action
 ├── Step 1
 ├── Step 2
 └── Step 3
```

A reusable workflow can contain jobs:

```text
Reusable Workflow
 ├── Job 1
 ├── Job 2
 └── Job 3
```

---

# Comparison

| Feature | Action | Composite Action | Reusable Workflow |
|---|---|---|---|
| Reusable component | Yes | Yes | Yes |
| Used as a step | Yes | Yes | No |
| Called as a job | No | No | Yes |
| Can contain multiple jobs | No | No | Yes |
| Useful for workflow architecture | Limited | Moderate | High |

---

# Reusable Workflow Architecture

A larger organization might use:

```text
Application Repository
        ↓
Reusable CI Workflow
        ↓
Build
Test
Security Scan
        ↓
Reusable Deployment Workflow
        ↓
Deploy
```

This reduces duplication.

---

# Example: Centralized CI

Suppose multiple projects need:

```text
Checkout
Setup Python
Install Dependencies
Run Tests
```

Create one reusable workflow:

```yaml
name: Python CI

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - run: pip install -r requirements.txt

      - run: pytest
```

Each project can then call it.

---

# Calling Centralized CI

```yaml
jobs:
  ci:
    uses: organization/ci-workflows/.github/workflows/python-ci.yml@v1
```

Now the project doesn't need to duplicate the entire CI workflow.

---

# Benefits

Reusable workflows provide:

```text
Less duplication
Centralized maintenance
Consistent CI/CD
Easier updates
Better organization
```

---

# Risks

Centralized workflows also require careful management.

If a shared workflow changes:

```text
Reusable Workflow
       ↓
Many Repositories
       ↓
Behavior Changes
```

Therefore, versioning and testing are important.

---

# Versioned Reusable Workflows

A useful organizational approach is:

```text
v1
v2
v3
```

Repositories can choose when to upgrade.

Conceptually:

```text
Project A → CI v1
Project B → CI v1
Project C → CI v2
```

This avoids forcing every project to change simultaneously.

---

# Permissions

Reusable workflows should follow the principle of least privilege.

Avoid giving them unnecessary permissions.

Example:

```yaml
permissions:
  contents: read
```

Only grant additional permissions when required.

---

# Security Considerations

Be especially careful with reusable workflows that:

```text
Access Secrets
Deploy Applications
Modify Repositories
Access Cloud Resources
Run Third-Party Code
```

A centralized workflow can affect many repositories, so mistakes can have a large impact.

---

# Common Mistakes

Avoid:

```text
Duplicating identical workflows
Using untrusted external workflows
Passing unnecessary secrets
Giving excessive permissions
Changing shared workflows without testing
Using mutable references carelessly
```

---

# Best Practices

- Use reusable workflows for repeated workflow-level logic.
- Keep inputs clearly defined.
- Pass only required secrets.
- Use minimum permissions.
- Version shared workflows.
- Test changes before rolling them out widely.
- Document reusable workflow inputs and outputs.
- Keep reusable workflows focused.

---

# Interview Questions

### What is a reusable workflow?

A workflow that can be called by another workflow.

### How do you make a workflow reusable?

Use:

```yaml
on:
  workflow_call:
```

### How do you call a reusable workflow?

Use it at the job level with:

```yaml
uses:
```

### What are workflow inputs?

Values passed from the calling workflow to the reusable workflow.

### What is the difference between an Action and a reusable workflow?

An Action is generally used as a workflow step, while a reusable workflow is called as a job and can contain multiple jobs and steps.

### Why are reusable workflows useful?

They reduce duplication and provide centralized, consistent CI/CD logic.

---

# Practice

Create:

```text
.github/workflows/reusable-test.yml
```

Paste:

```yaml
name: Reusable Test

on:
  workflow_call:
    inputs:
      message:
        required: false
        type: string
        default: "Running reusable workflow"

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Show Message
        run: echo "${{ inputs.message }}"
```

Then create:

```text
.github/workflows/reusable-workflow-practice.yml
```

Paste:

```yaml
name: Reusable Workflow Practice

on:
  workflow_dispatch:

jobs:
  call-test:
    uses: ./.github/workflows/reusable-test.yml
    with:
      message: "Hello from the calling workflow"
```

Run:

```text
Repository
   ↓
Actions
   ↓
Reusable Workflow Practice
   ↓
Run workflow
```

You should see:

```text
Hello from the calling workflow
```

---

# Summary

Reusable workflows allow you to share complete workflow logic.

The basic structure is:

```text
Reusable Workflow
        ↑
        │
Calling Workflow
```

Important concepts:

```text
workflow_call
inputs
secrets
uses
Reusable Workflow
Versioning
Permissions
```

The main benefit is simple:

```text
Write Once
    ↓
Reuse Across Workflows
    ↓
Less Duplication
    ↓
Consistent Automation
```
