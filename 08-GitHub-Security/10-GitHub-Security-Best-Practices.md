GitHub Security Best Practices

Introduction

GitHub security is not a single feature. A secure repository uses multiple layers of protection to reduce the risk of leaked credentials, vulnerable dependencies, insecure code, and unauthorized changes.

---

Security Layers

A strong GitHub security setup can include:

                    GitHub Repository
                           │
       ┌───────────────────┼───────────────────┐
       │                   │                   │
       ▼                   ▼                   ▼
 Secret Scanning      Code Scanning       Dependabot
       │                   │                   │
       ▼                   ▼                   ▼
 Credentials          Source Code        Dependencies
       │                   │                   │
       └───────────────────┼───────────────────┘
                           ▼
                    Branch Protection
                           │
                           ▼
                    Pull Requests
                           │
                           ▼
                     Code Review
                           │
                           ▼
                         Merge

---

1. Never Commit Secrets

Never store real credentials directly in source code.

Avoid:

API_KEY = "real-api-key"

Use environment variables or an appropriate secret-management system instead.

---

2. Use ".gitignore"

Sensitive local files should be excluded where appropriate.

Example:

.env
secrets/
*.key
*.pem

Remember that ".gitignore" does not remove secrets that were already committed.

---

3. Use GitHub Secrets

For GitHub Actions and CI/CD workflows, store sensitive values as GitHub Secrets.

Example:

env:
  API_KEY: ${{ secrets.API_KEY }}

Never hardcode the actual credential in the workflow.

---

4. Enable Secret Scanning

Secret Scanning can help detect credentials accidentally exposed in repositories.

Use it alongside good development practices rather than relying on it as the only protection.

---

5. Use Push Protection

When available, push protection can help prevent supported secrets from being pushed to a repository.

Conceptually:

git push
   │
   ▼
Secret Detection
   │
   ├── Safe → Push
   │
   └── Secret Detected → Warn / Block

---

6. Keep Dependencies Updated

Outdated dependencies can contain known vulnerabilities.

Use:

Dependabot

to help monitor dependencies and create update Pull Requests.

---

7. Review Dependency Changes

Use Dependency Review to examine dependency changes introduced by Pull Requests.

Before merging:

Dependency Changed
       ↓
Dependency Review
       ↓
Security Check
       ↓
Tests
       ↓
Review
       ↓
Merge

---

8. Use Code Scanning

Code scanning can analyze source code for potential security vulnerabilities.

CodeQL is one technology used by GitHub for this purpose.

A good workflow can run security analysis during Pull Requests.

---

9. Protect Important Branches

Protect branches such as:

main
production
release

Useful protections include:

- Pull Request requirements.
- Required reviews.
- Required status checks.
- Force-push restrictions.
- Branch deletion restrictions.

---

10. Use Pull Requests

Avoid directly pushing unreviewed work into important branches.

Recommended workflow:

Feature Branch
      ↓
Pull Request
      ↓
Automated Checks
      ↓
Code Review
      ↓
Approval
      ↓
Merge

---

11. Review GitHub Actions Carefully

GitHub Actions workflows can have access to:

- Repository code.
- Tokens.
- Secrets.
- External services.

Only use trusted Actions and review workflow permissions carefully.

Prefer minimum required permissions.

Example:

permissions:
  contents: read

---

12. Use Least Privilege

Give users and workflows only the permissions they actually need.

Avoid unnecessarily broad permissions.

Conceptually:

Required Permission
        ↓
Grant Only That Permission
        ↓
Reduce Attack Surface

---

13. Rotate Exposed Credentials

If a real secret is exposed:

REVOKE
   ↓
ROTATE
   ↓
REPLACE
   ↓
INVESTIGATE

Do not simply delete the secret from the latest version of the file.

---

14. Keep Security Documentation

A repository can contain:

SECURITY.md

This explains how security vulnerabilities should be reported.

Good security documentation makes responsible disclosure easier.

---

15. Review Security Alerts

Do not ignore security alerts indefinitely.

