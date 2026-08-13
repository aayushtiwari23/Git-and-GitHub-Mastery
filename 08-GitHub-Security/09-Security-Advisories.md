# GitHub Security Advisories

## Introduction

GitHub Security Advisories provide a way for maintainers to privately discuss, investigate, and disclose security vulnerabilities affecting their projects.

They are especially useful for open-source projects where a vulnerability may affect many users.

---

# What is a Security Advisory?

A security advisory is a structured record describing a security vulnerability in a project.

It can contain information such as:

- Vulnerability description
- Affected versions
- Patched versions
- Severity
- Vulnerability identifiers
- Recommended fixes

---

# Why Security Advisories Matter

Suppose an open-source library has a vulnerability:

```text
Vulnerability Discovered
          │
          ▼
Private Investigation
          │
          ▼
Fix Developed
          │
          ▼
Patched Version Released
          │
          ▼
Security Advisory Published
```

This allows users to understand the risk and update affected software.

---

# Private Vulnerability Reporting

Maintainers can provide a private way for security researchers to report vulnerabilities.

Instead of publicly creating an issue containing sensitive details:

```text
Researcher
    │
    ▼
Private Report
    │
    ▼
Maintainer
    │
    ▼
Investigation
```

This reduces the chance that vulnerability details are exposed before a fix is available.

---

# Why Public Disclosure Can Be Dangerous

Imagine a vulnerability is publicly posted before a fix exists:

```text
Vulnerability Details
        │
        ▼
Public Internet
        │
        ▼
Attackers Discover It
        │
        ▼
Users Remain Vulnerable
```

Responsible disclosure gives maintainers time to investigate and fix the issue.

---

# Security Advisory Workflow

```text
Vulnerability Found
        │
        ▼
Private Report
        │
        ▼
Maintainer Investigation
        │
        ▼
Confirm Vulnerability
        │
        ▼
Develop Fix
        │
        ▼
Test Fix
        │
        ▼
Release Patched Version
        │
        ▼
Publish Advisory
```

---

# Affected Versions

A security advisory can specify which versions are affected.

Example:

```text
Affected:
1.0.0 - 1.4.2

Patched:
1.4.3
```

Users running affected versions should update to a patched version.

---

# Severity

Security vulnerabilities can have different levels of severity.

Common categories include:

```text
Low
Moderate
High
Critical
```

Severity helps users understand how urgently they should respond.

---

# Common Vulnerability Scoring

Security vulnerabilities can be evaluated using standardized systems such as CVSS.

CVSS stands for:

```text
Common Vulnerability Scoring System
```

It provides a standardized way to communicate the severity of vulnerabilities.

---

# CVE

CVE stands for:

```text
Common Vulnerabilities and Exposures
```

A CVE identifier provides a standardized reference for a publicly known vulnerability.

Example format:

```text
CVE-YYYY-NNNNN
```

The exact identifier depends on the vulnerability.

---

# Security Advisory vs Security Issue

These should not be confused.

| Security Advisory | GitHub Issue |
|--------------------|--------------|
| Designed for vulnerabilities | General project discussion |
| Can contain sensitive security information | Usually public |
| Supports security disclosure workflows | Used for bugs/features |
| Helps coordinate vulnerability fixes | General collaboration |

---

# Maintainer Responsibilities

When receiving a security report, maintainers should:

1. Acknowledge the report.
2. Investigate the vulnerability.
3. Determine affected versions.
4. Develop a fix.
5. Test the fix.
6. Release a patched version.
7. Communicate the security impact.
8. Publish an advisory when appropriate.

---

# Researcher Responsibilities

Security researchers should:

- Report vulnerabilities responsibly.
- Avoid unnecessary exploitation.
- Avoid accessing or modifying other people's data.
- Provide enough information to reproduce the problem.
- Give maintainers reasonable time to respond.
- Avoid publicly exposing sensitive details prematurely.

---

# Example

Imagine a package:

```text
authentication-library
```

Version:

```text
2.3.0
```

contains a vulnerability.

A researcher discovers it.

The process could be:

```text
Researcher
    │
    ▼
Private Report
    │
    ▼
Maintainer Confirms
    │
    ▼
Version 2.3.1 Developed
    │
    ▼
Security Testing
    │
    ▼
2.3.1 Released
    │
    ▼
Advisory Published
```

Users can then update:

```text
2.3.0 → 2.3.1
```

---

# Security Advisories and Dependabot

Security Advisories can provide information that helps identify vulnerable dependencies.

Conceptually:

```text
Security Advisory
       │
       ▼
Vulnerable Package
       │
       ▼
Dependabot
       │
       ▼
Update Pull Request
       │
       ▼
Patched Version
```

This is one reason dependency security is important.

---

# Security Advisories and GitHub Security

Security Advisories are part of a broader security workflow:

```text
                 GitHub Security
                       │
       ┌───────────────┼───────────────┐
       │               │               │
       ▼               ▼               ▼
Secret Scanning   Code Scanning   Dependabot
       │               │               │
       └───────────────┼───────────────┘
                       ▼
              Security Advisories
```

---

# Best Practices

- Report vulnerabilities privately.
- Do not publish exploit details prematurely.
- Clearly identify affected versions.
- Release a patched version when possible.
- Communicate security impact clearly.
- Encourage users to update.
- Keep security documentation available.

---

