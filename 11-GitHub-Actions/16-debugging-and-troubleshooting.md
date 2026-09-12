
# GitHub Actions Debugging and Troubleshooting

## 1. What Is Debugging?

Debugging means finding and fixing problems in a GitHub Actions workflow.

A workflow can fail because of:

- YAML syntax
- Incorrect commands
- Missing permissions
- Wrong secrets
- Incorrect paths
- Dependency problems
- Environment differences
- Failed tests
- Incorrect conditions
- Action configuration

---

## 2. First Step When a Workflow Fails

Go to:

```text
GitHub Repository
    ↓
Actions
    ↓
Select Workflow
    ↓
Select Failed Run
    ↓
Select Failed Job
    ↓
Read Logs
```

Always start with the actual error message.

Don't immediately rewrite the entire workflow.

---

## 3. Understanding Workflow Status

Common statuses include:

```text
Queued
In progress
Success
Failure
Cancelled
Skipped
```

### Queued

The workflow is waiting for a runner.

### In progress

The workflow is currently executing.

### Success

All required work completed successfully.

### Failure

A step or job failed.

### Cancelled

The workflow was stopped.

### Skipped

A job or step condition evaluated to false.

---

## 4. Reading Logs

Logs are one of the most important debugging tools.

Example:

```text
Run npm test

Error: Cannot find module
```

The important part is:

```text
Cannot find module
```

Start debugging from the first meaningful error rather than random lines near the bottom.

---

## 5. Common YAML Errors

Incorrect indentation:

```yaml
jobs:
build:
  runs-on: ubuntu-latest
```

Correct:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

YAML indentation matters.

---

## 6. Invalid Workflow Structure

Incorrect:

```yaml
jobs:
  build:
  steps:
    - run: echo "Hello"
```

Correct:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello"
```

A job generally needs a runner when it contains normal execution steps.

---

## 7. Debugging With Simple Messages

Add temporary messages:

```yaml
- name: Debug
  run: echo "Reached this step"
```

You can also display variables:

```yaml
- name: Debug
  run: |
    echo "Branch: $GITHUB_REF"
    echo "Runner: $RUNNER_OS"
```

Remove unnecessary debugging output after fixing the problem.

---

## 8. Checking Environment Variables

You can inspect non-sensitive environment information.

Example:

```yaml
- name: Environment information
  run: |
    echo "OS: $RUNNER_OS"
    echo "Architecture: $RUNNER_ARCH"
```

Never print secrets.

Avoid:

```yaml
echo "${{ secrets.API_KEY }}"
```

---

## 9. Debugging Paths

Many workflow failures are caused by incorrect file paths.

Example:

```yaml
- name: List files
  run: ls -la
```

For deeper inspection:

```yaml
- name: Show directories
  run: find . -maxdepth 2 -type f
```

On Windows, commands may differ.

---

## 10. Checkout Problems

If your workflow needs repository files, make sure checkout happens first.

Example:

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: List files
    run: ls -la
```

Without checkout, the repository files may not be available in the expected location.

---

## 11. Dependency Problems

A common failure is missing dependencies.

Example:

```text
Module not found
Package not found
Command not found
```

For Node.js:

```yaml
- name: Install dependencies
  run: npm ci
```

For Python:

```yaml
- name: Install dependencies
  run: pip install -r requirements.txt
```

Make sure the correct runtime version is installed first.

---

## 12. Runtime Version Problems

Your local machine might use one version while GitHub Actions uses another.

Example:

```text
Local:
Node.js 24

GitHub Actions:
Node.js 20
```

This can cause unexpected failures.

Use setup actions to explicitly define versions.

Example:

```yaml
- name: Setup Node
  uses: actions/setup-node@v4
  with:
    node-version: 24
```

---

## 13. Permission Problems

A workflow may fail because it doesn't have sufficient permissions.

Example:

```text
Resource not accessible
Permission denied
403 Forbidden
```

You can define required permissions.

Example:

```yaml
permissions:
  contents: read
```

Only request permissions that the workflow actually needs.

---

## 14. Secret Problems