Investigate:

- What is affected?
- How severe is it?
- Which versions are affected?
- Is a patched version available?
- Does the issue affect your application?

---

16. Use Private Repositories When Appropriate

Not every project needs to be public.

Use private repositories when source code or project information should not be publicly accessible.

For public repositories:

Never assume "it's just a small project"

Even small repositories can accidentally expose credentials.

---

17. Keep Your Local Environment Secure

Repository security also depends on local development practices.

Avoid storing credentials in:

Source Code
Public Files
Screenshots
Commit Messages
Issues
Pull Requests
Chat Messages

---

18. Review Before Committing

Before:

git commit

check:

git status

and:

git diff

Ask:

Did I include anything sensitive?

---

19. Review Before Pushing

Before:

git push

check:

- Correct branch.
- Correct files.
- No secrets.
- No unnecessary files.
- Tests pass.
- Commit message is meaningful.

---

20. Combine Multiple Security Features

No single tool can detect every security problem.

A strong repository can combine:

Secret Scanning
       +
Push Protection
       +
Dependabot
       +
Dependency Review
       +
Code Scanning
       +
Branch Protection
       +
Code Review
       +
Testing

---

Complete Secure Development Workflow

Write Code
    │
    ▼
Review Changes
    │
    ▼
Check for Secrets
    │
    ▼
Commit
    │
    ▼
Push Feature Branch
    │
    ▼
Create Pull Request
    │
    ├── Code Scanning
    ├── Dependency Review
    ├── Automated Tests
    └── Security Checks
    │
    ▼
Code Review
    │
    ▼
Approval
    │
    ▼
Merge
    │
    ▼
Protected Main Branch

---

Security Checklist

Before pushing:

- [ ] No passwords committed.
- [ ] No API keys committed.
- [ ] No private keys committed.
- [ ] ".gitignore" configured.
- [ ] Dependencies checked.
- [ ] Correct branch selected.
- [ ] Changes reviewed.

Before merging:

- [ ] Pull Request reviewed.
- [ ] Tests pass.
- [ ] Code scanning passes where configured.
- [ ] Dependency checks pass where configured.
- [ ] Security issues resolved.
- [ ] Required approvals obtained.

---

Common Security Mistakes

Avoid:

Hardcoded API keys
Committed .env files
Public credentials
Unreviewed Pull Requests
Unprotected main branches
Outdated dependencies
Excessive GitHub Actions permissions
Ignoring security alerts
Force-pushing shared branches

---

Professional Security Mindset

Security should be considered throughout development:

Plan
 ↓
Develop
 ↓
Review
 ↓
Test
 ↓
Scan
 ↓
Deploy
 ↓
Monitor
 ↓
Improve

Security is an ongoing process, not a one-time GitHub setting.

---

Interview Questions

What are the most important GitHub security practices?

Protect secrets, update dependencies, scan code, protect important branches, review Pull Requests, limit permissions, and respond quickly to security alerts.

Why should GitHub Actions use minimum permissions?

Because excessive permissions increase the potential impact if a workflow or dependency is compromised.

What should you do if a secret is exposed?

Immediately revoke or rotate the credential, replace it with a new one, investigate the exposure, and remove the secret from the repository where appropriate.

Is Secret Scanning enough to secure a repository?

No. Repository security requires multiple layers including code scanning, dependency security, branch protection, access control, reviews, and secure development practices.

---

Practice

Review your own learning repository.

Check:

.gitignore
SECURITY.md
GitHub Secrets
Dependabot
Code Scanning
Secret Scanning
Branch Protection
Dependency Review

You do not need to enable every feature immediately. Understand what each feature does and enable appropriate protections as your projects become more serious.

---

Summary

A secure GitHub repository combines good development habits with GitHub security features.

The core principles are:

Protect Secrets
Keep Dependencies Updated
Scan Code
Protect Important Branches
Review Changes
Limit Permissions
Respond to Alerts

The goal is not to make development complicated. The goal is to prevent simple mistakes from becoming serious security problems.
