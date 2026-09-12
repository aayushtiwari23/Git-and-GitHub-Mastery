

## 3. Basic Expression Syntax

The basic syntax is:

```yaml
${{ value }}
```

Example:

```yaml
name: ${{ github.workflow }}
```

Another example:

```yaml
run: echo "${{ github.repository }}"
```

---

## 4. GitHub Actions Contexts

Contexts provide information about the workflow, repository, event, jobs, steps, and other GitHub Actions data.

Common contexts include:

```text
github
env
vars
secrets
job
jobs
steps
runner
matrix
needs
inputs
```

---

## 5. The `github` Context

The `github` context contains information about the workflow run and repository.

Example:

```yaml
- name: Repository
  run: echo "${{ github.repository }}"
```

It can provide information such as:

```text
Repository
Branch
Commit
Event
Actor
Workflow
Run ID
```

---

## 6. Useful `github` Properties

### Repository

```yaml
${{ github.repository }}
```

Example:

```text
owner/repository
```

### Branch or Ref

```yaml
${{ github.ref }}
```

### Commit SHA

```yaml
${{ github.sha }}
```

### Actor

```yaml
${{ github.actor }}
```

### Event Name

```yaml
${{ github.event_name }}
```

### Workflow Name

```yaml
${{ github.workflow }}
```

### Run ID

```yaml
${{ github.run_id }}
```

---

## 7. Example

```yaml
name: Context Demo

on:
  workflow_dispatch:

jobs:
  info:
    runs-on: ubuntu-latest

    steps:
      - name: Show information
        run: |
          echo "Repository: ${{ github.repository }}"
          echo "Actor: ${{ github.actor }}"
          echo "Event: ${{ github.event_name }}"
          echo "Commit: ${{ github.sha }}"
```

---

## 8. The `env` Context

The `env` context provides environment variables defined in the workflow.

Example:

```yaml
env:
  APP_NAME: MyApp
```

Use:

```yaml
${{ env.APP_NAME }}
```

Example:

```yaml
- run: echo "Application: ${{ env.APP_NAME }}"
```

---

## 9. Different Environment Levels

Environment variables can be defined at different levels.

### Workflow level

```yaml
env:
  APP_NAME: MyApp
```

### Job level

```yaml
jobs:
  build:
    env:
      ENVIRONMENT: development
```

### Step level

```yaml
steps:
  - name: Test
    env:
      MODE: test
    run: echo "$MODE"
```

More specific levels can override broader values.

---

## 10. The `vars` Context

The `vars` context provides GitHub Actions configuration variables.

Example:

```yaml
${{ vars.APP_NAME }}
```

Repository or organization variables can be configured in GitHub.

Example:

```yaml
- run: echo "Application: ${{ vars.APP_NAME }}"
```

---

## 11. The `secrets` Context

The `secrets` context provides access to GitHub Actions secrets.

Example:

```yaml
${{ secrets.API_KEY }}
```

Example:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Never print secrets to logs.

---

## 12. The `steps` Context

The `steps` context provides information about previous steps.

A step must have an `id` to reference its outputs.

Example:

```yaml
- name: Generate value
  id: result
  run: echo "value=100" >> "$GITHUB_OUTPUT"
```

Access the output:

```yaml
${{ steps.result.outputs.value }}
```

Example:

```yaml
- name: Show result
  run: echo "Value: ${{ steps.result.outputs.value }}"
```

---

## 13. The `job` Context

The `job` context provides information about the current job.

For example:

```yaml
${{ job.status }}
```

Possible statuses include:

```text
success
failure
cancelled
```

Example:

```yaml
- name: Show status
  run: echo "Job status: ${{ job.status }}"
```

---

## 14. The `runner` Context

The `runner` context provides information about the runner.

Example:

```yaml
${{ runner.os }}
```

Possible values include:

```text
Linux
Windows
macOS
```

Example:

```yaml
- run: echo "Operating System: ${{ runner.os }}"
```

---

## 15. The `matrix` Context

The `matrix` context is used with matrix jobs.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest
```

Use:

```yaml
runs-on: ${{ matrix.os }}
```

Another example:

```yaml
- run: echo "Running on ${{ matrix.os }}"
```

---

## 16. The `needs` Context

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

The second job depends on the first job.

You can check its result:

```yaml
${{ needs.build.result }}
```

---

## 17. The `inputs` Context

The `inputs` context is commonly used with:

- Reusable workflows
- `workflow_dispatch`
- Actions

Example:

```yaml
${{ inputs.environment }}
```

If a reusable workflow receives:

```yaml
environment: production
```

it can access:

```yaml
${{ inputs.environment }}
```

---

## 18. Conditional Expressions

Expressions become especially useful with `if`.

Example:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

This step runs only when the workflow is running on the `main` branch.

---

## 19. `if` Without `${{ }}`

GitHub Actions allows expressions directly in many `if` conditions.

Instead of:

```yaml
if: ${{ github.ref == 'refs/heads/main' }}
```

you can commonly write:

```yaml
if: github.ref == 'refs/heads/main'
```

Both represent an expression condition.

---

## 20. Comparison Operators

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
if: github.event_name == 'push'
```

Another:

```yaml
if: github.ref != 'refs/heads/main'
```

---

## 21. Logical Operators

### AND

```text
&&
```

Example:

```yaml
if: github.ref == 'refs/heads/main' && github.event_name == 'push'
```

### OR

```text
||
```

Example:

```yaml
if: github.ref == 'refs/heads/main' || github.ref == 'refs/heads/develop'
```

### NOT

```text
!
```

Example:

```yaml
if: ${{ !cancelled() }}
```

---

## 22. Useful Functions

GitHub Actions expressions provide built-in functions.

Common functions include:

