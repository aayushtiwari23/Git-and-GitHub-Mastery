
---

## 6. Job Outputs

A job can produce an output that another job can use.

Example:

```yaml
jobs:
  prepare:
    runs-on: ubuntu-latest

    outputs:
      version: ${{ steps.version.outputs.version }}

    steps:
      - name: Generate version
        id: version
        run: echo "version=1.0.0" >> "$GITHUB_OUTPUT"

  build:
    needs: prepare
    runs-on: ubuntu-latest

    steps:
      - name: Show version
        run: echo "Version: ${{ needs.prepare.outputs.version }}"
```

Flow:

```text
prepare
   ↓
version = 1.0.0
   ↓
build
```

---

## 7. Artifacts Between Jobs

Jobs normally run in separate environments.

If one job creates a file that another job needs, use artifacts.

Example:

```yaml
- name: Upload build
  uses: actions/upload-artifact@v4
  with:
    name: build
    path: dist/
```

Another job can download it:

```yaml
- name: Download build
  uses: actions/download-artifact@v4
  with:
    name: build
```

---

## 8. Conditional Jobs

Jobs can run only under certain conditions.

Example:

```yaml
deploy:
  if: github.ref == 'refs/heads/main'
  runs-on: ubuntu-latest

  steps:
    - run: echo "Deploying"
```

This prevents deployment from running on other branches.

---

## 9. Conditional Steps

Individual steps can also have conditions.

Example:

```yaml
- name: Deploy
  if: github.ref == 'refs/heads/main'
  run: echo "Deploy"
```

This is useful when only one part of a job needs a condition.

---

## 10. Manual Workflows

You can allow users to manually start a workflow.

Use:

```yaml
on:
  workflow_dispatch:
```

Example:

```yaml
name: Manual Deployment

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Manual deployment started"
```

This is useful for controlled deployments.

---

## 11. Manual Inputs

Manual workflows can accept inputs.

Example:

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: Deployment environment
        required: true
        default: staging
        type: choice
        options:
          - development
          - staging
          - production
```

The selected value can be accessed with:

```yaml
${{ inputs.environment }}
```

---

## 12. Reusable Workflow Pattern

Instead of duplicating deployment logic:

```text
Project A ──┐
Project B ──┼──→ Reusable Deployment Workflow
Project C ──┘
```

Use:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/deploy.yml
```

This keeps deployment logic centralized.

---

## 13. Matrix Testing

Matrix builds allow the same job to run with multiple configurations.

Example:

```yaml
strategy:
  matrix:
    node:
      - 20
      - 22
      - 24
```

The workflow tests all selected versions.

Conceptually:

```text
Node 20 ──→ Test
Node 22 ──→ Test
Node 24 ──→ Test
```

---

## 14. Matrix With Multiple Variables

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    node:
      - 22
      - 24
```

This creates combinations of:

```text
Ubuntu + Node 22
Ubuntu + Node 24
Windows + Node 22
Windows + Node 24
```

---

## 15. Fail-Fast Control

Matrix jobs can use:

```yaml
strategy:
  fail-fast: false
```

This allows other matrix jobs to continue even if one combination fails.

Useful when you want a complete compatibility report.

---

## 16. Concurrency

Concurrency prevents conflicting workflow runs from operating at the same time.

Example:

```yaml
concurrency:
  group: production
  cancel-in-progress: false
```

This can be particularly useful for deployments.

---

## 17. Canceling Outdated Runs

For development workflows, you may want a newer run to replace an older one.

Example:

```yaml
concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true
```

If a new commit is pushed to the same branch while an older run is still executing, the older run can be cancelled.

This can save runner time.

---

## 18. Environment-Based Deployment

A production pipeline can use environments.

Example:

```yaml
jobs:
  deploy:
    environment: production
    runs-on: ubuntu-latest

    steps:
      - run: echo "Production deployment"
```

This can be combined with:

- Environment secrets
- Required reviewers
- Deployment protection
- Branch restrictions

---

## 19. Build → Test → Deploy Pattern

A common CI/CD pattern is:

```text
Build
  ↓
Test
  ↓
Deploy
```

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Test"

  deploy:
    needs: test
    environment: production
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploy"
```

---

## 20. Build → Test → Staging → Production

A more realistic deployment pattern is:

```text
Build
  ↓
Test
  ↓
Staging
  ↓
Production
```

Production should only run after staging succeeds and any required protection rules are satisfied.

---

## 21. Dependency Graph Pattern

Complex workflows can be organized as a dependency graph.

Example:

