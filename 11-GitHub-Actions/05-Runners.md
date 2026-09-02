# GitHub Actions Runners

## 1. What is a Runner?

A runner is the machine that actually executes the jobs defined in a GitHub Actions workflow.

For example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Print message
        run: echo "Hello from GitHub Actions"
```

Here:

```yaml
runs-on: ubuntu-latest
```

tells GitHub which runner environment should execute the job.

The runner:

1. Starts the job
2. Downloads the repository
3. Executes the steps
4. Produces logs and outputs
5. Finishes the job

---

# 2. Why Do We Need Runners?

A workflow is only a set of instructions.

The runner is the machine that performs those instructions.

For example:

```yaml
steps:
  - run: python app.py
  - run: npm install
  - run: npm test
```

These commands need an operating system and an environment where they can run.

The runner provides that environment.

---

# 3. GitHub-Hosted Runners

GitHub provides machines that can execute your workflows.

Common runner labels include:

```yaml
ubuntu-latest
windows-latest
macos-latest
```

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - run: echo "Running on Linux"
```

GitHub manages the underlying machine for you.

You don't need to manually create or maintain the machine.

---

# 4. Operating Systems

You can choose different operating systems depending on your project.

## Ubuntu

```yaml
runs-on: ubuntu-latest
```

Useful for:

- Python
- Node.js
- Java
- C/C++
- Backend applications
- Linux-based testing

---

## Windows

```yaml
runs-on: windows-latest
```

Useful for:

- Windows-specific applications
- .NET projects
- PowerShell
- Windows compatibility testing

---

## macOS

```yaml
runs-on: macos-latest
```

Useful for:

- macOS applications
- Apple development
- Testing software on macOS

---

# 5. Choosing a Runner

The runner is selected using:

```yaml
runs-on:
```

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

Another example:

```yaml
jobs:
  test:
    runs-on: windows-latest
```

The operating system should match the requirements of your project.

---

# 6. Each Job Gets Its Own Runner

Consider:

```yaml
jobs:

  build:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Building..."

  test:
    runs-on: ubuntu-latest

    steps:
      - run: echo "Testing..."
```

The two jobs are separate.

Each job gets its own runner environment.

Files created during one job should not be assumed to automatically exist in another job.

If files need to move between jobs, use mechanisms such as:

- Artifacts
- Job outputs
- External storage

Artifacts will be covered later.

---

# 7. Fresh Environments

GitHub-hosted runners are generally provided as clean environments for jobs.

For example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - run: touch test.txt
      - run: ls
```

The file `test.txt` exists during this job.

But you should not expect it to automatically exist in another independent job.

This makes workflows more predictable and reduces dependency on previous runs.

---

# 8. Runner Labels

Runner labels identify what type of runner should execute a job.

For example:

```yaml
runs-on: ubuntu-latest
```

Here:

```text
ubuntu-latest
```

is the runner label.

Self-hosted runners can also have custom labels.

Example:

```yaml
runs-on:
  - self-hosted
  - linux
```

This tells GitHub to find an appropriate self-hosted runner matching those labels.

---

# 9. Self-Hosted Runners

A self-hosted runner is a machine that you manage yourself.

It can be:

- Your own computer
- A company server
- A virtual machine
- A cloud machine

Instead of GitHub providing the machine, you provide and maintain it.

Conceptually:

```text
GitHub Repository
       |
       v
GitHub Actions
       |
       v
Your Self-Hosted Runner
       |
       v
Execute Job
```

---

# 10. Why Use Self-Hosted Runners?

Self-hosted runners can be useful when you need:

- Custom software
- Special hardware
- Private network access
- Specialized development environments
- Internal company resources
- More control over the machine

For example, a company may need a workflow to access an internal server that isn't publicly accessible.

A self-hosted runner can be configured inside the company's infrastructure.

---

# 11. GitHub-Hosted vs Self-Hosted

| Feature | GitHub-Hosted | Self-Hosted |
|---|---|---|
| Machine management | GitHub | You |
| Setup | Easier | More involved |
| Maintenance | GitHub handles it | You handle it |
| Customization | Limited | High |
| Hardware control | Limited | High |
| Security responsibility | Mostly GitHub | Mostly you |
| Infrastructure cost | Depends on usage/plan | You provide infrastructure |

---

# 12. Checking Runner Information

GitHub provides runner context information.

Example:

```yaml
jobs:
  info:
    runs-on: ubuntu-latest

    steps:
      - name: Runner information
        run: |
          echo "OS: $RUNNER_OS"
          echo "Architecture: $RUNNER_ARCH"
          echo "Runner name: $RUNNER_NAME"
```

You can also access GitHub Actions contexts.

Example:

```yaml
jobs:
  info:
    runs-on: ubuntu-latest

    steps:
      - run: |
          echo "OS: ${{ runner.os }}"
          echo "Architecture: ${{ runner.arch }}"
          echo "Name: ${{ runner.name }}"
```

These values can help when debugging workflows.

---

# 13. Shell Differences

Commands can behave differently depending on the operating system.

For example:

Linux:

```yaml
run: |
  echo "Hello"
  pwd
  ls
