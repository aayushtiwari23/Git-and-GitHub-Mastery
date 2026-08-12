
# Security Policy

## Supported Versions

This project is primarily a learning repository.

Security fixes will be considered for the latest version of the project.

| Version | Supported |
|---------|-----------|
| Latest | ✅ |
| Older versions | ❌ |

---

## Reporting a Vulnerability

If you discover a genuine security vulnerability, please report it privately to the project maintainer.

Do not publicly disclose sensitive vulnerability details before they have been investigated.

When reporting a vulnerability, provide:

- A clear description of the issue.
- Steps to reproduce it.
- The affected file or component.
- The potential security impact.
- Any possible mitigation or fix.

---

## What Not to Include

Do not include real:

- Passwords
- API keys
- Access tokens
- Private keys
- Personal information

in a vulnerability report.

If sensitive information is accidentally exposed, revoke or rotate the affected credential immediately.

---

## Responsible Disclosure

Please allow the maintainer reasonable time to investigate and address a security issue before publicly disclosing detailed information.

The goal is to protect users and prevent exploitation while the issue is being fixed.

---

## Security Best Practices

Contributors should:

- Never commit secrets.
- Use environment variables for sensitive configuration.
- Keep dependencies updated.
- Review code before submitting Pull Requests.
- Follow secure coding practices.
- Avoid exposing credentials in issues, commits, or Pull Requests.

---

## Scope

This security policy applies to security vulnerabilities affecting this repository and its associated code.

It does not cover vulnerabilities in third-party services or dependencies that are outside the project's direct control.

---

## Important Note

This repository is primarily intended for learning and educational purposes.

Do not use it to store production credentials, private information, or other sensitive data.
