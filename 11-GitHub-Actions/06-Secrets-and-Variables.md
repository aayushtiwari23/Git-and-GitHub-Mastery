# GitHub Actions Secrets and Variables

## 1. What Are Secrets and Variables?

GitHub Actions workflows often need configuration values.

Examples:

- API keys
- Tokens
- Database credentials
- Application settings
- Environment names
- URLs
- Feature flags

These values can be stored using **variables** and **secrets**.

The main difference is:

```text
Variable
    ↓
Configuration value that is not highly sensitive

Secret
    ↓
Sensitive value that should be protected
```

---

# 2. Why Should We Not Hardcode Secrets?

Never put sensitive information directly inside workflow files.

Bad:

```yaml
steps:
  - run: |
      echo "API_KEY=abc123456"
```

The workflow file is part of the repository and may be visible to other people.

Instead, store sensitive values as GitHub Actions secrets.

---

# 3. GitHub Actions Secrets

Secrets are encrypted values designed for sensitive information.

Examples:

```text
API_KEY
DATABASE_PASSWORD
ACCESS_TOKEN
DEPLOYMENT_TOKEN
```

A workflow can access a secret using:

```yaml
${{ secrets.SECRET_NAME }}
```

Example:

```yaml
steps:
  - name: Use API key
    run: echo "API key is available"
    env:
      API_KEY: ${{ secrets.API_KEY }}
```

The actual secret value should not be written directly into the workflow.

---

# 4. Creating a Repository Secret

For a repository:

```text
Repository
→ Settings
→ Secrets and variables
→ Actions
→ New repository secret
```

Enter:

```text
Name:
API_KEY

Secret:
your-sensitive-value
```

Save it.

The secret can then be referenced as:

```yaml
${{ secrets.API_KEY }}
```

---

# 5. Using Secrets in a Workflow

Example:

```yaml
name: Secret Demo

on:
  workflow_dispatch:

jobs:
  demo:
    runs-on: ubuntu-latest

    steps:
      - name: Use secret
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: |
          echo "The API key has been loaded."
```

Notice that we do not print the secret itself.

---

# 6. Never Print Secrets

Avoid:

```yaml
run: echo "${{ secrets.API_KEY }}"
```

Even though GitHub attempts to redact known secret values from logs, you should never intentionally expose secrets.

Instead:

```yaml
run: echo "Secret loaded successfully"
```

Good security practice is to use secrets only where they are required.

---

# 7. Variables

Variables are useful for storing configuration values.

Examples:

```text
APP_NAME
ENVIRONMENT
NODE_VERSION
REGION
API_URL
```

Example:

```yaml
env:
  APP_NAME: MyApplication
```

Then:

```yaml
steps:
  - run: echo "$APP_NAME"
```

---

# 8. Workflow-Level Variables

A variable can be defined for the entire workflow.

Example:

```yaml
name: Variables Demo

on:
  workflow_dispatch:

env:
  APP_NAME: MyApplication
  ENVIRONMENT: production

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - run: |
          echo "Application: $APP_NAME"
          echo "Environment: $ENVIRONMENT"
```

The variables are available throughout the workflow unless overridden.

---

# 9. Job-Level Variables

Variables can also be defined for a specific job.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    env:
      ENVIRONMENT: development

    steps:
      - run: echo "Environment: $ENVIRONMENT"
```

Here the variable applies to the `build` job.

---

# 10. Step-Level Variables

Variables can also be defined for only one step.

Example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Test
        env:
          TEST_MODE: true
        run: echo "Test mode: $TEST_MODE"
```

The variable is available to that step.

---

# 11. Variable Scope

There are three common levels:

```text
Workflow
    ↓
Job
    ↓
Step
```

Example:

```yaml
env:
  LEVEL: workflow

jobs:
  build:
    env:
      LEVEL: job

    steps:
      - name: Example
        env:
          LEVEL: step
        run: echo "$LEVEL"
```

The more specific level overrides the broader value.

---

# 12. Repository Variables

GitHub also allows variables to be stored in repository settings.

Go to:

```text
Repository
→ Settings
→ Secrets and variables
→ Actions
→ Variables
```

Create a variable such as:

```text
APP_ENVIRONMENT = development
```

Then access it with:

```yaml
${{ vars.APP_ENVIRONMENT }}
```

Example:

```yaml
steps:
  - run: echo "Environment: ${{ vars.APP_ENVIRONMENT }}"
```

---

# 13. Secrets vs Variables

| Feature | Secrets | Variables |
|---|---|---|
| Sensitive information | Yes | Usually no |
| Example | API token | App name |
| Encrypted | Yes | Not intended as secret storage |
| Access syntax | `${{ secrets.NAME }}` | `${{ vars.NAME }}` |
| Should be exposed publicly? | No | Depends on value |

Simple rule:

```text
Sensitive → Secret
Configuration → Variable
```

---

# 14. Environment Variables vs GitHub Variables

These concepts are related but not identical.

An environment variable can be defined directly in YAML:

```yaml
env:
  APP_NAME: MyApp
```

A GitHub repository variable is stored in GitHub settings:

```text
Settings
→ Secrets and variables
→ Actions
→ Variables
```

It can then be referenced using:

```yaml
${{ vars.APP_NAME }}
```

---

# 15. Default GitHub Variables

GitHub Actions also provides built-in information through contexts.

For example:

```yaml
${{ github.repository }}
```

gives the repository name.

Other useful values include:

```yaml
${{ github.actor }}
${{ github.ref }}
${{ github.sha }}
${{ github.workflow }}
```

Example:

```yaml
steps:
  - run: |
      echo "Repository: ${{ github.repository }}"
      echo "Actor: ${{ github.actor }}"
      echo "Commit: ${{ github.sha }}"
```

