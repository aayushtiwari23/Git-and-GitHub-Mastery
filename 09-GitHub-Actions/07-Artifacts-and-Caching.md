# GitHub Actions Artifacts and Caching

## Introduction

GitHub Actions workflows often create files during execution.

Examples:

- Build packages
- Test reports
- Log files
- Compiled applications
- Screenshots
- Coverage reports

GitHub Actions provides **artifacts** for storing workflow-generated files.

It also provides **caching** to speed up repeated workflow runs.

---

# What is an Artifact?

An artifact is a file or collection of files produced during a workflow run that you want to preserve or share.

Conceptually:

```text
Source Code
    ↓
Build
    ↓
Generated Files
    ↓
Artifact
    ↓
Download / Share
```

---

# Why Use Artifacts?

Suppose a workflow builds an application:

```text
Source Code
    ↓
Build
    ↓
app.zip
```

Without an artifact, the generated file may disappear when the runner environment is removed.

With an artifact:

```text
Build
   ↓
app.zip
   ↓
Upload Artifact
   ↓
GitHub
```

You can then access the artifact from the workflow run.

---

# Uploading an Artifact

GitHub provides an official Action for uploading artifacts.

Example:

```yaml
- name: Upload Build
  uses: actions/upload-artifact@v4
  with:
    name: application-build
    path: ./build
```

Here:

```text
name
```

is the artifact name.

And:

```text
path
```

specifies the files or directory to upload.

---

# Complete Artifact Example

```yaml
name: Artifact Example

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create Build Directory
        run: |
          mkdir build
          echo "Sample application build" > build/app.txt

      - name: Upload Build Artifact
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: build
```

Workflow:

```text
Create Files
     ↓
Build Directory
     ↓
Upload Artifact
     ↓
GitHub Stores Artifact
```

---

# Downloading an Artifact

You can download an artifact using:

```yaml
- name: Download Artifact
  uses: actions/download-artifact@v5
  with:
    name: application-build
```

This is useful when another job needs files produced by an earlier job.

---

# Artifacts Between Jobs

Remember that jobs normally use separate runner environments.

Example:

```text
Build Job
    ↓
Runner A
    ↓
build/
```

Another job:

```text
Test Job
    ↓
Runner B
```

The second job does not automatically have the files created by the first job.

Artifacts can transfer those files.

---

# Artifact Workflow

```text
Build Job
    ↓
Create Files
    ↓
Upload Artifact
    ↓
GitHub
    ↓
Download Artifact
    ↓
Another Job
```

---

# Example: Two Jobs

```yaml
name: Build and Test

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create Build
        run: |
          mkdir build
          echo "Build output" > build/app.txt

      - name: Upload Build
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: build

  test:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download Build
        uses: actions/download-artifact@v5
        with:
          name: application-build

      - name: Show Build
        run: ls -R
```

The flow is:

```text
Build Job
    ↓
Upload Artifact
    ↓
Test Job
    ↓
Download Artifact
```

---

# Artifact Naming

Use meaningful artifact names.

Good:

```text
application-build
test-results
coverage-report
release-package
```

Avoid:

```text
file1
abc
test
thing
```

Meaningful names make workflow runs easier to understand.

---

# Multiple Files

You can upload a directory containing multiple files.

Example:

```yaml
- name: Upload Results
  uses: actions/upload-artifact@v4
  with:
    name: test-results
    path: results/
```

If the directory contains:

```text
results/
├── report.html
├── coverage.xml
└── test.log
```

all of these files can be included in the artifact.

---

# Multiple Paths

You can also configure multiple paths where supported by the Action's path handling.

For example:

```yaml
- name: Upload Reports
  uses: actions/upload-artifact@v4
  with:
    name: reports
    path: |
      report.html
      coverage.xml
```

---

# Artifact Retention

Artifacts are not necessarily stored forever.

You can configure retention where appropriate.

Example:

```yaml
- name: Upload Artifact
  uses: actions/upload-artifact@v4
  with:
    name: build
    path: build/
    retention-days: 7
```

This specifies a retention period for that artifact.

Repository and organization settings can also affect retention limits.

---

# Artifacts vs Source Code

Artifacts are not replacements for Git.

Use Git for:

```text
Source Code
Version History
Branches
Commits
Pull Requests
```

Use artifacts for:

```text
Build Output
Reports
Generated Files
Test Results
Packages
```

---

# What is Caching?

Caching stores reusable files so future workflow runs can execute faster.

The idea is:

```text
First Run
   ↓
Download Dependencies
   ↓
Store in Cache

Next Run
   ↓
Restore Cache
   ↓
Skip Repeated Downloads
```

---

# Why Use Caching?

Installing dependencies can take time.

For example:

```text
Node.js
Python
Java
.NET
```

A project may repeatedly download the same dependencies.

Caching can reduce this repeated work.

---

# Cache vs Artifact

These concepts are different.

| Artifact | Cache |
|----------|-------|
| Preserves workflow output | Speeds up future workflows |
| Used to share generated files | Used to reuse dependencies/data |
| Usually associated with a workflow run | Reused across workflow runs when cache matches |
| Example: build package | Example: dependency cache |

---

# Caching Dependencies

A typical pattern is:

```text
Restore Cache
     ↓
Install Missing Dependencies
     ↓
Run Tests
```

If the cache is available:

```text
Restore Cache
     ↓
Use Cached Dependencies
     ↓
Run Tests
```

---

# Cache Action

GitHub provides:

```yaml
actions/cache
```

Example:

