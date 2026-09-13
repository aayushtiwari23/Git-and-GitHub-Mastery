# GitHub Actions Security and Best Practices

## 1. Why Security Matters

GitHub Actions workflows can:

- Read repository files
- Execute commands
- Access secrets
- Create releases
- Deploy applications
- Access cloud services
- Modify repository resources

A poorly configured workflow can therefore create serious security risks.

---

## 2. Principle of Least Privilege

Give a workflow only the permissions it actually needs.

Example:

```yaml
permissions:
  contents: read
```

If a workflow only needs to read repository contents, don't give it write access.

---

## 3. Workflow Permissions

You can define permissions at workflow level:

```yaml
permissions:
  contents: read
```

You can also configure permissions for individual jobs when needed.

Example:

```yaml
permissions:
  contents: read

jobs:
  build:
    permissions:
      contents: read
```

Use the smallest permission set possible.

---

## 4. `GITHUB_TOKEN`

GitHub automatically provides a `GITHUB_TOKEN` to workflows.

It can be used for authenticated operations against the repository.

Example:

```yaml
env:
  GH_TOKEN: ${{ github.token }}
```

Its permissions should be restricted to what the workflow requires.

---

## 5. Don't Expose Secrets

Never print secrets.

Bad:

```yaml
- run: echo "${{ secrets.API_KEY }}"
```

Better:

```yaml
- name: Deploy
  env:
    API_KEY: ${{ secrets.API_KEY }}
  run: ./deploy.sh
```

The program can use the secret without deliberately printing it.

---

## 6. Repository Secrets

Sensitive values should be stored as GitHub Secrets rather than directly inside workflow files.

Examples:

```text
API_KEY
DATABASE_PASSWORD
DEPLOY_TOKEN
CLOUD_CREDENTIAL
```

Reference them with:

```yaml
${{ secrets.API_KEY }}
```

---

## 7. Never Hard-Code Credentials

Bad:

```yaml
env:
  API_KEY: "123456789"
```

Bad:

```yaml
run: login --password=my-password
```

Good:

```yaml
env:
  API_KEY: ${{ secrets.API_KEY }}
```

Credentials should never be committed to the repository.

---

## 8. Be Careful With Pull Requests

Pull requests can contain code or data controlled by contributors.

Workflows triggered by pull requests should be designed carefully, especially when they can access:

- Secrets
- Write permissions
- Deployment credentials
- Sensitive repository data

Never assume that code coming from an untrusted branch is safe to execute with powerful permissions.

---

## 9. `pull_request` vs `pull_request_target`

These triggers behave differently.

```yaml
on:
  pull_request:
```

runs in the context of the pull request.

```yaml
on:
  pull_request_target:
```

runs in the context of the base repository.

Because `pull_request_target` can have access to repository-level secrets and permissions, using it with untrusted pull-request code can create a serious security vulnerability.

Never blindly execute pull-request code with elevated privileges.

---

## 10. Protect Production Deployments

Production deployments should not automatically be available to every workflow.

Use GitHub Environments for sensitive deployments.

Example:

```yaml
jobs:
  deploy:
    environment: production
    runs-on: ubuntu-latest

    steps:
      - run: echo "Deploying"
```

Production environments can be protected with appropriate controls such as required reviewers.

---

## 11. Environment Secrets

Sensitive deployment credentials can be associated with an environment.

Example:

```text
development
staging
production
```

A production secret should not automatically be exposed to every development workflow.

Use the appropriate environment.

---

## 12. Pin Third-Party Actions

You may see:

```yaml
uses: actions/checkout@v4
```

This is convenient, but the referenced version can change over time.

For stronger supply-chain security, sensitive workflows can pin an action to a specific commit SHA.

Conceptually:

```yaml
uses: owner/action@<commit-sha>
```

This ensures the workflow uses the exact referenced revision.

---

## 13. Review Third-Party Actions

Before using an external action, check:

- Repository owner
- Source code
- Maintenance activity
- Release history
- Permissions required
- Security reputation
- Dependencies

Do not blindly copy actions from unknown repositories.

---

## 14. Avoid Untrusted Shell Injection

Be careful when putting event data directly into shell commands.

Potentially untrusted values include:

```text
Issue titles
Pull request titles
Commit messages
Branch names
User-provided inputs
```

Avoid blindly inserting such values into commands.

Instead, pass data through environment variables and handle it safely inside the script.

---

## 15. Example of Safer Handling

Instead of directly constructing a command from untrusted input, use an environment variable:

```yaml
- name: Process title
  env:
    TITLE: ${{ github.event.pull_request.title }}
  run: |
    echo "$TITLE"
```

This is generally safer than directly embedding the value into shell syntax.

Still validate and sanitize data when it is used by other commands.

---

## 16. Limit Secret Exposure

Only expose a secret to the step that needs it.

Example:

```yaml
steps:
  - name: Build
    run: npm run build

  - name: Deploy
    env:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
    run: ./deploy.sh
```

The build step doesn't need access to the deployment token.

---

## 17. Don't Use `secrets: inherit` Unnecessarily

With reusable workflows, you may see:

```yaml
secrets: inherit
```

This can expose more secrets than necessary.

Prefer passing only the required secrets.

Example:

```yaml
secrets:
  DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

This follows least privilege.

---

## 18. Protect Branches

Important branches such as:

```text
main
production
release
```

should be protected appropriately.

Branch protection can help control:

- Who can push
- Pull request requirements
- Required reviews
- Status checks
- Deployment processes

---

## 19. Require Successful Checks

A protected branch can require CI checks to pass before changes are merged.

Example:

```text
Pull Request
     ↓
Build
     ↓
Tests
     ↓
Security Checks
     ↓
Merge
```

This prevents many broken changes from reaching important branches.

---

## 20. Separate CI and CD

A useful approach is:

```text
CI
↓
Build
↓
Test
↓
Security Checks
↓
CD
↓
Staging
↓
Production
```

Don't give every CI workflow production deployment permissions.

---

## 21. Avoid Running Everything as a Single Job

Large workflows can become difficult to secure and debug.

Instead, separate responsibilities:

```text
Build Job
   ↓
Test Job
   ↓
Security Job
   ↓
Deploy Job
```

Then control dependencies using:

```yaml
needs:
```

---

## 22. Use Concurrency

Concurrency can prevent multiple workflows from performing conflicting operations at the same time.

Example:

```yaml
concurrency:
  group: production-deployment
  cancel-in-progress: false
```

This can be useful for deployment workflows.

---

## 23. Avoid Excessive Logging

Logs are useful for debugging, but unnecessary output can:

- Make debugging harder
- Increase log size
- Accidentally expose sensitive information

Log only what is useful.

Never intentionally log credentials.

---

## 24. Keep Dependencies Updated

Actions and dependencies can contain vulnerabilities.

Regularly review:

```text
GitHub Actions
Packages
Docker images
Runtime versions
Third-party tools
```

Keep important dependencies maintained and updated.

---

## 25. Use Dependency and Security Scanning

Security tools can detect vulnerable dependencies and insecure code.

Depending on your project, you may use tools such as:

```text
CodeQL
Dependabot
Dependency Review
Secret scanning
```

These can help detect problems before deployment.

---

## 26. Protect Self-Hosted Runners

Self-hosted runners require additional care.

They can potentially be exposed to:

- Malicious workflow code
- Untrusted pull requests
- Persistent files
- Credentials
- Network resources

Do not casually use a powerful self-hosted runner for untrusted workflows.

---

## 27. Keep Runner Permissions Limited

A runner should have only the access required for its workload.

Avoid giving a runner unnecessary access to:

```text
Production servers
Cloud accounts
Internal networks
Sensitive files
Long-lived credentials
```

---

## 28. Avoid Long-Lived Credentials

Where possible, prefer short-lived or temporary credentials.

For cloud deployments, modern identity mechanisms can allow workflows to authenticate without storing long-lived cloud passwords or access keys.

The goal is:

```text
Short-lived credential
        ↓
Use
        ↓
Expire
```

instead of:

```text
Permanent credential
        ↓
Stored indefinitely
```

---

## 29. Use OIDC for Cloud Authentication

GitHub Actions can use OpenID Connect (OIDC) to obtain short-lived cloud credentials from supported cloud providers.

Conceptually:

```text
GitHub Actions
      ↓
OIDC Identity
      ↓
