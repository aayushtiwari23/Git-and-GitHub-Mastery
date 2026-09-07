
```text
Ubuntu + Version 1
Ubuntu + Version 2
Windows + Version 1
Windows + Version 2
```

Total:

```text
2 × 2 = 4 jobs
```

---

## 7. Matrix With OS and Python

Example:

```yaml
jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

        python:
          - "3.11"
          - "3.12"

    runs-on: ${{ matrix.os }}

    steps:
      - name: Test configuration
        run: |
          echo "Operating System: ${{ matrix.os }}"
          echo "Python: ${{ matrix.python }}"
```

This creates:

```text
Ubuntu + Python 3.11
Ubuntu + Python 3.12
Windows + Python 3.11
Windows + Python 3.12
```

---

## 8. Matrix Values Inside Steps

Matrix values can be accessed using:

```yaml
${{ matrix.name }}
```

Example:

```yaml
strategy:
  matrix:
    environment:
      - development
      - staging
      - production
```

Use:

```yaml
run: echo "Environment: ${{ matrix.environment }}"
```

---

## 9. Matrix and `runs-on`

A common pattern is:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

runs-on: ${{ matrix.os }}
```

The matrix determines the runner.

For the first job:

```text
matrix.os = ubuntu-latest
```

For the second:

```text
matrix.os = windows-latest
```

---

## 10. Complete Example

```yaml
name: Matrix Demo

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

    runs-on: ${{ matrix.os }}

    steps:
      - name: Show operating system
        run: echo "Running on ${{ matrix.os }}"
```

Run the workflow.

You should see separate jobs for:

```text
Ubuntu
Windows
```

---

## 11. Matrix With Three Operating Systems

```yaml
name: Multi OS Test

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest
          - macos-latest

    runs-on: ${{ matrix.os }}

    steps:
      - name: Test
        run: echo "Testing on ${{ matrix.os }}"
```

This creates three job runs.

---

## 12. Matrix With Multiple Variables

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    node:
      - "18"
      - "20"
```

The combinations are:

```text
Ubuntu + Node 18
Ubuntu + Node 20
Windows + Node 18
Windows + Node 20
```

This is called the matrix product.

---

## 13. `include`

Sometimes you want to add a specific configuration to the matrix.

You can use:

```yaml
include:
```

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    version:
      - 1
      - 2

    include:
      - os: ubuntu-latest
        version: 3
```

The `include` option allows you to add or extend matrix configurations.

---

## 14. `exclude`

Sometimes you don't want every combination.

You can use:

```yaml
exclude:
```

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    version:
      - 1
      - 2

    exclude:
      - os: windows-latest
        version: 1
```

The excluded combination will not run.

The remaining combinations are:

```text
Ubuntu + Version 1
Ubuntu + Version 2
Windows + Version 2
```

---

## 15. `fail-fast`

By default, a matrix can stop other matrix jobs when a job fails.

You can control this using:

```yaml
fail-fast: false
```

Example:

```yaml
strategy:
  fail-fast: false

  matrix:
    os:
      - ubuntu-latest
      - windows-latest
      - macos-latest
```

With:

```yaml
fail-fast: false
```

other matrix jobs can continue even if one configuration fails.

---

## 16. Matrix Job Names

Matrix values can make workflow logs easier to understand.

Example:

```yaml
name: Test ${{ matrix.os }}

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

    runs-on: ${{ matrix.os }}

    steps:
      - run: echo "Testing"
```

Each matrix job can be identified by its configuration.

---

## 17. Matrix Builds and Parallel Execution

Matrix jobs can run independently.

For example:

```text
              ┌── Ubuntu
              │
Matrix ───────┼── Windows
              │
              └── macOS
```

This can save time compared with testing each configuration sequentially.

The actual execution depends on available runner capacity and other limits.

---

## 18. Matrix Build Example for Python

A more realistic example:

```yaml
name: Python Tests

on:
  push:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        python:
          - "3.10"
          - "3.11"
          - "3.12"

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Show Python version
        run: echo "Testing Python ${{ matrix.python }}"
```

This demonstrates testing multiple Python versions.

---

## 19. Matrix Build Example for Different OS

```yaml
name: Cross Platform Test

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest
          - macos-latest

    runs-on: ${{ matrix.os }}

    steps:
      - name: Show runner
        run: echo "Testing on ${{ matrix.os }}"
```

This is useful for checking cross-platform compatibility.

---

## 20. Practice

Create:

```text
.github/workflows/matrix.yml
```

Use:

```yaml
name: Matrix Test

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        os:
          - ubuntu-latest
          - windows-latest

        version:
          - 1
          - 2

    runs-on: ${{ matrix.os }}

    steps:
      - name: Show configuration
        run: |
          echo "OS: ${{ matrix.os }}"
          echo "Version: ${{ matrix.version }}"
```

Run the workflow.

You should get four configurations:

```text
Ubuntu + 1
Ubuntu + 2
Windows + 1
Windows + 2
```

---

## 21. Challenge

Create a matrix that tests:

```text
Ubuntu
Windows
macOS
```

with:

```text
Version 1
Version 2
```

You should get:

```text
Ubuntu + 1
Ubuntu + 2
Windows + 1
Windows + 2
macOS + 1
macOS + 2
```

Total:

```text
3 × 2 = 6 jobs
```

Then use:

```yaml
fail-fast: false
```

and observe the workflow.

---

## 22. Important Concepts

Remember:

```text
Matrix
  ↓
Multiple configurations from one job
```

Basic syntax:

```yaml
strategy:
  matrix:
    value:
      - A
      - B
      - C
```

Access a matrix value:

```yaml
${{ matrix.value }}
```

Use a matrix value for the runner:

```yaml
runs-on: ${{ matrix.os }}
```

Exclude configurations:

```yaml
exclude:
```

Add configurations:

```yaml
include:
```

Allow other matrix jobs to continue after failure:

```yaml
fail-fast: false
```

---

## 23. Interview Questions

### Q1. What is a matrix build?

A matrix build allows the same job to run with multiple configurations.

### Q2. Why are matrix builds useful?

They reduce duplicated workflow code and make it easier to test different operating systems, versions, and configurations.

### Q3. How do you define a matrix?

```yaml
strategy:
  matrix:
```

### Q4. How do you access a matrix value?

```yaml
${{ matrix.value }}
```

### Q5. Can a matrix select different runners?

Yes.

Example:

```yaml
runs-on: ${{ matrix.os }}
```

### Q6. What does `exclude` do?

It removes specific combinations from the matrix.

### Q7. What does `include` do?

It adds or extends specific matrix configurations.

### Q8. What does `fail-fast: false` do?

It prevents a failure in one matrix job from causing the remaining matrix jobs to be cancelled because of fail-fast behavior.

---

# Summary

Matrix builds allow one job to run with multiple configurations.

Example:

```text
One Job
   ↓
Matrix
   ├── Ubuntu
   ├── Windows
   └── macOS
```

You can combine multiple values:

```text
3 operating systems
×
2 versions
=
6 configurations
```

Important syntax:

```yaml
strategy:
  matrix:
```

Access values with:

```yaml
${{ matrix.value }}
```

Use:

```yaml
include:
```

to add configurations.

Use:

```yaml
exclude:
```

to remove configurations.

Use:

```yaml
fail-fast: false
```

when you want other matrix jobs to continue after one fails.

Next topic: GitHub Actions Caching