Possible errors:

```text
Secret is empty
Authentication failed
Invalid token
Unauthorized
```

Check:

- Secret name
- Repository
- Environment
- Organization settings
- Workflow references

Example:

```yaml
${{ secrets.API_TOKEN }}
```

Make sure the configured secret is actually named:

```text
API_TOKEN
```

---

## 15. Secret Security

Never debug secrets by printing them.

Bad:

```yaml
- run: echo "${{ secrets.API_TOKEN }}"
```

Better:

```yaml
- name: Test authentication
  run: ./deploy.sh
  env:
    API_TOKEN: ${{ secrets.API_TOKEN }}
```

The application can use the secret without exposing it in logs.

---

## 16. Debugging Conditions

A job or step may be skipped because its `if` condition is false.

Example:

```yaml
if: github.ref == 'refs/heads/main'
```

If the workflow runs on another branch, the step will be skipped.

Add a temporary debugging step:

```yaml
- name: Show branch
  run: echo "$GITHUB_REF"
```

Then verify that the condition matches the actual value.

---

## 17. Debugging Job Dependencies

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploy"
```

If `build` fails, `deploy` normally won't run.

Check the status of the dependency first.

---

## 18. Debugging Matrix Builds

Matrix workflows can create multiple jobs.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest
```

If one environment fails:

```text
ubuntu-latest → Success
windows-latest → Failure
```

Check the failing matrix combination.

Display the current value:

```yaml
- name: Show OS
  run: echo "Running on ${{ matrix.os }}"
```

---

## 19. Shell Differences

Commands can behave differently across operating systems.

Linux:

```yaml
shell: bash
```

Windows PowerShell:

```yaml
shell: pwsh
```

For example, this Linux command:

```bash
ls
```

may not behave the same way in every Windows shell environment.

When using multiple operating systems, account for shell differences.

---

## 20. Debugging Artifacts

If a workflow creates files that you need to inspect after failure, upload them as artifacts.

Example:

```yaml
- name: Upload logs
  if: failure()
  uses: actions/upload-artifact@v4
  with:
    name: logs
    path: logs/
```

This can help inspect files generated during the failed run.

---

## 21. Debugging With `if: failure()`

A diagnostic step can run after a failure.

Example:

```yaml
- name: Debug after failure
  if: failure()
  run: echo "A previous step failed"
```

You can use this for cleanup or diagnostics.

---

## 22. Step IDs and Outputs

If an output isn't available, check the step ID.

Correct:

```yaml
- name: Generate value
  id: result
  run: echo "value=hello" >> "$GITHUB_OUTPUT"
```

Access:

```yaml
${{ steps.result.outputs.value }}
```

If you use the wrong ID:

```yaml
${{ steps.output.outputs.value }}
```

the expected value won't be available.

---

## 23. Debugging Expressions

Suppose you have:

```yaml
if: github.ref == 'refs/heads/main'
```

Debug the value:

```yaml
- name: Show ref
  run: echo "${{ github.ref }}"
```

Then compare the actual value with your expression.

---

## 24. Debug Logging

GitHub Actions supports additional debugging features.

Two useful runner debug settings are:

```text
ACTIONS_STEP_DEBUG
ACTIONS_RUNNER_DEBUG
```

These can provide more detailed information during troubleshooting.

Use debug logging only when needed because logs can become very large.

---

## 25. Re-run Failed Jobs

After fixing a problem, you can re-run a workflow from GitHub.

Typical options include:

```text
Re-run all jobs
Re-run failed jobs
```

Use the appropriate option depending on the situation.

---

## 26. Local vs GitHub Differences

A workflow may work locally but fail on GitHub.

Possible reasons:

```text
Different OS
Different runtime version
Different environment variables
Missing dependencies
Missing secrets
Different permissions
Different filesystem behavior
```

Never assume that your local environment exactly matches the GitHub runner.

---

## 27. Common Errors

### Error: Command not found

Possible causes:

- Tool isn't installed.
- PATH is incorrect.
- Wrong runner.
- Wrong shell.

### Error: Permission denied

Possible causes:

