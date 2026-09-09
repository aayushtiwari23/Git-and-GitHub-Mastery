# GitHub Actions Composite Actions

## 1. What Is a Composite Action?

A composite action allows you to combine multiple GitHub Actions steps into one reusable action.

Instead of repeating the same steps in many workflows, you can create one composite action and reuse it.

Example:

```text
Workflow
   ↓
Composite Action
   ↓
Step 1
Step 2
Step 3
```

---

## 2. Why Use Composite Actions?

Suppose several workflows always run:

```text
Checkout code
Install dependencies
Run setup commands
Run tests
```

Instead of copying these steps everywhere, create one composite action.

Benefits:

- Less duplicate code
- Easier maintenance
- Reusable steps
- Cleaner workflows
- Consistent processes

---

## 3. Composite Action Location

A composite action can be stored in a directory such as:

```text
.github/actions/setup-project/
```

Inside it:

```text
.github/actions/setup-project/
    action.yml
```

The main file is:

```text
action.yml
```

---

## 4. Basic Composite Action

Example:

```yaml
name: Setup Project
description: Common project setup steps

runs:
  using: composite

  steps:
    - name: Step 1
      run: echo "Setting up project"
      shell: bash

    - name: Step 2
      run: echo "Setup complete"
      shell: bash
```

The important part is:

```yaml
runs:
  using: composite
```

This tells GitHub that the action is a composite action.

---

## 5. Using a Composite Action

Suppose the action is located at:

```text
.github/actions/setup-project/action.yml
```

A workflow can use it:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Project
        uses: ./.github/actions/setup-project
```

The workflow now runs all the steps defined inside the composite action.

---

## 6. Complete Example

Create:

```text
.github/actions/setup-project/action.yml
```

Use:

```yaml
name: Setup Project
description: Basic project setup

runs:
  using: composite

  steps:
    - name: Show message
      run: echo "Starting project setup"
      shell: bash

    - name: Show repository
      run: echo "Repository setup completed"
      shell: bash
```

Then create a workflow:

```text
.github/workflows/main.yml
```

Use:

```yaml
name: Composite Action Demo

on:
  workflow_dispatch:

jobs:
  setup:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run Composite Action
        uses: ./.github/actions/setup-project
```

---

## 7. Composite Action Inputs

Composite actions can accept inputs.

Example:

```yaml
name: Greeting Action
description: Prints a greeting

inputs:
  name:
    description: Name of the person
    required: true

runs:
  using: composite

  steps:
    - name: Greeting
      run: echo "Hello ${{ inputs.name }}"
      shell: bash
```

---

## 8. Passing Inputs

The workflow can provide the input:

```yaml
- name: Greeting
  uses: ./.github/actions/greeting
  with:
    name: Aayush
```

The action receives:

```text
Aayush
```

Output:

```text
Hello Aayush
```

---

## 9. Optional Inputs

Inputs can have defaults.

Example:

```yaml
inputs:
  name:
    description: Name
    required: false
    default: GitHub User
```

If the workflow doesn't provide a value, the default is used.

---

## 10. Environment Variables

Composite action steps can use environment variables.

Example:

```yaml
runs:
  using: composite

  steps:
    - name: Show environment
      run: echo "Environment is $ENVIRONMENT"
      shell: bash
      env:
        ENVIRONMENT: development
```

---

## 11. Running Shell Commands

Every `run` step in a composite action should specify an appropriate shell.

Example:

```yaml
- name: Run command
  run: echo "Hello"
  shell: bash
```

For Windows PowerShell:

```yaml
- name: Run PowerShell command
  run: Write-Host "Hello"
  shell: pwsh
```

---

## 12. Using Existing Actions Inside a Composite Action

Composite actions can include normal actions.

Example:

```yaml
runs:
  using: composite

  steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Setup
      run: echo "Project setup"
      shell: bash
```

This allows you to combine existing actions with your own commands.

---

## 13. Composite Action With Multiple Steps

Example:

```yaml
name: Project Setup
description: Common project setup