Cloud Provider
      ↓
Temporary Credentials
      ↓
Deployment
```

This can reduce the need for long-lived cloud access keys stored as secrets.

---

## 30. Review Workflow Changes

Treat workflow files as code.

Review changes to:

```text
.github/workflows/
.github/actions/
```

before merging them.

A small workflow modification can change:

- Permissions
- Secret access
- Deployment behavior
- Repository access

---

## 31. Example Secure Workflow

```yaml
name: Secure CI

on:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Run tests
        run: npm test

  deploy:
    needs: test
    environment: production
    runs-on: ubuntu-latest

    steps:
      - name: Deploy
        env:
          DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
        run: ./deploy.sh
```

Important security concepts demonstrated:

```text
Limited permissions
        ↓
Test first
        ↓
Deployment depends on tests
        ↓
Production environment
        ↓
Secret exposed only to deploy step
```

---

## 32. Security Checklist

Before using a workflow in production, check:

```text
[ ] Least-privilege permissions
[ ] Secrets stored securely
[ ] No hard-coded credentials
[ ] Third-party actions reviewed
[ ] Important actions appropriately pinned
[ ] Pull request workflows reviewed
[ ] Production environment protected
[ ] Secrets exposed only where needed
[ ] Untrusted input handled safely
[ ] Self-hosted runners secured
[ ] Dependencies maintained
[ ] Security scanning considered
[ ] Workflow changes reviewed
[ ] Logs don't expose sensitive information
```

---

## 33. Practice

Create a workflow that:

1. Uses:

```yaml
permissions:
  contents: read
```

2. Checks out the repository.
3. Runs tests.
4. Has a separate deployment job.
5. Makes deployment depend on successful tests.
6. Uses a production environment.
7. Uses a secret only in the deployment step.

---

## 34. Challenge

Design a secure CI/CD workflow:

```text
Pull Request
     ↓
Build
     ↓
Test
     ↓
Security Check
     ↓
Merge
     ↓
Staging
     ↓
Production
```

Requirements:

- Use least-privilege permissions.
- Don't expose secrets during CI.
- Protect production.
- Use environment-specific secrets.
- Make deployment depend on successful checks.
- Avoid unnecessary secret inheritance.
- Review third-party actions.

---

## 35. Interview Questions

### Q1. What is the principle of least privilege?

Giving a workflow or application only the permissions it actually needs.

### Q2. What is `GITHUB_TOKEN`?

A GitHub-provided authentication token available to workflows for interacting with GitHub resources according to its configured permissions.

### Q3. Why shouldn't secrets be printed?

Because logs can expose sensitive credentials.

### Q4. Why should third-party actions be reviewed?

An action executes code in your workflow and may have access to repository resources or secrets.

### Q5. Why can `pull_request_target` be dangerous?

It runs in the context of the base repository and may have access to elevated permissions or secrets. Executing untrusted pull-request code in that context can create security vulnerabilities.

### Q6. Why use GitHub Environments?

They help protect deployments and separate environment-specific configuration and secrets.

### Q7. What is OIDC?

OpenID Connect allows GitHub Actions to establish trusted identity with supported cloud providers and obtain short-lived credentials.

### Q8. Why pin actions?

Pinning can make the exact action revision used by a workflow predictable and reduce supply-chain risk.

### Q9. Why are self-hosted runners risky?

They may provide persistent access to machines, networks, credentials, or other resources and therefore require stronger isolation and security controls.

### Q10. What is secret minimization?

Only providing a secret to the workflow or step that actually requires it.

---

# Summary

GitHub Actions security is based on controlling what workflows can access and execute.

The most important principles are:

```text
Least Privilege
      ↓
Protect Secrets
      ↓
Review Actions
      ↓
Handle Untrusted Input Safely
      ↓
Protect Deployments
      ↓
Secure Runners
      ↓
Monitor Dependencies
```

Remember:

```text
Don't trust workflow code blindly.
Don't expose secrets unnecessarily.
Don't give unnecessary permissions.
Don't execute untrusted code with powerful credentials.
```

A secure CI/CD pipeline should make it difficult for a compromised workflow to affect systems beyond what it actually needs.

Next topic: GitHub Actions Advanced Workflow Patterns


