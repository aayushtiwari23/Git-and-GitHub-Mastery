# Using GitHub Actions

## Introduction

GitHub Actions provides reusable components called **Actions**.

Instead of writing every automation task from scratch, you can use existing Actions created by GitHub, organizations, or the community.

A simple example is:

```yaml
- name: Checkout Repository
  uses: actions/checkout@v4
```

---
# What is an Action?

An Action is a reusable piece of automation.

It can perform tasks such as:

- Checking out repository code.
- Setting up programming languages.
- Installing tools.
- Uploading artifacts.
- Downloading artifacts.
- Running security checks.
- Deploying applications.

Conceptually:

```text
Workflow
   ↓
Job
   ↓
Step
   ↓
Action
   ↓
Task Completed
```

---

# `uses`

The `uses` keyword tells GitHub Actions to use an existing Action.

Example:

```yaml
- name: Checkout Repository
  uses: actions/checkout@v4
```

The structure is:

```text
owner/repository@version
```

In this example:

```text
actions       → owner
checkout      → repository
v4            → version
```

---

# GitHub Official Actions

GitHub maintains several commonly used Actions.

Examples include:

```text
actions/checkout
actions/setup-python
actions/setup-node
actions/setup-java
actions/upload-artifact
actions/download-artifact
```

These are commonly used as building blocks in workflows.

---

# Checkout Action

One of the most commonly used Actions is:

```yaml
uses: actions/checkout@v4
```

It checks out the repository so that workflow steps can access the source code.

Example:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Show Files
    run: ls
```

Without checking out the repository, later steps may not have the repository files available in the expected workspace.

---

# Setup Python

For Python projects, you can use:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.12'
```

This prepares the runner with the requested Python version.

---

# Setup Node.js

For Node.js projects:

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
```

This prepares the environment for Node.js development.

---

# Setup Java

For Java projects:

```yaml
- name: Set up Java
  uses: actions/setup-java@v4
  with:
    distribution: 'temurin'
    java-version: '17'
```

This configures a Java environment for the workflow.

---

# Action Inputs

Actions can accept configuration values called **inputs**.

Example:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.12'
```

Here:

```text
with:
    python-version: '3.12'
```

provides an input to the Action.

---

# Action Outputs

Some Actions produce outputs that can be used by later steps.

Conceptually:

```text
Action
  ↓
Output
  ↓
Next Step
```

Outputs are useful when one automation step needs to provide information to another.

---

# Action Versions

Actions are normally referenced using a version or reference.

Example:

```yaml
uses: actions/checkout@v4
```

Here:

```text
v4
```

identifies the major version reference.

You should understand what version or reference you are using before adding an Action to an important workflow.

---

# Why Versioning Matters

Actions can change over time.

Using an explicit version helps make workflows more predictable.

Example:

```yaml
uses: actions/checkout@v4
```

is clearer than depending on an unspecified reference.

For security-sensitive environments, organizations may use stricter pinning strategies.

---

# Marketplace Actions

GitHub provides a marketplace where developers can discover reusable Actions.

Before using a third-party Action, review:

- Who maintains it.
- Repository activity.
- Documentation.
- Versioning.
- Permissions.
- Security history.
- What code the Action executes.

Do not blindly add an Action just because it appears in a search result.

---

# Trusted Actions

Prefer Actions from:

```text
GitHub
Well-known organizations
Trusted maintainers
```

Review third-party Actions carefully.

An Action can execute code on your runner, so using an untrusted Action can create security risks.

---

# Example Workflow

```yaml
name: Using Actions

on:
  workflow_dispatch:

jobs:
  setup:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Show Python Version
        run: python --version
```

Workflow:

```text
Workflow
   ↓
Runner
   ↓
Checkout Action
   ↓
Python Setup Action
   ↓
Python Command
```

---

# Combining Actions and Commands

A workflow can use both `uses` and `run`.

Example:

```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4

  - name: Set up Python
    uses: actions/setup-python@v5
    with:
      python-version: '3.12'

  - name: Install Dependencies
    run: pip install -r requirements.txt

  - name: Run Tests
    run: python -m pytest
```

This is one of the most common GitHub Actions patterns.

---

# Action vs Command

Consider:

```yaml
- name: Show Python Version
  run: python --version
```

This directly executes a command.

Whereas:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
```

uses reusable automation.

The difference is:

```text
run
 ↓
Execute command

uses
 ↓
Use an Action
```

---

# Composite Actions

You can create your own reusable Action using a **composite action**.

Conceptually:

```text
Reusable Action
      │
      ├── Step 1
      ├── Step 2
      └── Step 3
```

This is useful when the same group of steps needs to be reused.

---

# Reusable Workflows vs Actions

These concepts are related but different.

### Action

A reusable automation component used as a step.

```text
Job
 ↓
Step
 ↓
Action
```

### Reusable Workflow

A complete workflow that can be called by another workflow.

```text
Workflow A
    ↓
Reusable Workflow B
```

---

# Common Action Workflow

A typical application workflow might look like:

```text
Checkout
   ↓
Setup Language
   ↓
Install Dependencies
   ↓
Run Tests
   ↓
Build
   ↓
Upload Artifact
```

Example:

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v4

  - name: Set up Node.js
    uses: actions/setup-node@v4
    with:
      node-version: '20'

  - name: Install Dependencies
    run: npm install

  - name: Run Tests
    run: npm test
```

---

# Security Considerations

Actions execute code in your workflow environment.

Therefore:

```text
Unknown Action
      ↓
Could Execute Malicious Code
      ↓
Potential Security Risk
```

Be especially careful if the workflow has access to:

- Secrets.
- Cloud credentials.
- Repository write permissions.
- Deployment systems.
- Production environments.

---

# Permissions

Workflows should use the minimum permissions required.

Example:

```yaml
permissions:
  contents: read
```

This limits the workflow's repository content permission.

Do not give write permissions unless they are actually required.

---

# Pinning Actions

For stronger supply-chain security, some teams pin Actions to a specific commit SHA rather than relying only on a mutable tag.

Conceptually:

```yaml
uses: owner/action@commit-sha
```

This makes the exact Action revision explicit.

However, SHA pinning requires maintenance when updating Action versions.

---

# Updating Actions

Actions should not be left outdated forever.

A reasonable process is:

```text
Current Version
      ↓
Check New Version
      ↓
Review Changes
      ↓
Update Workflow
      ↓
Run Tests
      ↓
Merge
```

---

# Common Mistakes

Avoid:

```text
Using random Actions without review
Using outdated versions indefinitely
Giving Actions unnecessary permissions
Exposing secrets unnecessarily
Copying workflow code without understanding it
```

---

# Best Practices

- Prefer trusted Actions.
- Review third-party Actions.
- Use clear versions.
- Keep Actions updated.
- Use minimum permissions.
- Avoid unnecessary secrets.
- Test workflow changes.
- Understand what each Action actually does.

---

# Interview Questions

### What is a GitHub Action?

A reusable piece of automation that performs a specific task inside a GitHub Actions workflow.

### What does `uses` do?

It tells a workflow step to use an existing Action.

### What is `actions/checkout` used for?

It checks out the repository's source code into the workflow runner.

### What does `with` do?

It provides input parameters to an Action.

### Why should third-party Actions be reviewed?

Because Actions execute code in the workflow environment and may have access to repository resources or secrets.

### What is the difference between `run` and `uses`?

`run` executes a shell command, while `uses` invokes a reusable Action.

---

# Practice

Create:

```text
.github/workflows/actions-practice.yml
```

Paste:

```yaml
name: Actions Practice

on:
  workflow_dispatch:

jobs:
  practice:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'

      - name: Show Python Version
        run: python --version

      - name: Show Repository Files
        run: ls
```

Commit message:

```text
Add GitHub Actions reusable actions guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Actions Practice
   ↓
Run workflow
```

Open the workflow run and inspect each step.

---

# Summary

GitHub Actions provides reusable automation through Actions.

The most important concepts are:

```text
uses
with
Actions
Action Versions
Trusted Actions
Permissions
```

A typical workflow combines reusable Actions with shell commands:

```text
Checkout
   ↓
Setup Environment
   ↓
Install Dependencies
   ↓
Run Commands
   ↓
Test
   ↓
Build
```

Understanding how to use Actions is an important step toward building real CI/CD workflows.
