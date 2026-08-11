# Code Scanning and CodeQL

## Introduction

GitHub Code Scanning helps identify security vulnerabilities and coding errors in a repository.

One of the technologies used by GitHub for code scanning is **CodeQL**.

CodeQL analyzes source code and treats code as data that can be queried to find potentially dangerous patterns.

---

# What is Code Scanning?

Code scanning is an automated security analysis feature that examines source code for potential problems.

It can help identify:

- Security vulnerabilities
- Coding mistakes
- Unsafe programming patterns
- Potential injection vulnerabilities
- Other security-related issues

---

# What is CodeQL?

CodeQL is GitHub's semantic code analysis technology.

Instead of simply searching for text patterns, CodeQL can analyze relationships and behavior within source code.

Example:

```text
Source Code
     │
     ▼
CodeQL Analysis
     │
     ▼
Security Queries
     │
     ▼
Potential Vulnerability
     │
     ▼
Security Alert
```

---

# Why Use CodeQL?

CodeQL can help developers:

- Find vulnerabilities earlier.
- Automate security analysis.
- Review security issues through GitHub.
- Prevent vulnerable code from reaching production.
- Improve secure coding practices.

---

# Supported Languages

CodeQL supports several major programming languages.

Examples include:

```text
C/C++
C#
Java
JavaScript
TypeScript
Python
Go
Ruby
Kotlin
Swift
```

The exact supported languages and analysis capabilities can change over time.

---

# Code Scanning Workflow

```text
Developer Writes Code
        │
        ▼
Push / Pull Request
        │
        ▼
CodeQL Analysis
        │
        ▼
Security Queries
        │
        ▼
Potential Issue Found
        │
        ▼
GitHub Security Alert
        │
        ▼
Developer Fixes Issue
        │
        ▼
Scan Again
```

---

# Code Scanning in Pull Requests

Code scanning can run when a Pull Request is created or updated.

Example:

```text
Pull Request
     │
     ▼
CodeQL Scan
     │
     ├── No Issues
     │       │
     │       ▼
     │     Continue
     │
     └── Issue Found
             │
             ▼
       Review and Fix
```

This allows security problems to be discovered before code is merged.

---

# Example Security Problem

Suppose an application builds a database query directly from user input.

Potentially unsafe pattern:

```text
User Input
    │
    ▼
SQL Query
    │
    ▼
Database
```

A security analysis tool may identify the flow of untrusted input into a sensitive operation.

The developer can then fix the vulnerability.

---

# CodeQL Database

CodeQL creates a representation of the source code that can be analyzed.

Conceptually:

```text
Source Code
     │
     ▼
CodeQL Database
     │
     ▼
Queries
     │
     ▼
Results
```

Queries can search for specific security patterns.

---

# CodeQL Queries

A CodeQL query defines what the analyzer should look for.

Different queries can identify different types of problems.

Examples include:

- Injection vulnerabilities
- Unsafe data flows
- Authentication problems
- Improper validation
- Dangerous API usage

---

# Default CodeQL Setup

GitHub can provide a default CodeQL configuration for supported repositories.

A typical workflow may look like:

```text
.github/
└── workflows/
    └── codeql.yml
```

The workflow can automatically run CodeQL analysis.

---

# Custom CodeQL Configuration

Advanced projects can customize CodeQL.

For example, teams can configure:

- Languages
- Analysis frequency
- Query suites
- Build requirements
- Additional security queries

---

# Code Scanning Alerts

When Code Scanning finds a potential problem, GitHub can create an alert.

An alert may include:

```text
Problem
Severity
Location
Description
Security Category
Recommended Action
```

Developers can investigate and fix the issue.

---

# Severity

Security findings may have different levels of severity.

Common categories include:

```text
Critical
High
Medium
Low
```

The exact severity depends on the vulnerability and analysis system.

---

# Code Scanning vs Dependabot

| Code Scanning | Dependabot |
|---------------|------------|
| Analyzes source code | Analyzes dependencies |
| Finds coding vulnerabilities | Finds vulnerable/outdated packages |
| Uses CodeQL and other tools | Monitors dependency versions |
| Can identify insecure code patterns | Can create dependency update PRs |

---

# Code Scanning vs Secret Scanning

| Code Scanning | Secret Scanning |
|---------------|-----------------|
| Analyzes source code | Detects exposed secrets |
| Finds coding vulnerabilities | Finds credentials/tokens |
| Uses security analysis | Uses secret detection |
| Helps secure application logic | Helps protect credentials |

---

# Secure Development Workflow

A strong GitHub security workflow can combine:

```text
                 GitHub Repository
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
   Secret Scanning  Dependabot   Code Scanning
          │             │             │
          ▼             ▼             ▼
       Secrets      Dependencies    Source Code
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                  Security Alerts
                        │
                        ▼
                    Fix Issues
```

---

# Best Practices

- Enable code scanning for important repositories.
- Run scans regularly.
- Run security checks on Pull Requests.
- Review security alerts.
- Fix high-severity vulnerabilities quickly.
- Keep CodeQL configuration maintained.
- Combine code scanning with dependency and secret scanning.

---

# Common Mistakes

- Assuming code scanning finds every vulnerability.
- Ignoring security alerts.
- Treating automated scanning as a replacement for code review.
- Running security scans only after deployment.
- Not investigating false positives.

---

# Limitations

Automated security tools are not perfect.

A clean scan does **not** guarantee that an application is completely secure.

Security should combine:

```text
Automated Scanning
+
Code Review
+
Testing
+
Secure Design
+
Developer Awareness
```

---

# Interview Questions

### What is GitHub Code Scanning?

GitHub Code Scanning is a security feature that analyzes source code to identify potential vulnerabilities and coding problems.

---

### What is CodeQL?

CodeQL is GitHub's semantic code analysis technology used to query source code for security vulnerabilities and other coding problems.

---

### What is the difference between CodeQL and Dependabot?

CodeQL analyzes source code, while Dependabot focuses primarily on dependencies and their versions or vulnerabilities.

---

### Can Code Scanning run on Pull Requests?

Yes. Code scanning can be integrated into Pull Request workflows to identify security issues before code is merged.

---

### Does a clean CodeQL scan guarantee secure code?

No. Automated analysis cannot detect every possible security problem.

---

# Practice

1. Create or open a GitHub repository.
2. Open the repository's:

```text
Security
```

section.

3. Explore:

```text
Code scanning
```

4. Check whether CodeQL is available for your repository.
5. Explore the setup options.
6. If appropriate, enable the default CodeQL workflow.
7. Create a small code change.
8. Open a Pull Request.
9. Observe the security checks.

Do not intentionally introduce a real exploitable vulnerability into a public repository just for testing.

---

# Summary

GitHub Code Scanning helps identify potential security vulnerabilities and coding problems automatically.

CodeQL provides powerful semantic analysis that can understand relationships within source code rather than simply searching for text.

A strong security workflow combines:

```text
Code Scanning
+
Dependabot
+
Secret Scanning
+
Code Review
+
Testing
```

Together, these practices help developers detect and fix security problems earlier in the software development lifecycle.