```text
          ┌── Lint ──┐
          │          │
Start ────┼── Test ──┼──→ Build ──→ Staging ──→ Production
          │          │
          └── Scan ──┘
```

This allows independent checks to run simultaneously.

---

## 22. Conditional Deployment

Example:

```yaml
deploy:
  needs: test
  if: github.ref == 'refs/heads/main'
  runs-on: ubuntu-latest

  steps:
    - run: echo "Deploying production"
```

This ensures:

```text
Tests must pass
        AND
Branch must be main
```

before deployment runs.

---

## 23. Failure Handling

You can create a notification or diagnostic step:

```yaml
- name: Failure notification
  if: failure()
  run: echo "Workflow failed"
```

This can later be replaced with an actual notification system.

---

## 24. Cleanup Pattern

Cleanup steps can use:

```yaml
if: always()
```

Example:

```yaml
- name: Cleanup
  if: always()
  run: echo "Cleaning temporary files"
```

Use this carefully when cancellation behavior matters.

---

## 25. Caching in Advanced Pipelines

Caching can speed up dependency installation.

Example:

```yaml
- name: Setup Node
  uses: actions/setup-node@v4
  with:
    node-version: 24
    cache: npm
```

Caching is useful for dependencies that are downloaded repeatedly.

Don't use cache as a replacement for artifacts.

---

## 26. Artifacts vs Cache

### Artifact

Used to preserve and transfer files.

```text
Build
 ↓
Artifact
 ↓
Deploy
```

### Cache

Used to speed up repeated operations.

```text
Previous dependencies
        ↓
      Cache
        ↓
   Faster install
```

Simple rule:

```text
Need to transfer files → Artifact

Need to speed up repeated work → Cache
```

---

## 27. Reusable Components

Advanced workflows can combine:

```text
Reusable Workflows
Composite Actions
Artifacts
Caches
Environments
Matrix Builds
Expressions
Secrets
```

This allows large automation systems to remain organized.

---

## 28. Example Advanced CI Workflow

```yaml
name: Advanced CI

on:
  push:
    branches:
      - main

  pull_request:

permissions:
  contents: read

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Lint
        run: echo "Linting"

  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Test
        run: echo "Testing"

  build:
    needs:
      - lint
      - test

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Build
        run: echo "Building"

  deploy:
    needs: build

    if: github.ref == 'refs/heads/main'

    environment: production

    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying"
```

The workflow structure is:

```text
       ┌── Lint ──┐
       │          │
Start ─┤          ├──→ Build ──→ Deploy
       │          │
       └── Test ──┘
```

---

## 29. Workflow Design Principles

When designing advanced workflows:

```text
Keep jobs focused
        ↓
Run independent jobs in parallel
        ↓
Use needs for dependencies
        ↓
Use artifacts for file transfer
        ↓
Use cache for speed
        ↓
Use environments for deployments
        ↓
Use reusable workflows for repeated jobs
        ↓
Use composite actions for repeated steps
```

---

## 30. Avoid Overengineering

Advanced does not mean unnecessarily complicated.

Avoid creating:

```text
20 jobs for a simple project
```

when:

```text
Build → Test → Deploy
```

is sufficient.

Use advanced patterns only when they solve a real problem.

---

## 31. Practice

Create a workflow with:

```text
Lint ──┐
       ├──→ Build ──→ Deploy
Test ──┘
```

Requirements:

1. Lint and Test run independently.
2. Build waits for both.
3. Deploy waits for Build.
4. Deploy runs only on `main`.
5. Use a production environment.
6. Use least-privilege permissions.

---

## 32. Challenge

Build a complete CI/CD workflow:

```text
              ┌── Lint ──┐
              │          │
Pull Request ─┼── Test ──┼──→ Build ──→ Staging
              │          │
              └── Scan ──┘                 ↓
                                      Production
```

Requirements:

- Run independent checks in parallel.
- Use `needs`.
- Use an artifact between appropriate jobs.
- Use caching where useful.
- Use a production environment.
- Add a production deployment condition.
- Add concurrency for deployments.
- Add failure handling.
- Use least-privilege permissions.

---

## 33. Interview Questions

### Q1. Why use multiple jobs?

To separate responsibilities, improve organization, enable parallel execution, and control dependencies.

### Q2. What is the purpose of `needs`?

It defines dependencies between jobs.

### Q3. How can independent jobs run simultaneously?

Don't give them unnecessary `needs` dependencies.

### Q4. What are job outputs?

Values produced by one job that can be consumed by another dependent job.

### Q5. When should you use artifacts?

When files need to be preserved or transferred between jobs.

### Q6. When should you use caching?

When you want to speed up repeated operations such
