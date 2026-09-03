
```text
Application
     ↓
GitHub Actions
     ↓
Build
     ↓
Build files
     ↓
Upload as Artifact
```

---

## 2. Why Are Artifacts Useful?

GitHub Actions runners are temporary environments.

If a job creates a file, you may want to save it after the workflow finishes.

Artifacts allow you to store and download those files.

For example:

```text
build/
├── app.exe
├── config.json
└── README.txt
```

The entire `build` folder can be uploaded as an artifact.

---

## 3. Uploading an Artifact

GitHub provides the official action:

```yaml
actions/upload-artifact
```

Example:

```yaml
- name: Upload build
  uses: actions/upload-artifact@v4
  with:
    name: build-files
    path: build/
```

Here:

```yaml
name: build-files
```

is the artifact name.

And:

```yaml
path: build/
```

specifies what should be uploaded.

---

## 4. Complete Example

```yaml
name: Artifact Demo

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create file
        run: |
          mkdir build
          echo "Hello GitHub Actions" > build/app.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: build/
```

The workflow creates:

```text
build/app.txt
```

and uploads it as:

```text
build-files
```

---

## 5. Downloading an Artifact

GitHub provides:

```yaml
actions/download-artifact
```

Example:

```yaml
- name: Download artifact
  uses: actions/download-artifact@v4
  with:
    name: build-files
    path: downloaded/
```

The artifact will be downloaded into:

```text
downloaded/
```

---

## 6. Artifacts Between Jobs

Suppose we have two jobs:

```text
Build
  ↓
Test
```

The build job creates a file.

The test job needs that file.

You can use an artifact:

```text
Build Job
   ↓
Upload Artifact
   ↓
Test Job
   ↓
Download Artifact
```

Example:

```yaml
jobs:

  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create build
        run: |
          mkdir build
          echo "Application build" > build/app.txt

      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: build/

  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: build-files
          path: build/

      - name: Check build
        run: cat build/app.txt
```

The important part is:

```yaml
needs: build
```

This makes the test job wait for the build job.

---

## 7. Uploading Multiple Files

You can upload multiple files or folders.

Example:

```yaml
- name: Upload files
  uses: actions/upload-artifact@v4
  with:
    name: project-files
    path: |
      build/
      reports/
      logs/
```

---

## 8. Uploading a Specific File

You don't always need to upload an entire folder.

Example:

```yaml
- name: Upload report
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: report.html
```

Only:

```text
report.html
```

will be uploaded.

---

## 9. Artifact Names

Artifact names should clearly describe their contents.

Good examples:

```text
build-files
test-results
coverage-report
application-build
logs
```

Avoid confusing names such as:

```text
abc
file1
stuff
test123
```

Clear names make workflows easier to understand.

---

## 10. Downloading All Artifacts

You can download artifacts without specifying a particular artifact name.

Example:

```yaml
- name: Download all artifacts
  uses: actions/download-artifact@v4
  with:
    path: artifacts/
```

This downloads the available artifacts into:

```text
artifacts/
```

---

## 11. Artifacts vs Repository Files

Don't confuse artifacts with normal repository files.

Repository files:

```text
Git repository
    ↓
Permanent source code
```

Artifacts:

```text
Workflow
    ↓
Generated files
    ↓
Saved workflow output
```

For example:

```text
app.py
```

belongs in your repository.

But:

```text
build/app.exe
```

might be generated during the workflow and stored as an artifact.

---

## 12. Artifacts vs Git Commits

Artifacts do not automatically become commits.

For example:

```text
Workflow
   ↓
Generate report
   ↓
Upload artifact
```

does not modify your Git repository.

If you want to permanently add something to the repository, you need to commit and push it.

---

## 13. Artifacts vs Cache

Artifacts and caches have different purposes.

### Artifacts

Used to save workflow outputs.

Examples:

```text
Build files
Test reports
Logs
Screenshots
```

### Cache

Used to speed up future workflow runs.

Examples:

```text
Dependencies
Package manager files
Build caches
```

Simple rule:

```text
Need to save output?
→ Artifact

Need to speed up future runs?
→ Cache
```

Caching will be covered later.

---

## 14. Artifact Retention

Artifacts are not necessarily stored forever.

GitHub allows artifact retention settings so that artifacts can be automatically removed after a certain period.

You can also specify retention in a workflow.

Example:

```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: build-files
    path: build/
    retention-days: 5
```

This requests that the artifact be retained for 5 days.

---

## 15. Test Reports

Artifacts are very useful for testing.

For example:

```text
Run tests
    ↓
Generate report.html
    ↓
Upload report
    ↓
Download report later
```

Example:

```yaml
- name: Run tests
  run: python -m unittest

- name: Upload test report
  uses: actions/upload-artifact@v4
  with:
    name: test-report
    path: report.html
```

---

## 16. Build Output

A common CI workflow looks like:

```text
Checkout
   ↓
Install dependencies
   ↓
Build
   ↓
Test
   ↓
Upload build
```

The generated build can then be downloaded from the workflow run.

---

## 17. Practice

Create:

```text
.github/workflows/artifact.yml
```

Use:

```yaml
name: Artifact Demo

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Create files
        run: |
          mkdir build
          echo "Application build" > build/app.txt
          echo "Version 1.0" > build/version.txt

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: build-files
          path: build/
```

Run the workflow.

Then open the completed workflow run.

You should see an artifact named:

```text
build-files
```

Download it and check its contents.

---

## 18. Challenge

Create two jobs:

```text
build
test
```

The `build` job should:

1. Create a folder named `build`
2. Create a file named `app.txt`
3. Upload the folder as an artifact

The `test` job should:

1. Depend on the `build` job
2. Download the artifact
3. Display the contents of `app.txt`

Expected flow:

```text
build
 ↓
upload artifact
 ↓
test
 ↓
download artifact
 ↓
read app.txt
```

---

## 19. Interview Questions

### Q1. What is a GitHub Actions artifact?

An artifact is a file or collection of files produced by a workflow that is uploaded and stored with the workflow run.

### Q2. Which action uploads artifacts?

```yaml
actions/upload-artifact@v4
```

### Q3. Which action downloads artifacts?

```yaml
actions/download-artifact@v4
```

### Q4. Why are artifacts useful between jobs?

Because separate jobs use separate runner environments. Artifacts provide a way to transfer generated files between jobs.

### Q5. What is the difference between artifacts and cache?

Artifacts store workflow outputs, while caches are mainly used to speed up future workflow runs.

### Q6. Do artifacts automatically modify the Git repository?

No.

### Q7. Can artifacts be automatically deleted?

Yes. Artifact retention settings can remove them after the configured retention period.

---

# Summary

Artifacts allow GitHub Actions to save and transfer generated files.

Upload:

```yaml
uses: actions/upload-artifact@v4
```

Download:

```yaml
uses: actions/download-artifact@v4
```

Typical workflow:

```text
Build
 ↓
Create files
 ↓
Upload artifact
 ↓
Another job
 ↓
Download artifact
 ↓
Use files
```

Remember:

```text
Artifact → Save workflow output

Cache → Speed up future workflows

Repository → Store source code
```

Next topic: GitHub Actions Environments
