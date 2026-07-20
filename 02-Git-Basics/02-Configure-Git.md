# Configure Git

## Introduction

After installing Git, the first step is configuring your identity. Every commit in Git stores information about the author, including their name and email address. This helps identify who made each change in a project.

Git configuration only needs to be done once on a computer unless you want to change it later.

---

# Why Configure Git?

Git uses your name and email to:

- Identify the author of each commit
- Display commit history
- Collaborate with team members
- Connect commits with your GitHub account

Without configuration, Git cannot properly record commit information.

---

# Configuration Levels

Git supports three configuration levels:

| Level | Scope |
|------|------|
| System | All users on the computer |
| Global | Current user |
| Local | Current repository |

Most developers use the **Global** configuration.

---

# Configure Username

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "Aayush Suraj Tiwari"
```

---

# Configure Email

```bash
git config --global user.email "your@email.com"
```

Example:

```bash
git config --global user.email "aayush.works.t23@gmail.com"
```

Use the same email associated with your GitHub account.

---

# Verify Configuration

Check your username:

```bash
git config --global user.name
```

Check your email:

```bash
git config --global user.email
```

View all configured settings:

```bash
git config --list
```

---

# Where Is Configuration Stored?

Git saves your global configuration in a configuration file on your computer.

Every new repository will automatically use these settings.

---

# Best Practices

- Use your real name.
- Use the same email as your GitHub account.
- Verify your configuration after setting it.
- Configure Git immediately after installation.

---

# Common Mistakes

- Typing the wrong email address
- Forgetting quotation marks around names with spaces
- Using different emails for Git and GitHub without understanding the effect
- Forgetting the `--global` flag when you intend the setting to apply everywhere

---

# Interview Questions

### Why do we configure Git?

To identify the author of every commit.

---

### Which command sets the username?

```bash
git config --global user.name "Your Name"
```

---

### Which command sets the email?

```bash
git config --global user.email "your@email.com"
```

---

### Which command displays all Git configurations?

```bash
git config --list
```

---

# Practice

1. Configure your Git username.
2. Configure your Git email.
3. Verify both settings.
4. Display all Git configuration values.

---

# Summary

Git configuration is the first setup step after installation. By configuring your username and email, every commit you create will contain your identity, making collaboration and project history clear and organized.