```yaml
- name: Cache Dependencies
  uses: actions/cache@v4
  with:
    path: ~/.cache
    key: ${{ runner.os }}-dependencies
```

The exact cache path depends on the package manager and project.

---

# Cache Keys

A cache key identifies a cache.

Example:

```yaml
key: ${{ runner.os }}-dependencies
```

This can produce different caches for different operating systems.

For dependency caching, a stronger key often includes a hash of the dependency lock file.

---

# Hashing Dependency Files

Example:

```yaml
key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
```

If `package-lock.json` changes, the hash changes.

Conceptually:

```text
package-lock.json
       ↓
     Hash
       ↓
New Cache Key
       ↓
New Dependency Cache
```

This helps avoid using an outdated dependency cache.

---

# Cache Restore

A cache can be restored when the key matches.

Conceptually:

```text
Workflow Run
     ↓
Calculate Cache Key
     ↓
Cache Exists?
   /        \
 Yes        No
 ↓           ↓
Restore    Install
Cache      Dependencies
```

---

# Package Manager Caching

Some setup Actions can provide built-in caching support.

For example, a Node.js setup workflow can use:

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'
```

This can simplify dependency caching for supported package managers.

---

# Python Caching

Python workflows can also use caching through supported setup and dependency approaches.

Example:

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.12'
    cache: 'pip'
```

This can help speed up repeated dependency installation.

---

# Cache Invalidation

A cache should change when the underlying dependencies change.

For example:

```text
Old package-lock.json
       ↓
Old Cache

Updated package-lock.json
       ↓
New Hash
       ↓
New Cache
```

This is why lock-file hashes are useful in cache keys.

---

# Complete Caching Example

```yaml
name: Dependency Cache

on:
  workflow_dispatch:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install Dependencies
        run: npm ci

      - name: Run Tests
        run: npm test
```

The setup Action handles the supported npm caching configuration.

---

# Artifacts and Caches Together

A workflow can use both.

Example:

```text
Cache
 ↓
Faster Dependency Installation
 ↓
Build
 ↓
Generated Output
 ↓
Artifact
```

This is a common CI pattern.

---

# Example CI Flow

```text
Checkout
    ↓
Restore Dependencies Cache
    ↓
Install Dependencies
    ↓
Run Tests
    ↓
Build Application
    ↓
Upload Build Artifact
```

---

# Security Considerations

Be careful about what you store in artifacts and caches.

Do not intentionally store:

```text
Passwords
API Keys
Private Credentials
Private Tokens
Sensitive Personal Data
```

Artifacts and caches should not be treated as a secure place for secrets.

---

# Cache Security

Caches can persist data between workflow runs.

Be careful when workflows process untrusted code.

Do not assume cached data is automatically safe just because it came from an earlier workflow run.

---

# Artifact Security

Artifacts may contain source code or build output.

Consider:

```text
Who can access the workflow?
Who can download the artifact?
Does the artifact contain sensitive information?
How long should it be retained?
```

Use appropriate repository and environment permissions.

---

# Common Mistakes

Avoid:

```text
Caching unnecessary files
Using outdated cache keys
Storing secrets in artifacts
Storing sensitive information in caches
Using artifacts as permanent source storage
Ignoring artifact retention
```

---

# Best Practices

### For Artifacts

- Use meaningful artifact names.
- Upload only necessary files.
- Set appropriate retention.
- Avoid sensitive data.
- Use artifacts to transfer build outputs between jobs.

### For Caching

- Cache dependencies that are expensive to download.
- Use reliable cache keys.
- Include dependency lock files when appropriate.
- Avoid caching unnecessary files.
- Review cache behavior when dependencies change.

---

# Interview Questions

### What is an artifact?

An artifact is a file or collection of files produced by a workflow that can be stored and accessed after the workflow completes.

### Why are artifacts useful?

They preserve build outputs, reports, test results, and other generated files.

### What is caching?

Caching stores reusable files so future workflow runs can execute faster.

### What is the difference between artifacts and caches?

Artifacts preserve workflow outputs, while caches are primarily used to speed up future workflow runs by reusing data such as dependencies.

### Why use a dependency lock file in a cache key?

Changes to the lock file can indicate dependency changes, so the cache key changes and a more appropriate cache can be created.

---

# Practice

Create:

```text
.github/workflows/artifacts-practice.yml
```

Paste:

```yaml
name: Artifacts Practice

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create Build Directory
        run: |
          mkdir build
          echo "This is my build output" > build/app.txt

      - name: Upload Build Artifact
        uses: actions/upload-artifact@v4
        with:
          name: my-build
          path: build/

  download:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download Build Artifact
        uses: actions/download-artifact@v5
        with:
          name: my-build

      - name: Show Downloaded Files
        run: ls -R
```

Commit message:

```text
Add GitHub Actions artifacts and caching guide
```

Then:

```text
Repository
   ↓
Actions
   ↓
Artifacts Practice
   ↓
Run workflow
```

Open the workflow run.

Check:

```text
Build Job
   ↓
Artifact Upload
   ↓
Download Job
   ↓
Downloaded Files
```

---

# Summary

Artifacts and caches solve different problems.

```text
Artifacts
    ↓
Preserve / Share Workflow Output

Caches
    ↓
Speed Up Future Workflow Runs
```

A common CI pipeline looks like:

```text
Checkout
    ↓
Restore Cache
    ↓
Install Dependencies
    ↓
Test
    ↓
Build
    ↓
Upload Artifact
```

Understanding artifacts and caching is an important step toward building efficient and professional GitHub Actions workflows.
