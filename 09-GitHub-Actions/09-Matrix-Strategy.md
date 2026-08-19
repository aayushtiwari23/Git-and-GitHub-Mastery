# GitHub Actions Matrix Strategy

## Introduction

A matrix strategy allows the same job to run with multiple combinations of configuration values.

For example, instead of creating separate jobs for:

```text
Python 3.10
Python 3.11
Python 3.12
```

you can define one job with a matrix.
Conceptually:

```text
One Job
   ↓
Matrix
 ├── Python 3.10
 ├── Python 3.11
 └── Python 3.12
```

---

# Basic Matrix

Example:

```yaml
jobs:
  test:
    strategy:
      matrix:
        python-version:
          - '3.10'
          - '3.11'
          - '3.12'

    runs-on: ubuntu-latest

    steps:
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Show Python Version
        run: python --version
```

GitHub creates a job for each matrix value.

---

# Matrix Execution

The example produces:

```text
test / Python 3.10
test / Python 3.11
test / Python 3.12
```

Conceptually:

```text
             Matrix
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
     3.10     3.11     3.12
       ↓        ↓        ↓
     Test     Test     Test
```

Independent matrix jobs can run in parallel.

---

# The `strategy` Keyword

The matrix is defined inside:

```yaml
strategy:
```

Example:

```yaml
strategy:
  matrix:
    version:
      - 1
      - 2
      - 3
```

---

# The `matrix` Context

Matrix values are accessed using:

```text
matrix
```

Example:

```yaml
${{ matrix.version }}
```

If the current job uses:

```text
version = 2
```

then:

```yaml
${{ matrix.version }}
```

evaluates to:

```text
2
```

---

# Multiple Matrix Variables

A matrix can contain multiple variables.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    node-version:
      - '18'
      - '20'
```

This creates combinations of:

```text
Ubuntu + Node 18
Ubuntu + Node 20
Windows + Node 18
Windows + Node 20
```

Total:

```text
2 × 2 = 4 jobs
```

---

# Matrix Visualization

```text
                 Node 18       Node 20
              ┌───────────┬───────────┐
Ubuntu        │    Job    │    Job    │
              ├───────────┼───────────┤
Windows       │    Job    │    Job    │
              └───────────┴───────────┘
```

This is useful for cross-platform testing.

---


# Cross-Platform Testing

A common use case is testing an application on multiple operating systems.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest
      - macos-latest

runs-on: ${{ matrix.os }}
```

This allows the same job to run on:

```text
Ubuntu
Windows
macOS
```

---

# Complete Cross-Platform Example

```yaml
name: Cross Platform Tests

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
      - name: Show Operating System
        run: echo "Running on ${{ matrix.os }}"
```

The workflow creates three job variations.

---

# Matrix with Python Versions

```yaml
name: Python Matrix

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        python-version:
          - '3.10'
          - '3.11'
          - '3.12'

    runs-on: ubuntu-latest

    steps:
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Test Version
        run: python --version
```

---

# Matrix with Node.js Versions

```yaml
name: Node Matrix

on:
  workflow_dispatch:

jobs:
  test:
    strategy:
      matrix:
        node-version:
          - '18'
          - '20'
          - '22'

    runs-on: ubuntu-latest

    steps:
      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Show Version
        run: node --version
```

---

# Matrix with Java Versions

```yaml
strategy:
  matrix:
    java-version:
      - '17'
      - '21'
```

Then:

```yaml
- name: Set up Java
  uses: actions/setup-java@v4
  with:
    distribution: 'temurin'
    java-version: ${{ matrix.java-version }}
```

---

# Matrix Combination Count

Suppose you have:

```yaml
os:
  - ubuntu
  - windows

python:
  - '3.10'
  - '3.11'
  - '3.12'
```

The total combinations are:

```text
2 × 3 = 6
```

So GitHub can create six job variations.

---

# `include`

The `include` keyword allows you to add or modify specific matrix combinations.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    version:
      - '1'
      - '2'

    include:
      - os: ubuntu-latest
        version: '3'
```

This adds an additional combination.

---

# Why Use `include`?

Sometimes you need a special configuration that doesn't fit the normal matrix.

Example:

```text
Ubuntu + Version 1
Ubuntu + Version 2
Windows + Version 1
Windows + Version 2
Ubuntu + Version 3
```

`include` allows you to add that special case.

---

# `exclude`

The `exclude` keyword removes specific matrix combinations.

Example:

```yaml
strategy:
  matrix:
    os:
      - ubuntu-latest
      - windows-latest

    version:
      - '1'
      - '2'

    exclude:
      - os: windows-latest
        version: '1'
```

The excluded combination:

```text
Windows + Version 1
```

will not run.

---

# Include vs Exclude

| `include` | `exclude` |
|---|---|
| Adds or modifies combinations | Removes combinations |
| Useful for special cases | Useful for unsupported combinations |
| Expands or customizes the matrix | Reduces the matrix |

---

# Fail-Fast

Matrix jobs can use:

```yaml
strategy:
  fail-fast: true
```

The default behavior is generally to cancel in-progress and queued