```text
success()
failure()
cancelled()
always()
contains()
startsWith()
endsWith()
format()
join()
toJSON()
fromJSON()
```

---

## 23. `success()`

Checks whether previous steps/jobs succeeded.

Example:

```yaml
if: success()
```

---

## 24. `failure()`

Runs when a previous step or dependency has failed.

Example:

```yaml
- name: Show failure message
  if: failure()
  run: echo "Something failed"
```

---

## 25. `always()`

Runs regardless of whether previous steps succeeded or failed.

Example:

```yaml
- name: Cleanup
  if: always()
  run: echo "Cleanup"
```

Use carefully, especially when a job may have been cancelled.

---

## 26. `cancelled()`

Checks whether the workflow has been cancelled.

Example:

```yaml
if: cancelled()
```

---

## 27. `contains()`

Checks whether a value contains another value.

Example:

```yaml
if: contains(github.ref, 'main')
```

Another example:

```yaml
if: contains(github.event.head_commit.message, 'deploy')
```

---

## 28. `startsWith()`

Checks whether a string starts with a specific value.

Example:

```yaml
if: startsWith(github.ref, 'refs/heads/')
```

---

## 29. `endsWith()`

Checks whether a string ends with a specific value.

Example:

```yaml
if: endsWith(github.ref, '/main')
```

---

## 30. `format()`

Creates formatted strings.

Example:

```yaml
${{ format('Hello {0}', github.actor) }}
```

---

## 31. `toJSON()`

Converts a context or object into JSON.

Example:

```yaml
- name: Show event
  run: echo '${{ toJSON(github.event) }}'
```

Be careful when displaying event data because it may contain information you don't want in logs.

---

## 32. `fromJSON()`

Converts JSON into a value that expressions can work with.

Example:

```yaml
${{ fromJSON('["build", "test"]') }}
```

It can also be useful for dynamically creating matrix values.

---

## 33. Dynamic Job Conditions

Example:

```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying"
```

The deploy job runs only on `main`.

---

## 34. Combining Contexts

Expressions can combine different contexts.

Example:

```yaml
if: github.ref == 'refs/heads/main' && runner.os == 'Linux'
```

This checks:

```text
Branch = main
AND
Runner OS = Linux
```

---

## 35. Practical Example

```yaml
name: Expression Demo

on:
  push:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Show repository
        run: echo "Repository: ${{ github.repository }}"

      - name: Show actor
        run: echo "Actor: ${{ github.actor }}"

      - name: Main branch check
        if: github.ref == 'refs/heads/main'
        run: echo "Running on main branch"

      - name: Build
        run: echo "Building project"

      - name: Failure handler
        if: failure()
        run: echo "A previous step failed"
```

---

## 36. Important Security Rule

Expressions can insert dynamic values into commands.

Be careful with untrusted input.

For example, data from pull requests, issue titles, commit messages, or other event payloads should not automatically be placed into shell commands.

Avoid blindly doing:

```yaml
run: echo "${{ github.event.issue.title }}"
```

when the value is untrusted and could affect shell interpretation.

Treat external event data as untrusted input.

---

## 37. Practice

Create a workflow that displays:

```text
Repository
Actor
Branch/Ref
Event
Runner OS
Commit SHA
```

Then add a condition:

```text
Only display "Production Deployment"
when the workflow is running on main.
```

---

## 38. Challenge

Create a workflow with:

### Job 1

Build the project.

### Job 2

Run only when Job 1 succeeds.

### Job 3

Run only when Job 2 fails.

Use:

```text
needs
if
success()
failure()
```

Also display the repository name and runner operating system.

---

## 39. Important Contexts to Remember

```text
github  → Workflow/repository/event information

env     → Environment variables

vars    → Configuration variables

secrets → Secrets

steps   → Previous step outputs/status

job     → Current job information

runner  → Runner information

matrix  → Matrix values

needs   → Dependency job information

inputs  → Workflow/action inputs
```

---

## 40. Interview Questions

### Q1. What is an expression in GitHub Actions?

An expression dynamically evaluates values or conditions during workflow execution.

### Q2. What syntax is used for expressions?

```yaml
${{ expression }}
```

### Q3. What is the `github` context?

It provides information about the workflow run, repository, event, commit, actor, and related GitHub data.

### Q4. What is the `steps` context used for?

It is used to access information and outputs from previous steps.

### Q5. What is the `needs` context used for?

It provides information about jobs that the current job depends on.

### Q6. What is the `matrix` context?

It provides the current values of a matrix strategy.

### Q7. What does `failure()` do?

It evaluates to true when a previous step or required dependency has failed.

### Q8. What does `always()` do?

It allows a step or job to run regardless of the success or failure state of previous work, subject to workflow cancellation behavior.

### Q9. What is the difference between `vars` and `secrets`?

Variables are intended for configuration data, while secrets are intended for sensitive information.

### Q10. Why should expressions be handled carefully?

Because dynamically inserting untrusted data into commands can create security vulnerabilities.

---

# Summary

GitHub Actions expressions make workflows dynamic and intelligent.

Basic syntax:

```yaml
${{ expression }}
```

Important contexts:

```text
github
env
vars
secrets
steps
job
runner
matrix
needs
inputs
```

Important functions:

```text
success()
failure()
cancelled()
always()
contains()
startsWith()
endsWith()
format()
toJSON()
fromJSON()
```

Expressions are commonly used for:

```text
Dynamic values
Conditions
Job dependencies
Matrix builds
Step outputs
Deployment decisions
Workflow automation
```

The key idea is:

```text
Context
   ↓
Expression
   ↓
Decision / Value
   ↓
Workflow Action
```

Next topic: GitHub Actions Debugging and Troubleshooting

Commit message:

```text
Add GitHub Actions expressions and contexts guide
```
