
Introduction to GitHub Actions

Introduction

GitHub Actions is GitHub's automation platform.

It allows you to automatically run tasks when something happens in a repository.

Examples:

- Run tests when code is pushed.
- Build an application.
- Check code quality.
- Scan for security issues.
- Deploy an application.
- Automatically perform repetitive tasks.

---

What is GitHub Actions?

GitHub Actions allows developers to create automated workflows inside a GitHub repository.

A simple workflow can be:

Code Push
    ↓
GitHub Actions Starts
    ↓
Install Dependencies
    ↓
Run Tests
    ↓
Build Project
    ↓
Success / Failure

---

Why Use GitHub Actions?

Without automation:

Developer
   ↓
Manually install dependencies
   ↓
Manually run tests
   ↓
Manually build
   ↓
Manually deploy

With GitHub Actions:

Developer
   ↓
git push
   ↓
GitHub Actions
   ↓
Tests + Build + Deployment

Automation saves time and reduces repetitive work.

---

GitHub Actions and CI/CD

GitHub Actions is commonly used for:

CI = Continuous Integration

CD = Continuous Delivery / Continuous Deployment

Continuous Integration

Developers frequently push changes to a shared repository.

Automated workflows can:

- Build the project.
- Run tests.
- Check code quality.
- Perform security checks.

---

Continuous Delivery

Code is automatically prepared for release after passing required checks.

---

Continuous Deployment

Code can automatically be deployed to a production environment after passing the required checks.

---

Basic GitHub Actions Structure

GitHub Actions workflows are stored inside:

.github/workflows/

Example:

repository/
│
├── .github/
│   └── workflows/
│       └── test.yml
│
├── src/
├── README.md
└── ...

---

What is a Workflow?

A workflow is an automated process defined in a YAML file.

Example:

.github/workflows/test.yml

A workflow can contain:

Trigger
   ↓
Jobs
   ↓
Steps

---

Basic Workflow Structure

name: Example Workflow

on:
  push:

jobs:
  example:
    runs-on: ubuntu-latest

    steps:
      - name: Print Message
        run: echo "Hello GitHub Actions"

---

Understanding the Workflow

"name"

name: Example Workflow

Defines the name of the workflow.

---

"on"

on:
  push:

Defines when the workflow should run.

In this example, the workflow runs when code is pushed.

---

"jobs"

jobs:

Defines the tasks that the workflow will perform.

---

"runs-on"

runs-on: ubuntu-latest

Defines the environment where the job runs.

GitHub provides hosted runners that can execute workflows.

---

"steps"

steps:

Defines individual actions performed by the job.

---

"run"

run: echo "Hello GitHub Actions"

Runs a shell command on the runner.

---

Workflow Components

The basic relationship is:

Workflow
    │
    └── Job
          │
          ├── Step
          ├── Step
          └── Step

A workflow can contain multiple jobs.

A job can contain multiple steps.

---

GitHub Actions Runner

A runner is the machine that executes your workflow.

Conceptually:

GitHub
   │
   ▼
Workflow
   │
   ▼
Runner
   │
   ├── Checkout Code
   ├── Install Dependencies
   ├── Run Tests
   └── Build

GitHub provides hosted runners for common operating systems.

Examples include:

Ubuntu
Windows
macOS

---

Actions

An Action is a reusable unit of automation.

For example:

uses: actions/checkout@v4

The checkout Action downloads your repository's code into the runner.

---

"run" vs "uses"

"run"

Executes a command.

run: echo "Hello"

"uses"

Uses a reusable GitHub Action.

uses: actions/checkout@v4

---

Example Workflow

name: Basic CI

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Show files
        run: ls

      - name: Print message
        run: echo "GitHub Actions is working!"

---

What Happens?

When you push code:

git push
   ↓
GitHub Detects Push
   ↓
Workflow Starts
   ↓
Runner Created
   ↓
Repository Checked Out
   ↓
