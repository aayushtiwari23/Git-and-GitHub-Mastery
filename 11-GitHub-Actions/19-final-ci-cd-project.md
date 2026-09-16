
# GitHub Actions Final CI/CD Project

## 1. Project Overview

In this project, we will combine the GitHub Actions concepts learned throughout this series into one complete CI/CD pipeline.

The pipeline will demonstrate:

- Workflow triggers
- Jobs
- Steps
- Actions
- Environment variables
- Secrets
- Expressions
- Conditions
- Job dependencies
- Artifacts
- Caching
- Matrix testing
- Environments
- Concurrency
- Failure handling
- Security best practices

---

## 2. Project Goal

Build a CI/CD workflow with this structure:

```text
Pull Request / Push
        ↓
     Lint
        ↓
     Test
        ↓
     Build
        ↓
   Upload Artifact
        ↓
     Deploy
```

For `main`:

```text
main
 ↓
Lint
 ↓
Test
 ↓
Build
 ↓
Artifact
 ↓
Production
```

---

## 3. Recommended Repository Structure

Create this structure:

```text
project/
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── src/
│   └── app.js
│
├── package.json
│
└── README.md
```

---

## 4. Sample Application

Create:

```text
src/app.js
```

Add:

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

module.exports = { greet };
```

---

## 5. Package Configuration

Create:

```text
package.json
```

Example:

```json
{
  "name": "github-actions-final-project",
  "version": "1.0.0",
  "description": "GitHub Actions CI/CD final project",
  "main": "src/app.js",
  "scripts": {
    "test": "node test.js",
    "lint": "node --check src/app.js",
    "build": "mkdir -p dist && cp src/app.js dist/app.js"
  }
}
```

---

## 6. Test File

Create:

```text
test.js
```

Add:

```javascript
const { greet } = require("./src/app");

const result = greet("GitHub");

if (result !== "Hello, GitHub!") {
  throw new Error("Test failed");
}

console.log("All tests passed");
```

---

## 7. Workflow File

Create:

```text
.github/workflows/ci-cd.yml
```

The workflow will contain the complete pipeline.

---

## 8. Workflow Trigger

Use:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:
```

This means the workflow can run when:

```text
Push to main
Pull request targeting main
Manual execution
```

---

## 9. Permissions

Use least privilege:

```yaml
permissions:
  contents: read
```

The workflow only receives read access to repository contents unless additional permissions are explicitly required.

---

## 10. Concurrency

Add:

```yaml
concurrency:
  group: ci-cd-${{ github.ref }}
  cancel-in-progress: true
```

This helps prevent unnecessary duplicate runs for the same branch or reference.

---

## 11. Lint Job

Create the first job:

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Run lint
        run: npm run lint
```

The job performs:

```text
Checkout
   ↓
Setup Node
   ↓
Install
   ↓
Lint
```

---

## 12. Test Job

Add:

```yaml
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Lint and Test can run independently.

---

## 13. Matrix Testing

You can test multiple Node.js versions.

Example:

```yaml
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node:
          - 22
          - 24

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

The workflow now tests:

```text
Node 22
Node 24
```

---

## 14. Build Job

The build should wait for lint and tests.

```yaml
  build:
    needs:
      - lint
      - test

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build
```

Dependency graph:

```text
Lint ──┐
       ├──→ Build
Test ──┘
```

---

## 15. Upload Build Artifact

After building, upload the generated files.

Add:

```yaml
      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: dist/
```

The build job now produces an artifact.

---

## 16. Download Artifact

A deployment job can download the artifact.

Example:

```yaml
  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: application-build
          path: dist/

      - name: Show files
        run: ls -la dist
```

---

## 17. Production Condition

Production deployment should happen only from `main`.

Example:

```yaml
if: github.ref == 'refs/heads/main'
```

This prevents pull requests from automatically deploying to production.

---

## 18. Production Environment

Use:

```yaml
environment: production
```

Example:

```yaml
  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    environment: production
    runs-on: ubuntu-latest
