
# GitHub Actions Conditions and Expressions

## 

```text
Build
 ↓
Test
 ↓
Deploy
```

With conditions:

```text
Build
 ↓
Tests Passed?
 ├── No  → Stop
 └── Yes → Deploy
```

Conditions allow workflows to respond differently depending on the situation.

---

# The `if` Keyword

The `if` keyword controls whether a job or step runs.

Example:

```yaml
- name: Deploy
  if: ${{ success() }}
  run: echo "Deploying"
```

The step runs when previous steps have succeeded.

---

# Basic Conditional Example

```yaml
name: Condition Example

on:
  workflow_dispatch:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: First Step
        run: echo "First step completed"

      - name: Second Step
        if: ${{ success() }}
        run: echo "Previous step succeeded"
```

---

# Expressions

Expressions allow you to evaluate values dynamically.

Expressions commonly use:

```text
${{ ... }}
```

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

This checks whether the workflow is running for the `main` branch reference.

---

# Comparison Operators

Common operators include:

```text
==
!=
>
<
>=
<=
```

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

Another:

```yaml
if: ${{ github.event_name == 'push' }}
```

---

# Logical Operators

You can combine conditions.

Common logical operators include:

```text
&&
||
!
```

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' && github.event_name == 'push' }}
```

This checks both conditions.

---

# OR Condition

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' || github.ref == 'refs/heads/develop' }}
```

The condition is true when either branch reference matches.

---

# NOT Condition

Example:

```yaml
if: ${{ !cancelled() }}
```

This means the condition is true when the workflow has not been cancelled.

---

# Status Check Functions

GitHub Actions provides functions that help determine the status of previous steps or jobs.

Important functions include:

```text
success()
failure()
cancelled()
always()
```

---

# `success()`

`success()` returns true when previous steps have succeeded.

Example:

```yaml
- name: Deploy
  if: ${{ success() }}
  run: echo "Deploying"
```

This is useful for operations that should only happen after successful checks.

---

# `failure()`

`failure()` returns true when a previous step or job has failed.

Example:

```yaml
- name: Show Failure Message
  if: ${{ failure() }}
  run: echo "Something failed"
```

This can be useful for diagnostics.

---

# `cancelled()`

`cancelled()` returns true when the workflow has been cancelled.

Example:

```yaml
- name: Cleanup
  if: ${{ cancelled() }}
  run: echo "Workflow was cancelled"
```

---

# `always()`

`always()` returns true regardless of the previous result.

Example:

```yaml
- name: Cleanup
  if: ${{ always() }}
  run: echo "Cleanup"
```

This can be useful for cleanup or diagnostic steps.

Use it carefully when working with secrets or sensitive operations.

---

# Conditional Step Example

```yaml
steps:
  - name: Run Tests
    run: echo "Tests running"

  - name: Deploy
    if: ${{ success() }}
    run: echo "Deploying application"

  - name: Failure Message
    if: ${{ failure() }}
    run: echo "Tests failed"
```

Conceptually:

```text
Run Tests
    ↓
   / \
Pass  Fail
 ↓      ↓
Deploy  Failure Message
```

---

# Branch-Based Conditions

You can use GitHub context values.

Example:

```yaml
- name: Production Deployment
  if: ${{ github.ref == 'refs/heads/main' }}
  run: echo "Deploying production"
```

This allows deployment only for the main branch.

---

# Event-Based Conditions

Example:

```yaml
- name: Pull Request Check
  if: ${{ github.event_name == 'pull_request' }}
  run: echo "This is a Pull Request"
```

The step runs only when the workflow was triggered by a Pull Request event.

---

# Contexts

Contexts provide information about the workflow execution.

Common contexts include:

```text
github
env
vars
secrets
runner
job
steps
needs
matrix
strategy
inputs
```

These contexts allow workflows to make decisions based on runtime information.

---

# GitHub Context

The `github` context contains information about the current workflow execution and repository event.

Example:

```yaml
${{ github.event_name }}
```

This can provide the event that triggered the workflow.

---

# Common GitHub Context Values

Examples include:

```text
github.repository
github.ref
github.sha
github.actor
github.event_name
```

Conceptually:

```text
github.repository
      ↓
owner/repository

github.ref
      ↓
Current Git reference

github.sha
      ↓
Commit SHA

github.actor
      ↓
User that initiated the workflow
```

---

# Environment Context

The `env` context can reference environment variables.

Example:

```yaml
env:
  APP_MODE: production

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Check Environment
        if: ${{ env.APP_MODE == 'production' }}
        run: echo "Production mode"
```

---

# Variables Context

GitHub Actions also supports configuration variables.

These can be accessed through:

```text
vars
```

Example:

```yaml
if: ${{ vars.DEPLOY_ENV == 'production' }}
```

Variables are intended for configuration, not sensitive credentials.

---

# Secrets Context

Secrets are accessed through:

```text
secrets
```

Example:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Do not use secrets unnecessarily in conditional logic, and never print secret values.

---

# Steps Context

The `steps` context can be used to access outputs from previous steps.

Example concept:

```text
Step A
 ↓
Output
 ↓
Step B
```

A previous step needs an `id` if you want to reference its outputs.

---

# Step ID

Example:

```yaml
- name: Generate Value
  id: generate
  run: echo "value=hello" >> "$GITHUB_OUTPUT"
```

The step ID is:

```text
generate
```

A later step can reference its output.

---

# Step Outputs

Example:

```yaml
name: Output Example

on:
  workflow_dispatch:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Generate Value
        id: generate
        run: echo "value=hello" >> "$GITHUB_OUTPUT"

      - name: Use Value
        run: echo "${{ steps.generate.outputs.value }}"
```