Commands Execute
   ↓
Workflow Finishes

---

Workflow Status

A workflow can have statuses such as:

Queued
In Progress
Success
Failure
Cancelled
Skipped

You can view workflow runs from the repository's:

Actions

tab.

---

Workflow Logs

GitHub provides logs for workflow runs.

Logs help you understand:

- What command ran.
- Which step failed.
- What error occurred.
- How long a step took.

Example:

Build
  ↓
Tests
  ↓
❌ Test failed
  ↓
Open Logs
  ↓
Find Error
  ↓
Fix Code
  ↓
Push Again

---

Common Triggers

GitHub Actions workflows can respond to many repository events.

Examples:

on:
  push:

on:
  pull_request:

on:
  issues:

on:
  workflow_dispatch:

The available events depend on GitHub's workflow event system.

---

Manual Workflow Trigger

A workflow can also be manually triggered using:

on:
  workflow_dispatch:

This allows you to run the workflow from GitHub when needed.

---

GitHub Actions Folder

Your repository should eventually look like:

Git-and-GitHub-Mastery/
│
├── 01-Getting-Started/
├── 02-Git-Basics/
├── 03-GitHub-Basics/
├── ...
├── 08-GitHub-Security/
│
└── 09-GitHub-Actions/
    └── 01-Introduction-to-GitHub-Actions.md

Later, this chapter will contain more advanced workflow files and documentation.

---

GitHub Actions vs Git

Git and GitHub Actions are different.

Git| GitHub Actions
Version control| Automation
Tracks changes| Runs automated tasks
Works locally| Runs workflows on GitHub infrastructure or other configured runners
Uses commits and branches| Uses workflows, jobs, and steps

They work together:

Git
 ↓
Push Code
 ↓
GitHub
 ↓
GitHub Actions
 ↓
Automated Tasks

---

GitHub Actions vs GitHub

GitHub is the platform.

GitHub Actions is an automation feature within GitHub.

GitHub
│
├── Repositories
├── Pull Requests
├── Issues
├── Projects
├── Security
└── Actions

---

Best Practices

- Keep workflows simple.
- Give workflows only the permissions they need.
- Use official or trusted Actions.
- Pin Action versions appropriately.
- Keep secrets out of workflow files.
- Review workflow changes carefully.
- Use CI checks for important projects.

---

Common Mistakes

- Incorrect YAML indentation.
- Using invalid workflow syntax.
- Exposing secrets in logs.
- Giving workflows unnecessary permissions.
- Using untrusted Actions without reviewing them.
- Creating unnecessarily complicated workflows.

---

Interview Questions

What is GitHub Actions?

GitHub Actions is a GitHub automation platform used to build workflows that automatically perform tasks in response to repository events.

What is a workflow?

A workflow is an automated process defined in a YAML file inside ".github/workflows/".

What is a runner?

A runner is the machine or environment that executes a GitHub Actions job.

What is the difference between "run" and "uses"?

"run" executes shell commands, while "uses" invokes a reusable Action.

Where are GitHub Actions workflows stored?

.github/workflows/

---

Practice

Create:

.github/workflows/hello.yml

Add:

name: Hello GitHub Actions

on:
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest

    steps:
      - name: Print Hello
        run: echo "Hello from GitHub Actions!"

Commit:

git add .github/workflows/hello.yml

Commit:

git commit -m "Add first GitHub Actions workflow"

Push:

git push

Then open:

GitHub Repository
      ↓
Actions
      ↓
Hello GitHub Actions
      ↓
Run workflow

Run it manually and open the workflow logs.

---

Summary

GitHub Actions allows you to automate development tasks directly from your GitHub repository.

The fundamental structure is:

Workflow
   ↓
Job
   ↓
Steps
   ↓
Commands / Actions
   ↓
Runner

Once you understand these concepts, you can build automated CI/CD pipelines, testing systems, security checks, deployments, and many other workflows.