```

The `production` environment can be configured in GitHub with appropriate protection rules.

---

## 19. Deployment Secret

If the deployment requires a credential, store it as a GitHub secret.

Example:

```text
DEPLOY_TOKEN
```

Use it only in the deployment step:

```yaml
      - name: Deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: ./deploy.sh
```

Never print the token.

---

## 20. Complete Workflow

A simplified final workflow can look like:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

  pull_request:
    branches:
      - main

  workflow_dispatch:

permissions:
  contents: read

concurrency:
  group: ci-cd-${{ github.ref }}
  cancel-in-progress: true

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Run lint
        run: npm run lint

  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node:
          - 22
          - 24

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

  build:
    needs:
      - lint
      - test

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 24
          cache: npm

      - name: Install dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Upload build
        uses: actions/upload-artifact@v4
        with:
          name: application-build
          path: dist/

  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    environment: production
    runs-on: ubuntu-latest

    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: application-build
          path: dist/

      - name: Show build
        run: ls -la dist

      - name: Deploy
        run: echo "Production deployment completed"
```

---

## 21. Final Pipeline

The complete flow is:

```text
                  ┌── Lint ──┐
                  │          │
Push / PR ────────┤          ├──→ Build ──→ Artifact ──→ Deploy
                  │          │
                  └── Test ──┘
```

Test runs on:

```text
Node 22
Node 24
```

Production deployment runs only when:

```text
Build succeeds
AND
Branch = main
```

---

## 22. What Happens on a Pull Request?

When a pull request targets `main`:

```text
Pull Request
     ↓
Lint
     ↓
Test
     ↓
Build
```

Production deployment does not run because:

```yaml
github.ref == 'refs/heads/main'
```

is false for the pull request workflow context.

---

## 23. What Happens After Push to Main?

When changes are pushed to `main`:

```text
Push
 ↓
Lint ──┐
       ├──→ Build ──→ Artifact ──→ Production
Test ──┘
```

All required checks must succeed before deployment.

---

## 24. What If Lint Fails?

Example:

```text
Lint ❌
Test  ✅
```

Because Build requires:

```yaml
needs:
  - lint
  - test
```

the build will not proceed normally.

The pipeline stops before deployment.

---

## 25. What If Tests Fail?

Example:

```text
Lint ✅
Test ❌
```

Build waits for both jobs.

Therefore:

```text
Test failure
     ↓
Build blocked
     ↓
Deploy blocked
```

This protects production from failed code.

---

## 26. What If Build Fails?

Example:

```text
Lint ✅
Test ✅
Build ❌
```

The artifact is not successfully produced.

Therefore:

```text
Build failure
     ↓
No successful artifact
     ↓
Deploy blocked
```

---

## 27. What Is Being Demonstrated?

This project combines:

```text
Workflow Triggers
       ↓
Permissions
       ↓
Concurrency
       ↓
Jobs
       ↓
Actions
       ↓
Caching
       ↓
Matrix Testing
       ↓
Job Dependencies
       ↓
Build
       ↓
Artifacts
       ↓
Conditions
       ↓
Environment
       ↓
Deployment
```

---

## 28. Security Improvements

For a real production project, consider:

```text
Least-privilege permissions
Environment protection
Environment-specific secrets
Pinned third-party actions
OIDC for supported cloud deployments
Dependency scanning
Secret scanning
Code scanning
Protected branches
Required status checks
```

Do not copy production credentials into the repository.

---

## 29. Testing the Project

After creating the files:

```bash
git status
```

Then:

```bash
git add .
```

Commit:

```bash
git commit -m "Build complete GitHub Actions CI/CD pipeline"
```

Push:

```bash
git push
```

Then open:

```text
GitHub
  ↓
Actions
  ↓
CI/CD Pipeline
```

Watch each job execute.

---

## 30. What to Observe

During the workflow, check:

```text
Lint job
Test matrix
Build job
Artifact
Deploy job
Job dependencies
Logs
Workflow status
```

You should understand why each job runs and why later jobs wait for earlier jobs.

---

## 31. Final Challenge

Modify the project to add:

```text
Security Scan
```

The new structure should be:

```text
             ┌── Lint ─────┐
             │             │
             ├── Test ─────┤
             │             │
             └── Security ─┘
                    ↓
                  Build
                    ↓
                 Artifact
                    ↓
               Production
```

Build should depend on all three checks.

---

## 32. Advanced Challenge

Add a staging environment.

Final structure:

```text
Lint ──┐
Test ──┼──→ Build ──→ Staging ──→ Production
Scan ──┘
```

Requirements:

- Staging runs after Build.
- Production runs after Staging.
- Production only runs from `main`.
- Production uses the `production` environment.
- Use appropriate secrets.
- Add concurrency.
- Keep permissions minimal.

---

## 33. Portfolio Improvement

After completing the project, add it to your GitHub profile.

Your repository should contain:

```text
README.md
.github/workflows/ci-cd.yml
src/
package.json
test.js
```

The README should explain:

```text
Project
Technologies
Workflow
CI pipeline
CD pipeline
Security
How to run
```

A working CI/CD project is much more valuable than simply having workflow files that are never executed.

---

## 34. Interview Questions

### Q1. What is CI?

Continuous Integration is the practice of automatically building and testing code changes.

### Q2. What is CD?

Continuous Delivery or Continuous Deployment automates the process of preparing or deploying software after successful CI checks.

### Q3. Why separate lint, test, and build jobs?

It keeps responsibilities clear and allows independent checks to run in parallel.

### Q4. Why use `needs`?

To create dependencies between jobs.

### Q5. Why use matrix testing?

To test the application against multiple configurations such as different runtime versions or operating systems.

### Q6. Why use artifacts?

To preserve and transfer generated files between jobs.

### Q7. Why use caching?

To speed up repeated dependency installation or other reusable operations.

### Q8. Why should production deployment depend on tests?

To prevent code that fails required checks from being deployed.

### Q9. Why use environments?

To separate deployment targets and provide environment-specific controls, secrets, and protection rules.

### Q10. Why use concurrency?

To prevent unnecessary or conflicting workflow runs.

---

## 35. Final GitHub Actions Checklist

You should now understand:

```text
[ ] What GitHub Actions is
[ ] Workflow files
[ ] Events
[ ] Jobs
[ ] Steps
[ ] Runners
[ ] Actions
[ ] Marketplace actions
[ ] Environment variables
[ ] Secrets
[ ] Contexts
[ ] Expressions
[ ] Conditions
[ ] Job dependencies
[ ] Matrix builds
[ ] Artifacts
[ ] Caching
[ ] Custom actions
[ ] Reusable workflows
[ ] Manual workflows
[ ] Environments
[ ] Concurrency
[ ] Debugging
[ ] Security
[ ] CI/CD architecture
```

---

# Final Summary

You have now combined the major GitHub Actions concepts into a complete CI/CD project.

The overall architecture is:

```text
Developer
    ↓
GitHub Push / Pull Request
    ↓
GitHub Actions
    ↓
┌──────────┬──────────┬──────────────┐
│   Lint   │   Test   │   Security   │
└──────────┴──────────┴──────────────┘
              ↓
            Build
              ↓
           Artifact
              ↓
           Staging
              ↓
         Production
```

The most important concept is:

```text
Code Change
    ↓
Automated Checks
    ↓
Build
    ↓
Verified Artifact
    ↓
Controlled Deployment
```

You are no longer just learning individual GitHub Actions commands.

You are learning how to design an actual CI/CD system.

---

# GitHub Actions Series Complete

Your GitHub Actions learning path is now complete.

Next major topic in the broader roadmap:

```text
Linux
```

Commit message:

```text
Build complete GitHub Actions CI/CD pipeline
```