```

Windows PowerShell:

```yaml
run: |
  Write-Host "Hello"
  Get-Location
  Get-ChildItem
```

Therefore, a workflow designed for Linux may require changes before running on Windows.

---

# 14. Testing Multiple Operating Systems

Sometimes you want to make sure your application works on different operating systems.

You can create separate jobs:

```yaml
jobs:

  linux:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Linux test"

  windows:
    runs-on: windows-latest
    steps:
      - run: echo "Windows test"

  macos:
    runs-on: macos-latest
    steps:
      - run: echo "macOS test"
```

These jobs can run independently.

Later, we will learn how to do this more efficiently using matrix builds.

---

# 15. Runner Security

Runner security is extremely important.

Be careful when workflows execute code from untrusted sources.

For example:

```yaml
run: |
  echo "${{ github.event.pull_request.title }}"
```

You should understand where values come from before placing them directly into shell commands.

Self-hosted runners require extra care because the machine belongs to you.

A malicious workflow could potentially affect the machine or resources accessible from it.

Important practices include:

- Use least-privilege permissions
- Avoid exposing sensitive credentials
- Keep runners updated
- Restrict who can use self-hosted runners
- Avoid running untrusted code on sensitive machines
- Separate sensitive infrastructure from general CI workloads

---

# 16. Runner Selection Example

A simple Python workflow:

```yaml
name: Python Test

on:
  push:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Check Python
        run: python --version

      - name: Run test
        run: python -m unittest
```

The important part is:

```yaml
runs-on: ubuntu-latest
```

The job executes on a Linux-based GitHub-hosted runner.

---

# 17. Complete Runner Information Workflow

```yaml
name: Runner Information

on:
  workflow_dispatch:

jobs:
  info:
    runs-on: ubuntu-latest

    steps:
      - name: Runner details
        run: |
          echo "Runner OS: ${{ runner.os }}"
          echo "Runner Architecture: ${{ runner.arch }}"
          echo "Runner Name: ${{ runner.name }}"

      - name: System information
        run: |
          uname -a
          pwd
```

Run this workflow manually from GitHub.

Observe the logs.

---

# 18. Practice

Create:

```text
11-GitHub-Actions/05-Runners.md
```

Then create a workflow:

```text
.github/workflows/runner-info.yml
```

Use:

```yaml
name: Runner Information

on:
  workflow_dispatch:

jobs:
  runner-info:
    runs-on: ubuntu-latest

    steps:
      - name: Display runner information
        run: |
          echo "OS: ${{ runner.os }}"
          echo "Architecture: ${{ runner.arch }}"
          echo "Runner: ${{ runner.name }}"

      - name: Display system information
        run: |
          uname -a
          pwd
          ls
```

Go to:

```text
GitHub
→ Actions
→ Runner Information
→ Run workflow
```

Check the logs.

---

# 19. Challenge

Modify the workflow so that it has two jobs:

```text
linux
windows
```

The Linux job should print:

```text
Linux runner
```

The Windows job should print:

```text
Windows runner
```

Use:

```yaml
runs-on: ubuntu-latest
```

for Linux and:

```yaml
runs-on: windows-latest
```

for Windows.

Observe how the environments differ.

---

# 20. Important Concepts

Remember these:

```text
Runner
    ↓
Machine that executes a job

GitHub-hosted runner
    ↓
Machine managed by GitHub

Self-hosted runner
    ↓
Machine managed by you

runs-on
    ↓
Selects the runner environment

runner labels
    ↓
Identify suitable runners

Separate jobs
    ↓
Use separate runner environments
```

---

# 21. Interview Questions

### Q1. What is a GitHub Actions runner?

A runner is the machine or environment that executes jobs in a GitHub Actions workflow.

### Q2. What does `runs-on` do?

It specifies the runner environment on which a job should execute.

### Q3. Give examples of GitHub-hosted runners.

```text
ubuntu-latest
windows-latest
macos-latest
```

### Q4. What is a self-hosted runner?

A self-hosted runner is a machine managed by the user or organization that executes GitHub Actions jobs.

### Q5. Why would you use a self-hosted runner?

For custom software, special hardware, private infrastructure, internal network access, or greater control over the execution environment.

### Q6. Do separate jobs automatically share files?

No. Each job has its own runner environment. Use artifacts, outputs, or external storage when files need to be transferred.

### Q7. Why is runner security important?

Because workflows execute commands on runners. A compromised or malicious workflow can potentially access resources available to the runner.

---

# 22. Summary

A runner is the execution environment for a GitHub Actions job.

The most important setting is:

```yaml
runs-on: ubuntu-latest
```

GitHub-hosted runners are managed by GitHub, while self-hosted runners are managed by you.

Important concepts:

- Runners execute jobs
- `runs-on` selects the runner
- Ubuntu, Windows, and macOS runners are available
- Jobs use separate runner environments
- Self-hosted runners provide more control
- Runner labels help select suitable machines
- Different operating systems may require different commands
- Runner security is important
- Artifacts can be used to transfer files between jobs

Next topic: **GitHub Actions Secrets and Variables**
