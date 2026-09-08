-latest

    steps:
      - name: Create cache directory
        run: |
          mkdir -p cache
          echo "Cached data" > cache/data.txt

      - name: Cache files
        uses: actions/cache@v4
        with:
          path: cache/
          key: demo-cache-v1
```

The `cache` directory is stored using the key:

```text
demo-cache-v1
```

---

## 7. Restoring a Cache

If the same cache key is used in a later workflow run:

```yaml
key: demo-cache-v1
```

GitHub can restore the existing cache.

Example:

```yaml
- name: Restore cache
  uses: actions/cache@v4
  with:
    path: cache/
    key: demo-cache-v1
```

If the cache exists, it can be restored.

---

## 8. Cache Hit and Miss

There are two common situations.

### Cache Hit

```text
Requested key
     ↓
Cache found
     ↓
Restore cache
```

### Cache Miss

```text
Requested key
     ↓
Cache not found
     ↓
Run normally
     ↓
Create new cache
```

---

## 9. Using a Dependency Hash

A better cache key often includes a hash of the dependency file.

Example:

```yaml
key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
```

This means the cache key depends on:

```text
Operating system
+
package-lock.json
```

If the dependency file changes, the key changes and a new cache can be created.

---

## 10. Node.js Example

Node.js projects commonly use:

```text
package-lock.json
```

Example:

```yaml
name: Node Cache

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Cache npm
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}

      - name: Install dependencies
        run: npm ci
```

The cache stores npm's package cache.

---

## 11. Python Example

Python projects can also cache package downloads.

Example:

```yaml
- name: Cache pip
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('requirements.txt') }}

- name: Install dependencies
  run: pip install -r requirements.txt
```

The key depends on:

```text
Operating System
+
requirements.txt
```

If `requirements.txt` changes, the cache key changes.

---

## 12. Cache Paths

The `path` specifies what should be stored.

Example:

```yaml
path: ~/.npm
```

Another example:

```yaml
path: ~/.cache/pip
```

You can also cache multiple paths:

```yaml
path: |
  ~/.npm
  ~/.cache/myapp
```

---

## 13. Cache Restore Keys

You can also provide:

```yaml
restore-keys:
```

Example:

```yaml
- name: Cache
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

If the exact key isn't available, GitHub can try to find a suitable cache using the restore key.

---

## 14. Why Use Hashes?

Suppose your project initially has:

```text
package-lock.json
```

with version A.

The cache key might be:

```text
ubuntu-node-ABC123
```

Later, dependencies change.

The file hash changes:

```text
ubuntu-node-XYZ789
```

A new cache can then be created.

This helps prevent outdated dependency caches from being reused incorrectly.

---

## 15. Cache Security

Be careful about what you store in caches.

Do not intentionally store:

- Passwords
- API keys
- Access tokens
- Private credentials

Caches are designed for reusable data, not secret storage.

Use GitHub Secrets for sensitive information.

---

## 16. Cache Scope

Caches are not simply global storage available to everyone.

Their availability is controlled by GitHub Actions cache rules and workflow context.

You should design cache keys carefully so that unrelated projects or configurations don't accidentally reuse inappropriate data.

---

## 17. Using Setup Actions With Built-In Caching

Some setup actions support caching directly.

For example, `actions/setup-node` can enable npm caching.

Example:

```yaml
- name: Setup Node.js
  uses: actions/setup-node@v4
  with:
    node-version: 20
    cache: npm
```

This is often simpler than manually configuring `actions/cache`.

---

## 18. Complete Node.js Example

```yaml
name: Node.js CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

The important part is:

```yaml
cache: npm
```

This enables npm dependency caching through the setup action.

---

## 19. When Should You Use Caching?

Caching is useful when:

```text
Same dependencies
      +
Repeated workflow runs
      ↓
Caching can save time
```

It is especially useful for projects with:

- Large dependencies
- Slow downloads
- Frequent CI runs
- Large build caches

---

## 20. When Should You Not Use Caching?

Don't add caching just because you can.

Caching may not be useful when:

- The files are very small
- The dependency installation is already very fast
- The data changes constantly
- The cache is difficult to maintain
- The cache provides little performance improvement

Measure the workflow before and after when performance matters.

---

## 21. Practice

Create:

```text
.github/workflows/cache.yml
```

Use:

```yaml
name: Cache Demo

on:
  workflow_dispatch:

jobs:
  cache:
    runs-on: ubuntu-latest

    steps:
      - name: Create files
        run: |
          mkdir -p cache
          echo "Hello from cache" > cache/data.txt

      - name: Cache files
        uses: actions/cache@v4
        with:
          path: cache/
          key: demo-cache-v1
```

Run the workflow.

Then run it again and observe the workflow logs.

---

## 22. Challenge

Create a Node.js workflow that:

1. Checks out the repository.
2. Sets up Node.js.
3. Enables npm caching.
4. Installs dependencies.
5. Runs tests.

Use:

```yaml
cache: npm
```

inside the Node.js setup step.

Try running the workflow multiple times and compare the dependency installation behavior.

---

## 23. Important Concepts

Remember:

```text
Cache
  ↓
Reuse data between workflow runs
```

Basic action:

```yaml
uses: actions/cache@v4
```

Important options:

```yaml
path:
key:
restore-keys:
```

A cache key can include:

```yaml
${{ runner.os }}
```

and:

```yaml
${{ hashFiles('package-lock.json') }}
```

For example:

```yaml
key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
```

---

## 24. Interview Questions

### Q1. What is caching in GitHub Actions?

Caching stores reusable files so future workflow runs can avoid downloading or generating them again.

### Q2. What is the main purpose of caching?

To improve workflow performance by reusing previously stored data.

### Q3. What is the difference between a cache and an artifact?

A cache is mainly used to speed up future workflow runs. An artifact is used to save workflow-generated files.

### Q4. Which action can be used for caching?

```yaml
actions/cache@v4
```

### Q5. What is a cache key?

A cache key identifies a particular cache.

### Q6. Why use `hashFiles()` in a cache key?

It allows the cache key to change when important dependency files change.

### Q7. What does `restore-keys` do?

It provides fallback key prefixes that GitHub can use to find a suitable existing cache when the exact key isn't available.

### Q8. Should secrets be stored in caches?

No. Secrets should be stored using GitHub Secrets.

---

# Summary

Caching helps make GitHub Actions workflows faster.

Basic example:

```yaml
- name: Cache
  uses: actions/cache@v4
  with:
    path: ~/.cache
    key: my-cache
```

For dependency caching, setup actions can sometimes provide simpler options.

Example:

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 20
    cache: npm
```

Remember:

```text
Cache
→ Speed up future runs

Artifact
→ Save workflow output

Secret
→ Store sensitive information
```

Next topic: GitHub Actions CI/CD Pipelines
