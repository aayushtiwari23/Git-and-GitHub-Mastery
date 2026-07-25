
# GitHub Pages

## Introduction

GitHub Pages is a free hosting service provided by GitHub that allows you to publish static websites directly from a GitHub repository. It is commonly used for portfolio websites, documentation, project showcases, and blogs.

No separate web hosting service is required.

---

# What is GitHub Pages?

GitHub Pages hosts static websites built with:

- HTML
- CSS
- JavaScript
- Markdown

Each website is served directly from a GitHub repository.

---

# Why Use GitHub Pages?

Developers use GitHub Pages to:

- Build personal portfolios
- Publish project documentation
- Host landing pages
- Create blogs
- Showcase open-source projects

---

# Features

- Free hosting
- HTTPS support
- Custom domain support
- Easy deployment
- Integrated with GitHub

---

# How to Create a GitHub Pages Website

### Step 1

Create a GitHub repository.

Example:

```text
portfolio
```

---

### Step 2

Add an `index.html` file.

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Portfolio</title>
</head>
<body>
    <h1>Hello, GitHub Pages!</h1>
</body>
</html>
```

---

### Step 3

Commit and push the changes.

---

### Step 4

Open:

**Settings → Pages**

---

### Step 5

Under **Build and Deployment**:

- Source → Deploy from a branch

---

### Step 6

Select:

- Branch: `main`
- Folder: `/ (root)`

---

### Step 7

Click **Save**.

GitHub will publish your website.

---

# Website URL

Your website will usually be available at:

```text
https://your-username.github.io/repository-name/
```

Example:

```text
https://aayushsuraj.github.io/portfolio/
```

---

# Repository Structure

```text
portfolio
│
├── index.html
├── style.css
├── script.js
└── images
```

---

# Real-World Example

A student builds a personal portfolio.

The repository contains:

- About Me
- Projects
- Skills
- Contact Information

Using GitHub Pages, the portfolio becomes publicly accessible through a web link that can be shared on LinkedIn and in resumes.

---

# GitHub Pages vs Traditional Hosting

| GitHub Pages | Traditional Hosting |
|--------------|---------------------|
| Free | Usually paid |
| Static websites | Static and dynamic websites |
| Easy setup | More configuration required |
| GitHub integration | Separate hosting platform |

---

# Best Practices

- Keep the homepage simple.
- Optimize images.
- Use responsive design.
- Update content regularly.
- Test your website before publishing.

---

# Common Mistakes

- Missing `index.html`.
- Wrong branch selected.
- Incorrect folder selection.
- Expecting server-side languages like PHP to work.

---

# Interview Questions

### What is GitHub Pages?

GitHub Pages is a free service for hosting static websites directly from GitHub repositories.

---

### Which file is required?

```text
index.html
```

---

### Can GitHub Pages run PHP or Node.js?

No.

It only hosts static websites.

---

### What is the default website URL?

```text
https://username.github.io/repository-name/
```

---

# Practice

1. Create a repository named `portfolio`.
2. Add an `index.html` file.
3. Enable GitHub Pages.
4. Visit the published website.
5. Update the page and republish it.

---

# Summary

GitHub Pages provides a simple and free way to publish static websites directly from GitHub. It is widely used for portfolios, project documentation, and personal websites, making it an excellent tool for students and developers.
