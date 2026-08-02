# GitHub Actions Basics

## Introduction

GitHub Actions is GitHub's built-in automation platform. It allows developers to automatically build, test, and deploy applications whenever specific events occur in a repository.

Instead of performing repetitive tasks manually, GitHub Actions executes them automatically using workflows.

---

# What are GitHub Actions?

GitHub Actions is a Continuous Integration and Continuous Deployment (CI/CD) service provided by GitHub.

It can automatically:

- Build applications
- Run tests
- Check code quality
- Deploy applications
- Send notifications
- Automate repetitive tasks

---

# Why Use GitHub Actions?

Developers use GitHub Actions to:

- Save time
- Reduce manual work
- Catch bugs early
- Automate deployments
- Improve software quality

---

# How GitHub Actions Work

```text
Developer Pushes Code
          │
          ▼
GitHub Event
          │
          ▼
Workflow Starts
          │
          ▼
Runner Executes Jobs
          │
          ▼
Build → Test → Deploy
```

---

# Key Components

## 1. Workflow

A workflow is an automated process.

It is written in a YAML file.

Example location:

```text
.github/workflows/build.yml
```

---

## 2. Event

An event triggers the workflow.

Common events:

- push
- pull_request
- schedule
- workflow_dispatch

Example:

```yaml
on:
  push:
```

---

## 3. Job

A workflow can contain one or more jobs.

Example:

```yaml
jobs:
```

Each job runs independently.

---

## 4. Step

A job consists of multiple steps.

Example:

```yaml
steps:
```

Typical steps:

- Checkout code
- Install dependencies
- Build project
- Run tests

---

## 5. Runner

A runner is the machine that executes the workflow.

GitHub provides:

- Ubuntu
- Windows
- macOS

Example:

```yaml
runs-on: ubuntu-latest
```

---

# Simple Workflow Example

```yaml
name: First Workflow

on:
  push:

jobs:
  build:

    runs-on: ubuntu-latest

    steps:

      - uses: actions/checkout@v4

      - name: Say Hello
        run: echo "Hello GitHub Actions!"
```

---

# Workflow Folder Structure

```text
Repository
│
├── .github
│   └── workflows
│       └── build.yml
│
├── src
├── README.md
└── ...
```

---

# Real-World Example

A developer pushes new code.

GitHub Actions automatically:

1. Downloads the code.
2. Installs dependencies.
3. Builds the application.
4. Runs tests.
5. Reports success or failure.

No manual work is required.

---

# Advantages

- Free for many projects
- Built into GitHub
- Easy automation
- Supports multiple operating systems
- Large marketplace of reusable actions

---

# Best Practices

- Keep workflows simple.
- Store workflows inside `.github/workflows`.
- Use reusable actions.
- Test workflows before production.
- Keep secrets in GitHub Secrets.

---

# Common Mistakes

- Incorrect YAML syntax.
- Hardcoding passwords or API keys.
- Running unnecessary workflows.
- Ignoring failed workflow runs.

---

# Interview Questions

### What is GitHub Actions?

GitHub Actions is GitHub's automation platform for building, testing, and deploying software.

---

### Where are workflow files stored?

```text
.github/workflows/
```

---

### Which language is used to write workflow files?

YAML.

---

### What triggers a workflow?

Events such as:

- push
- pull_request
- schedule
- workflow_dispatch

---

### What is a Runner?

A machine that executes workflow jobs.

---

# Practice

1. Create a GitHub repository.
2. Create:

```text
.github/workflows
```

3. Add a file named:

```text
build.yml
```

4. Paste the sample workflow.

5. Commit and push.

6. Open the **Actions** tab on GitHub.

7. Verify that the workflow runs successfully.

---

# Summary

GitHub Actions is GitHub's built-in automation platform that enables developers to automatically build, test, and deploy software. It is a fundamental DevOps tool and is widely used in professional software development.