The flow is:

```text
Generate Value
      ↓
Output
      ↓
Use Value
```

---

# Job Outputs

A job can also provide outputs to another job.

Example:

```yaml
jobs:
  generate:
    runs-on: ubuntu-latest

    outputs:
      result: ${{ steps.value.outputs.result }}

    steps:
      - name: Generate Result
        id: value
        run: echo "result=success" >> "$GITHUB_OUTPUT"

  use-result:
    needs: generate
    runs-on: ubuntu-latest

    steps:
      - name: Show Result
        run: echo "${{ needs.generate.outputs.result }}"
```

The flow is:

```text
Job A
 ↓
Output
 ↓
Job B
```

---

# The `needs` Context

The `needs` context provides information about jobs that the current job depends on.

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
```

The `test` job can use information from the `build` job through the appropriate `needs` context.

---

# Conditional Jobs

Conditions can be applied to entire jobs.

Example:

```yaml
jobs:
  deploy:
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying"
```

The entire job is skipped if the condition is false.

---

# Conditional Steps

Conditions can also be applied to individual steps.

Example:

```yaml
steps:
  - name: Build
    run: echo "Building"

  - name: Deploy
    if: ${{ github.ref == 'refs/heads/main' }}
    run: echo "Deploying"
```

Only the deployment step is conditional.

---

# Job Condition vs Step Condition

| Job Condition | Step Condition |
|---|---|
| Controls entire job | Controls one step |
| Job may be skipped | Other steps can still run |
| Useful for deployment stages | Useful for specific tasks |

---

# Example: Production Deployment

```yaml
name: Conditional Deployment

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Build successful"

  deploy:
    needs: build
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Production deployment"
```

Workflow:

```text
Push
 ↓
Build
 ↓
Is branch main?
 ├── Yes → Deploy
 └── No  → Skip Deploy
```

---

# Skipped Jobs

A condition that evaluates to false can cause a job to be skipped.

Example:

```text
Build → Success
          ↓
      Deploy Condition
          ↓
       False
          ↓
       Skipped
```

A skipped job is different from a failed job.

---

# Matrix Conditions

Conditions can also be combined with matrix strategies.

For example, you may want to run certain tasks only for a particular operating system or version.

Conceptually:

```text
Matrix
 ├── Ubuntu
 ├── Windows
 └── macOS
```

A condition can restrict a task to one of these configurations.

---

# Expressions and Strings

Expressions can compare strings.

Example:

```yaml
if: ${{ github.event_name == 'push' }}
```

Make sure the values being compared are what you expect.

---

# Expressions Without `${{ }}`

For certain workflow fields, GitHub Actions can automatically evaluate expressions in an `if` conditional.

For clarity, beginners can consistently use:

```yaml
if: ${{ condition }}
```

when writing conditional expressions.

---

# Common Functions

Important functions to remember:

```text
success()
failure()
cancelled()
always()
```

Quick reference:

```text
success()
    ↓
Previous work succeeded

failure()
    ↓
Previous work failed

cancelled()
    ↓
Workflow was cancelled

always()
    ↓
Run regardless of previous result
```

---

# Example: Test and Report

```yaml
name: Test Conditions

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Run Tests
        run: exit 1

      - name: Failure Report
        if: ${{ failure() }}
        run: echo "Tests failed"
```

The test intentionally fails.

The failure-report step can then execute because:

```text
failure() = true
```

---

# Security Considerations

Conditions should not be treated as a complete security boundary.

Be careful when workflows involve:

```text
Secrets
Production Deployments
Pull Requests from Forks
Third-Party Actions
Self-Hosted Runners
```

Always design permissions and workflow triggers appropriately.

---

# Common Mistakes

Avoid:

```text
Using incorrect context names
Using wrong branch references
Forgetting job dependencies
Printing secrets while debugging
Creating overly complicated conditions
Assuming skipped means failed
```

---

# Best Practices

- Keep conditions readable.
- Use meaningful job and step names.
- Use `needs` for job dependencies.
- Use status functions appropriately.
- Restrict deployments to appropriate branches.
- Keep sensitive operations protected.
- Test conditions before using them in production workflows.

---

# Interview Questions

### What does `if` do in GitHub Actions?

It controls whether a job or step should run.

### What does `success()` mean?

It evaluates whether previous work has succeeded.

### What does `failure()` mean?

It evaluates whether a previous step or job has failed.

### What does `always()` do?

It evaluates to true regardless of the previous result.

### What is a context?

A context provides information about the workflow, repository, runner, jobs, steps, environment, and other runtime information.

### What is the `github` context?

It provides information about the repository and the event that triggered the workflow.

### What is the difference between a skipped job and a failed job?

A skipped job did not run because its condition was false or it was otherwise skipped. A failed job ran but encountered a failure.

---

# Practice

Create:

```text
.github/workflows/conditions-practice.yml
```

Paste:

```yaml
name: Conditions Practice

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build
        run: echo "Build completed"

      - name: Success Message
        if: ${{ success() }}
        run: echo "Build was successful"

  deploy:
    needs: build
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        run: echo "Deploying from main branch"
```

Commit message:

```text
Add GitHub Actions conditions and expressions guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Conditions Practice
   ↓
Run workflow
```

Check whether the `deploy` job runs or is skipped based on the branch.

---

# Summary

Conditions and expressions allow GitHub Actions workflows to make decisions.

The basic pattern is:

```text
Event
 ↓
Condition
 ↓
Decision
 ↓
Run / Skip
```

Important concepts include:

```text
if
Expressions
Contexts
success()
failure()
cancelled()
always()
needs
steps
```

These features allow you to create workflows that react intelligently to different branches, events, job results, environments, and workflow states.