runs:
  using: composite

  steps:
    - name: Install dependencies
      run: echo "Installing dependencies"
      shell: bash

    - name: Run tests
      run: echo "Running tests"
      shell: bash

    - name: Build project
      run: echo "Building project"
      shell: bash
```

One `uses` statement can now execute all these steps.

---

## 14. Composite Action vs Reusable Workflow

These are different.

### Composite Action

Used mainly to reuse steps.

```text
Composite Action
      ↓
   Steps
```

Example:

```yaml
- uses: ./.github/actions/setup
```

### Reusable Workflow

Used to reuse jobs and workflow logic.

```text
Reusable Workflow
       ↓
      Jobs
       ↓
     Steps
```

Example:

```yaml
jobs:
  build:
    uses: ./.github/workflows/reusable.yml
```

Simple rule:

```text
Reuse steps → Composite Action

Reuse jobs/workflow → Reusable Workflow
```

---

## 15. Composite Actions vs Scripts

A normal script might look like:

```bash
./setup.sh
```

A composite action can package the script together with other GitHub Actions steps and inputs.

This makes it easier to integrate reusable setup logic directly into GitHub Actions workflows.

---

## 16. Repository Structure

A simple project can contain:

```text
.github/
    actions/
        setup-project/
            action.yml
    workflows/
        main.yml
```

Keep the structure simple and organized.

---

## 17. Practice

Create:

```text
.github/actions/greeting/action.yml
```

Use:

```yaml
name: Greeting
description: Greeting composite action

inputs:
  name:
    description: Name
    required: true

runs:
  using: composite

  steps:
    - name: Say hello
      run: echo "Hello ${{ inputs.name }}"
      shell: bash
```

Then use it in a workflow:

```yaml
- name: Greeting
  uses: ./.github/actions/greeting
  with:
    name: GitHub
```

Expected output:

```text
Hello GitHub
```

---

## 18. Challenge

Create a composite action called:

```text
project-check
```

It should:

1. Display a setup message.
2. Display the current operating system.
3. Display the current working directory.
4. Run a test command.
5. Display a completion message.

Then call the composite action from a GitHub Actions workflow.

---

## 19. Best Practices

1. Give the action a clear name.
2. Add a useful description.
3. Use inputs for values that may change.
4. Keep actions focused on a specific purpose.
5. Avoid unnecessary complexity.
6. Specify the correct shell.
7. Don't hard-code secrets.
8. Document important inputs.
9. Test the action before using it widely.
10. Version shared actions when appropriate.

---

## 20. Security

Never hard-code secrets inside:

```text
action.yml
```

Avoid:

```yaml
run: echo "my-secret-password"
```

Use GitHub Secrets when sensitive information is required.

Also be careful when executing user-controlled input inside shell commands.

---

## 21. Interview Questions

### Q1. What is a composite action?

A composite action combines multiple workflow steps into one reusable action.

### Q2. Where is a composite action commonly defined?

In an `action.yml` file.

### Q3. What identifies a composite action?

```yaml
runs:
  using: composite
```

### Q4. How do you use a local composite action?

```yaml
uses: ./.github/actions/action-name
```

### Q5. Can composite actions accept inputs?

Yes.

Inputs are defined under:

```yaml
inputs:
```

### Q6. What is the difference between a composite action and reusable workflow?

A composite action primarily reuses steps, while a reusable workflow reuses jobs and workflow logic.

### Q7. Can a composite action use other actions?

Yes, composite actions can contain `uses` steps as well as `run` steps.

---

# Summary

A composite action packages multiple GitHub Actions steps into one reusable action.

Basic structure:

```yaml
name: My Action
description: My reusable action

runs:
  using: composite

  steps:
    - name: Step
      run: echo "Hello"
      shell: bash
```

Use it with:

```yaml
- uses: ./.github/actions/my-action
```

Remember:

```text
Composite Action
       ↓
Reusable Steps
```

while:

```text
Reusable Workflow
       ↓
Reusable Jobs
```

Composite actions are useful when the same collection of steps needs to be reused across multiple workflows.

Next topic: GitHub Actions Custom Actions