- File permissions.
- Repository permissions.
- Workflow token permissions.

### Error: Authentication failed

Possible causes:

- Missing secret.
- Incorrect token.
- Expired credential.
- Incorrect configuration.

### Error: File not found

Possible causes:

- Wrong path.
- Checkout missing.
- File wasn't generated.
- Wrong working directory.

### Error: Tests failed

Possible causes:

- Code failure.
- Dependency mismatch.
- Environment difference.
- Incorrect test configuration.

---

## 28. A Simple Debugging Process

Use this process:

```text
Workflow fails
     ↓
Read failed job
     ↓
Find first meaningful error
     ↓
Identify category
     ↓
Check configuration
     ↓
Add temporary debug information
     ↓
Fix problem
     ↓
Re-run workflow
     ↓
Confirm success
```

---

## 29. Debugging Checklist

Before asking for help, check:

```text
[ ] YAML indentation
[ ] Workflow trigger
[ ] Job name
[ ] runs-on
[ ] Checkout
[ ] File paths
[ ] Runtime version
[ ] Dependencies
[ ] Secrets
[ ] Variables
[ ] Permissions
[ ] if conditions
[ ] needs dependencies
[ ] Matrix values
[ ] Shell
[ ] Action versions
```

---

## 30. Practice

Create a workflow with an intentional error.

For example:

```yaml
- name: Test
  run: npm test
```

without installing dependencies.

Run the workflow.

Then:

1. Read the error.
2. Identify the cause.
3. Add the dependency installation step.
4. Run the workflow again.
5. Confirm success.

---

## 31. Challenge

Create a workflow with:

```text
Build
   ↓
Test
   ↓
Deploy
```

Intentionally introduce one error in the test job.

Then:

1. Find the failed job.
2. Read the logs.
3. Add a failure debugging step.
4. Fix the error.
5. Re-run the workflow.
6. Confirm that deployment runs successfully.

---

## 32. Important Debugging Rules

Remember:

1. Read logs before changing code.
2. Find the first meaningful error.
3. Check YAML indentation.
4. Verify paths.
5. Check runtime versions.
6. Check dependencies.
7. Check permissions.
8. Never print secrets.
9. Check conditions carefully.
10. Check job dependencies.
11. Consider OS and shell differences.
12. Remove temporary debugging output when finished.

---

## 33. Interview Questions

### Q1. How do you debug a failed GitHub Actions workflow?

Open the failed workflow run, inspect the failed job and step logs, identify the error, fix the cause, and re-run the workflow.

### Q2. What is the first thing you should check after a workflow failure?

The job and step logs containing the actual error.

### Q3. Why can a workflow work locally but fail on GitHub?

The environments may differ in OS, runtime versions, dependencies, permissions, environment variables, or filesystem behavior.

### Q4. How can you run a step only after a failure?

Use:

```yaml
if: failure()
```

### Q5. How can you inspect non-sensitive environment information?

Use commands or expressions to display safe values such as the runner OS or Git reference.

### Q6. Why shouldn't you print secrets while debugging?

Because workflow logs can expose sensitive credentials.

### Q7. How can artifacts help with debugging?

Files such as logs, reports, or test results can be uploaded and inspected after the workflow finishes.

### Q8. What can cause a job to be skipped?

An `if` condition may evaluate to false, or a required dependency may not have completed successfully.

---

# Summary

Debugging GitHub Actions is mainly about understanding the workflow logs and identifying the actual cause of failure.

Remember this process:

```text
Failure
   ↓
Logs
   ↓
Error
   ↓
Cause
   ↓
Fix
   ↓
Re-run
   ↓
Success
```

The most important debugging areas are:

```text
YAML
Paths
Dependencies
Runtime versions
Secrets
Permissions
Conditions
Job dependencies
Matrix builds
Shell differences
Action versions
```

Good debugging is not about randomly changing the workflow.

It is about:

```text
Observe → Identify → Fix → Verify
```

Next topic: GitHub Actions Advanced Security and Best Practices

Commit message:

```text
Add GitHub Actions debugging and troubleshooting guide
```
