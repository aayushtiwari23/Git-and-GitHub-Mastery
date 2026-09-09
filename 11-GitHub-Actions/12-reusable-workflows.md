
# GitHub Actions Reusable Workflows

## 1. What Is a Reusable Workflow?

A reusable workflow is a GitHub Actions workflow that can be called and used by another workflow.

Instead of copying the same workflow code into multiple files, you can create it once and reuse it.

Example:

```text
Workflow A ──┐
             ↓
        Reusable Workflow
             ↑
Workflow B ──┘
```

---

## 2. Why Use Reusable Workflows?

Reusable workflows help reduce duplicate code.

Without reusable workflows:

```text
Project A
  ↓
Copy workflow

Project B
  ↓
Copy workflow

Project C
  ↓
Copy workflow
```

With reusable workflows:

```text
Reusable Workflow
       ↑
       ├── Project A
       ├── Project B
       └── Project C
```

---

## 3. The `workflow_call` Trigger

A workflow becomes reusable by using:

```yaml
on:
  workflow_call:
```

Example:

```yaml
name: Reusable Build

on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building application"
```

This workflow can now be called by another workflow.

---

## 4. Calling a Reusable Workflow

Another workflow can call it using:

```yaml
uses:
```

Example:

```yaml
jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
```

The path points to the reusable workflow.

---

## 5. Complete Example

Create:

```text
.github/workflows/reusable-build.yml
```

Use:

```yaml
name: Reusable Build

on:
  workflow_call:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Building application"
```

Then create another workflow:

```text
.github/workflows/main.yml
```

Use:

```yaml
name: Main Workflow

on:
  workflow_dispatch:

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
```

The main workflow calls the reusable workflow.

---

## 6. Workflow Flow

The structure is:

```text
main.yml
   ↓
reusable-build.yml
   ↓
Build Job
```

The main workflow doesn't need to duplicate the build steps.

---

## 7. Reusable Workflow With Inputs

Reusable workflows can accept inputs.

Example:

```yaml
name: Reusable Build

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Environment: ${{ inputs.environment }}"
```

The reusable workflow expects an input called:

```text
environment
```

---

## 8. Passing an Input

The calling workflow can provide the value.

Example:

```yaml
jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      environment: development
```

The reusable workflow receives:

```text
development
```

---

## 9. Complete Input Example

Reusable workflow:

```yaml
name: Reusable Build

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Show environment
        run: echo "Environment: ${{ inputs.environment }}"
```

Calling workflow:

```yaml
name: Main

on:
  workflow_dispatch:

jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      environment: staging
```

The output will contain:

```text
Environment: staging
```

---

## 10. Input Types

Reusable workflow inputs can have types such as:

```text
string
boolean
number
```

Example:

```yaml
inputs:
  debug:
    required: true
    type: boolean
```

The caller can provide:

```yaml
with:
  debug: true
```

---

## 11. Optional Inputs

An input does not always have to be required.

Example:

```yaml
inputs:
  environment:
    required: false
    type: string
    default: development
```

If the caller doesn't provide a value, the default can be used.

---

## 12. Reusable Workflow With Secrets

Reusable workflows can also receive secrets.

Example:

```yaml
name: Reusable Deploy

on:
  workflow_call:
    secrets:
      DEPLOY_TOKEN:
        required: true

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: echo "Deployment token is available"
```

The secret itself should not be printed.

---

## 13. Passing Secrets

The calling workflow can pass a secret:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deploy.yml

    secrets:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

The reusable workflow can then access:

```yaml
${{ secrets.DEPLOY_TOKEN }}
```

---

## 14. Passing All Available Secrets

A caller can use:

```yaml
secrets: inherit
```

Example:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    secrets: inherit
```

This allows the called workflow to access secrets available to the caller according to GitHub's permissions and security rules.

Use this carefully.

---

## 15. Reusable Workflow With Environment

A reusable workflow can also be used as part of deployment pipelines.

Example:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
```

The reusable workflow can use the input to determine its deployment behavior.

---

## 16. Reusing Workflows Across Repositories

Reusable workflows don't have to exist only in the same repository.

An organization can maintain reusable workflows in a dedicated repository.

Conceptually:

```text
Central Repository
        ↓
Reusable Workflows
        ↓
Project A
Project B
Project C
```

This is useful for organizations that want standardized CI/CD processes.

---

## 17. Local vs External Reusable Workflows

Local workflow:

```yaml
uses: ./.github/workflows/reusable.yml
```

External workflow:

```yaml
uses: organization/repository/.github/workflows/reusable.yml@main
```

When using workflows from another repository, specify an appropriate reference such as a branch, tag, or commit SHA.

---

## 18. Why Versioning Matters

Suppose many projects use:

```text
Reusable Workflow v1
```

If you make changes to the reusable workflow, those changes can affect multiple projects.

Versioning helps control changes.

For example:

```text
v1
v2
v3
```

Projects can choose which version they use.

---

## 19. Reusable Workflow vs Composite Action

These two concepts are different.

### Reusable Workflow

Reuses an entire workflow or set of jobs.

```text
Workflow
   ↓
Jobs
   ↓
Steps
```

### Composite Action

Reuses a collection of steps.

```text
Action
   ↓
Steps
```

Simple rule:

```text
Reuse jobs/workflow → Reusable Workflow

Reuse steps → Composite Action
```

---

## 20. Reusable Workflow Example

Reusable workflow:

```yaml
name: Reusable Test

on:
  workflow_call:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run tests
        run: echo "Running tests"
```

Calling workflow:

```yaml
name: CI

on:
  workflow_dispatch:

jobs:
  test:
    uses: ./.github/workflows/reusable-test.yml
```

---

## 21. Practice

Create:

```text
.github/workflows/reusable.yml
```

Use:

```yaml
name: Reusable Workflow

on:
  workflow_call:
    inputs:
      name:
        required: true
        type: string

jobs:
  hello:
    runs-on: ubuntu-latest

    steps:
      - name: Greeting
        run: echo "Hello ${{ inputs.name }}"
```

Then create:

```text
.github/workflows/main.yml
```

Use:

```yaml
name: Main Workflow

on:
  workflow_dispatch:

jobs:
  hello:
    uses: ./.github/workflows/reusable.yml
    with:
      name: GitHub
```

Run the main workflow.

Expected output:

```text
Hello GitHub
```

---

## 22. Challenge

Create a reusable workflow that accepts:

```text
environment
```

as an input.

It should display:

```text
Deploying to development
```

or:

```text
Deploying to staging
```

or:

```text
Deploying to production
```

The calling workflow should decide which environment is passed.

---

## 23. Important Rules

Remember:

1. Use `workflow_call` to create a reusable workflow.
2. Use `uses` to call another workflow.
3. Use `with` to pass inputs.
4. Use `secrets` to pass specific secrets.
5. Use `secrets: inherit` carefully.
6. Don't print sensitive values.
7. Version shared workflows when appropriate.
8. Use reusable workflows to avoid duplicate CI/CD code.

---

## 24. Interview Questions

### Q1. What is a reusable workflow?

A workflow that can be called and executed by another workflow.

### Q2. How do you make a workflow reusable?

Use:

```yaml
on:
  workflow_call:
```

### Q3. How do you call a reusable workflow?

Use:

```yaml
uses:
```

### Q4. How do you pass inputs?

Use:

```yaml
with:
```

### Q5. How do you pass secrets?

Use:

```yaml
secrets:
```

### Q6. What does `secrets: inherit` do?

It allows a called workflow to access secrets available to the caller, subject to GitHub's security and permission rules.

### Q7. What is the difference between a reusable workflow and a composite action?

A reusable workflow reuses jobs/workflow logic, while a composite action packages reusable steps.

### Q8. Why use reusable workflows?

They reduce duplicated workflow code and help standardize CI/CD processes.

---

# Summary

Reusable workflows allow you to create a workflow once and use it from other workflows.

Create:

```yaml
on:
  workflow_call:
```

Call it with:

```yaml
uses:
```

Pass inputs with:

```yaml
with:
```

Pass secrets with:

```yaml
secrets:
```

The basic idea is:

```text
Create Once
    ↓
Reusable Workflow
    ↓
Use in Multiple Workflows
```

Reusable workflows are especially useful for larger projects and organizations where the same CI/CD process is used repeatedly.

Next topic: GitHub Actions Composite Actions
