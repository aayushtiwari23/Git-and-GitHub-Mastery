# CI/CD Introduction

## Introduction

CI/CD stands for **Continuous Integration (CI)** and **Continuous Delivery/Continuous Deployment (CD)**. It is a modern software development practice that automates building, testing, and deploying applications.

Companies like Google, Microsoft, Amazon, Netflix, Meta, and Spotify use CI/CD to deliver software quickly, reliably, and with fewer errors.

---

# What is CI/CD?

CI/CD is a pipeline that automatically checks and delivers code whenever developers make changes.

Instead of manually building, testing, and deploying software, these tasks are automated.

---

# What is Continuous Integration (CI)?

Continuous Integration is the practice of regularly merging code changes into a shared repository.

Every time code is pushed:

- Source code is downloaded.
- The project is built.
- Automated tests are executed.
- Code quality checks are performed.

If any step fails, developers are notified immediately.

---

# What is Continuous Delivery (CD)?

Continuous Delivery automatically prepares the application for deployment after all tests pass.

Deployment is ready but requires **manual approval** before going to production.

---

# What is Continuous Deployment?

Continuous Deployment goes one step further.

If all tests pass, the application is deployed to production automatically without manual approval.

---

# CI/CD Pipeline

```text
Write Code
     │
git add
     │
git commit
     │
git push
     │
GitHub Actions
     │
Build
     │
Run Tests
     │
Code Quality Check
     │
Deploy
```

---

# Why Use CI/CD?

Developers use CI/CD to:

- Detect bugs early.
- Reduce manual work.
- Deploy software faster.
- Improve software quality.
- Increase development speed.
- Deliver updates frequently.

---

# CI vs CD

| Continuous Integration | Continuous Delivery | Continuous Deployment |
|------------------------|--------------------|-----------------------|
| Build code | Prepare for release | Automatically release |
| Run tests | Manual approval | No manual approval |
| Detect bugs | Verify deployment | Automatic production deployment |

---

# Popular CI/CD Tools

- GitHub Actions
- Jenkins
- GitLab CI/CD
- CircleCI
- Azure DevOps
- Travis CI

---

# Real-World Example

A developer pushes code to GitHub.

GitHub Actions automatically:

1. Downloads the project.
2. Installs dependencies.
3. Builds the application.
4. Runs unit tests.
5. Checks code quality.
6. Deploys the application if all checks pass.

The developer does not perform these steps manually.

---

# Advantages

- Faster releases.
- Fewer production bugs.
- Better software quality.
- Automated testing.
- Reliable deployments.
- Improved team productivity.

---

# Best Practices

- Commit small changes frequently.
- Automate testing.
- Keep the pipeline fast.
- Monitor failed builds.
- Protect production deployments.

---

# Common Mistakes

- Skipping automated tests.
- Ignoring failed pipelines.
- Deploying untested code.
- Making pipelines unnecessarily complex.
- Hardcoding secrets in workflow files.

---

# Interview Questions

### What does CI stand for?

Continuous Integration.

---

### What does CD stand for?

Continuous Delivery or Continuous Deployment.

---

### What is the difference between Continuous Delivery and Continuous Deployment?

Continuous Delivery requires manual approval before deployment.

Continuous Deployment automatically deploys after all tests pass.

---

### Name some CI/CD tools.

- GitHub Actions
- Jenkins
- GitLab CI/CD
- CircleCI
- Azure DevOps

---

### Why is CI/CD important?

It automates software development, reduces bugs, improves quality, and enables faster releases.

---

# Practice

1. Create a GitHub repository.
2. Add a simple GitHub Actions workflow.
3. Push code.
4. Verify the workflow runs.
5. Make another commit.
6. Observe the workflow running automatically.

---

# Summary

CI/CD is a modern software development practice that automates building, testing, and deploying applications. It helps teams deliver software faster, improve quality, and reduce manual effort, making it an essential skill for every software engineer and DevOps professional.
