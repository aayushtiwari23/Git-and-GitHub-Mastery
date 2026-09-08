
CI stands for **Continuous Integration**.

Continuous Integration means automatically checking and testing code whenever developers make changes.

A CI pipeline commonly performs:

1. Checkout the code
2. Set up the required programming environment
3. Install dependencies
4. Run linting or code checks
5. Run tests
6. Build the application

The goal is to detect problems early.

---

## 2. What is CD?

CD can mean:

- Continuous Delivery
- Continuous Deployment

### Continuous Delivery

The application is automatically built, tested, and prepared for release.

The final production deployment usually requires manual approval.

### Continuous Deployment

The application is automatically deployed to production after passing the required checks.

---

## 3. Typical CI/CD Pipeline

A simple pipeline can look like this:

```text
Developer pushes code
        ↓
GitHub Repository
        ↓
CI starts
        ↓
Checkout code
        ↓
Install dependencies
        ↓
Lint / Code checks
        ↓
Run tests
        ↓
Build application
        ↓
Upload build artifact
        ↓
Deployment
        ↓
Staging
        ↓
Production