These are GitHub-provided context values, not secrets.

---

# 16. Using a Secret as an Environment Variable

A common pattern is:

```yaml
steps:
  - name: Run application
    env:
      API_KEY: ${{ secrets.API_KEY }}
    run: python app.py
```

The application can read:

```text
API_KEY
```

from its environment.

This keeps the secret out of the source code.

---

# 17. Example: Python Application

Suppose your Python program contains:

```python
import os

api_key = os.getenv("API_KEY")

print("API key loaded:", api_key is not None)
```

Your workflow can provide the secret:

```yaml
name: Python App

on:
  workflow_dispatch:

jobs:
  run:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run application
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: python app.py
```

The Python application gets the value from its environment.

---

# 18. Environment Secrets

Secrets can also be associated with GitHub environments.

For example:

```text
development
staging
production
```

A production deployment may require different credentials from a development deployment.

Conceptually:

```text
Development
    ↓
Development secrets

Staging
    ↓
Staging secrets

Production
    ↓
Production secrets
```

This is useful for separating sensitive configuration between deployment environments.

Environments will be covered in more detail later.

---

# 19. Secrets in Pull Requests

Be careful when workflows run for pull requests, especially when code comes from forks.

You should not assume that every workflow execution has access to repository secrets.

This is an important security consideration.

Never design a workflow that blindly exposes sensitive credentials to untrusted code.

---

# 20. Least Privilege

Give workflows and credentials only the access they actually need.

For example, if a workflow only needs to read repository information, don't give it unnecessary write permissions.

Security principle:

```text
Minimum required access
        ↓
Lower risk
```

This principle is called **least privilege**.

---

# 21. Bad Example

Avoid this:

```yaml
env:
  PASSWORD: my-super-secret-password
```

The password is directly stored in the repository.

Another bad example:

```yaml
run: curl https://example.com?token=my-secret-token
```

The token is exposed in the workflow.

---

# 22. Better Example

Store the sensitive value as a GitHub secret:

```text
DEPLOY_TOKEN
```

Then:

```yaml
steps:
  - name: Deploy
    env:
      DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
    run: ./deploy.sh
```

The workflow references the secret without hardcoding its value.

---

# 23. Practice Workflow

Create:

```text
.github/workflows/secrets-and-variables.yml
```

Use:

```yaml
name: Secrets and Variables

on:
  workflow_dispatch:

env:
  APP_NAME: GitHub-Actions-Demo

jobs:
  demo:
    runs-on: ubuntu-latest

    steps:
      - name: Display variable
        run: echo "Application: $APP_NAME"

      - name: Check secret
        env:
          DEMO_SECRET: ${{ secrets.DEMO_SECRET }}
        run: |
          if [ -n "$DEMO_SECRET" ]; then
            echo "Secret is available."
          else
            echo "Secret is not configured."
          fi
```

Before running it, create a repository secret:

```text
Name:
DEMO_SECRET

Value:
test-secret-value
```

Do not print the actual secret.

---

# 24. Practice With Repository Variables

Create a repository variable:

```text
APP_VERSION
```

Set its value to:

```text
1.0.0
```

Then add:

```yaml
- name: Display application version
  run: echo "Version: ${{ vars.APP_VERSION }}"
```

Run the workflow manually.

---

# 25. Challenge

Create a workflow that uses:

```text
Repository variable:
APP_ENVIRONMENT

Repository secret:
API_TOKEN
```

The workflow should print:

```text
Environment: <environment value>
API token is configured.
```

It must **not** print the API token.

Your workflow should demonstrate the difference between:

```yaml
${{ vars.APP_ENVIRONMENT }}
```

and:

```yaml
${{ secrets.API_TOKEN }}
```

---

# 26. Important Security Rules

Remember:

1. Never hardcode passwords in workflow files.
2. Never commit API keys to Git.
3. Store sensitive credentials as secrets.
4. Don't intentionally print secrets.
5. Use variables for normal configuration.
6. Use least-privilege permissions.
7. Be careful with workflows triggered by untrusted code.
8. Keep production credentials separated from development credentials.
9. Give secrets only to jobs that actually need them.
10. Rotate compromised credentials immediately.

---

# 27. Interview Questions

### Q1. What is the difference between a GitHub Actions secret and variable?

Secrets are intended for sensitive values, while variables are generally used for non-sensitive configuration.

### Q2. How do you access a secret?

```yaml
${{ secrets.SECRET_NAME }}
```

### Q3. How do you access a GitHub Actions variable?

```yaml
${{ vars.VARIABLE_NAME }}
```

### Q4. Where can environment variables be defined?

They can be defined at workflow, job, or step level.

### Q5. Why shouldn't API keys be hardcoded?

Because the workflow file is part of the repository and the credential could be exposed.

### Q6. What is least privilege?

Giving a workflow, user, or credential only the permissions required to perform its task.

### Q7. Should secrets be printed in workflow logs?

No.

### Q8. Why are pull requests from forks important from a security perspective?

Because workflows involving untrusted fork code must be designed carefully so sensitive credentials aren't exposed to potentially malicious code.

---

# 28. Summary

GitHub Actions provides secrets and variables for managing configuration.

Use:

```yaml
${{ secrets.NAME }}
```

for sensitive values.

Use:

```yaml
${{ vars.NAME }}
```

for GitHub-managed non-sensitive configuration.

Environment variables can also be defined directly:

```yaml
env:
  APP_NAME: MyApp
```

The key security principle is:

```text
Never hardcode sensitive credentials.
        ↓
Store them securely.
        ↓
Give workflows only the access they need.
```

Next topic: **GitHub Actions Artifacts**
