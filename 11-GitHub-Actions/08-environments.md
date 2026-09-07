
An environment can require approval before a deployment continues.

Example:

```text
Production Deployment
        ↓
Approval Required
        ↓
Reviewer Approves
        ↓
Deployment Continues
```

This is useful when a team wants someone to review production deployments.

---

## 9. Deployment Branch Restrictions

An environment can restrict which branches or tags are allowed to deploy.

For example:

```text
production
     ↓
Only main branch
```

This can help prevent an unexpected development branch from deploying directly to production.

---

## 10. Development, Staging and Production

A common setup is:

```text
Development
     ↓
Develop and test

Staging
     ↓
Final testing

Production
     ↓
Real users
```

Each environment can have its own:

- Secrets
- Variables
- Protection rules
- Deployment restrictions

---

## 11. Complete Example

```yaml
name: Deploy

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Deploy application
        run: echo "Deploying application"

      - name: Show environment
        run: echo "Production deployment"
```

The important part is:

```yaml
environment: production
```

---

## 12. Multiple Environments

You can create different jobs for different environments.

Example:

```yaml
jobs:

  development:
    runs-on: ubuntu-latest
    environment: development

    steps:
      - run: echo "Development deployment"

  staging:
    runs-on: ubuntu-latest
    environment: staging

    steps:
      - run: echo "Staging deployment"

  production:
    runs-on: ubuntu-latest
    environment: production

    steps:
      - run: echo "Production deployment"
```

These jobs can run independently.

If you want them to run in sequence, use `needs`.

---

## 13. Environment Deployment Flow

Example:

```yaml
jobs:

  development:
    runs-on: ubuntu-latest
    environment: development

    steps:
      - run: echo "Deploying to development"

  staging:
    needs: development
    runs-on: ubuntu-latest
    environment: staging

    steps:
      - run: echo "Deploying to staging"

  production:
    needs: staging
    runs-on: ubuntu-latest
    environment: production

    steps:
      - run: echo "Deploying to production"
```

The flow becomes:

```text
development
     ↓
staging
     ↓
production
```

---

## 14. Environment Secrets vs Repository Secrets

Repository secrets and environment secrets serve different purposes.

Repository secret:

```text
Repository
    ↓
Repository-level configuration
```

Environment secret:

```text
Environment
    ↓
Environment-specific configuration
```

For example:

```text
Repository
│
├── Development
│   └── API_KEY
│
├── Staging
│   └── API_KEY
│
└── Production
    └── API_KEY
```

This allows each environment to use its own credentials.

---

## 15. Why Protect Production?

Imagine a workflow that performs:

```text
Deploy application
```

against a production environment.

A mistake could affect real users.

Production environments can therefore use:

- Required approvals
- Branch restrictions
- Environment-specific secrets
- Deployment protection

The goal is to reduce accidental or unauthorized deployments.

---

## 16. Practice

Create these environments in your GitHub repository:

```text
development
staging
production
```

Then create:

```text
.github/workflows/environments.yml
```

Use:

```yaml
name: Environment Demo

on:
  workflow_dispatch:

jobs:

  development:
    runs-on: ubuntu-latest
    environment: development

    steps:
      - name: Development
        run: echo "Development environment"

  staging:
    needs: development
    runs-on: ubuntu-latest
    environment: staging

    steps:
      - name: Staging
        run: echo "Staging environment"

  production:
    needs: staging
    runs-on: ubuntu-latest
    environment: production

    steps:
      - name: Production
        run: echo "Production environment"
```

Run the workflow and observe the order.

---

## 17. Challenge

Modify the workflow so that:

```text
Development
      ↓
Staging
      ↓
Production
```

Production should use an environment secret called:

```text
DEPLOY_TOKEN
```

Do not print the token.

Instead, check whether it exists:

```bash
if [ -n "$DEPLOY_TOKEN" ]; then
  echo "Deployment token is configured."
else
  echo "Deployment token is not configured."
fi
```

Use:

```yaml
env:
  DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
```

---

## 18. Important Security Rules

Remember:

1. Don't put passwords directly in workflow files.
2. Use environment secrets for sensitive environment-specific values.
3. Protect production deployments.
4. Use approvals when required.
5. Restrict which branches can deploy to production when appropriate.
6. Don't expose deployment credentials in logs.
7. Give deployment jobs only the permissions they need.

---

## 19. Interview Questions

### Q1. What is a GitHub Actions environment?

An environment represents a deployment or execution environment such as development, staging, or production.

### Q2. Why are environments useful?

They allow you to separate configuration, secrets, and deployment protection rules between different environments.

### Q3. How do you assign an environment to a job?

```yaml
environment: production
```

### Q4. Can environments have their own secrets?

Yes.

### Q5. Why protect a production environment?

To reduce the risk of accidental or unauthorized production deployments.

### Q6. What are required reviewers?

People who must approve a deployment before a protected environment job can continue.

### Q7. Can you restrict which branches deploy to an environment?

Yes. Environment deployment rules can restrict allowed branches or tags.

### Q8. Give examples of common environments.

```text
Development
Staging
Production
```

---

# Summary

GitHub Actions environments help manage deployments safely.

A common structure is:

```text
Development
     ↓
Staging
     ↓
Production
```

An environment can have:

- Secrets
- Variables
- Protection rules
- Required approvals
- Branch/tag restrictions

Assign an environment to a job using:

```yaml
environment: production
```

The main idea is:

```text
Development → Develop and test

Staging → Test before release

Production → Protect carefully
```

Next topic: GitHub Actions Matrix Builds
