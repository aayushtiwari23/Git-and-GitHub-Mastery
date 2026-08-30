
Example:

```text
v1.0.0
```

Conceptually:

```text
A ─ B ─ C ─ D
          ↑
        v1.0.0
```

The tag identifies a particular point in project history.

---

# Creating a Tag

Create a tag:

```bash
git tag v1.0.0
```

View tags:

```bash
git tag
```

Push the tag:

```bash
git push origin v1.0.0
```

---

# Annotated Tags

An annotated tag stores additional information.

Create one:

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

Push:

```bash
git push origin v1.0.0
```

Annotated tags are commonly useful for releases.

---

# Tag vs Branch

A branch normally moves as new commits are added.

A tag generally identifies a fixed point in history.

Example:

```text
Branch:

A ─ B ─ C ─ D
          ↑
        main
             ↓
             E
```

The branch continues moving.

Tag:

```text
A ─ B ─ C ─ D
        ↑
      v1.0.0
```

The tag identifies that specific release point.

---

# GitHub Release

A GitHub Release can be created from a tag.

Conceptually:

```text
Commit
  ↓
Tag
  ↓
GitHub Release
```

A Release can contain:

```text
Version
Release Notes
Changes
Assets
Source Code
```

---

# Release Notes

Release notes explain what changed in a version.

Example:

```md
# v1.1.0

## New Features

- Added dark mode
- Added search

## Improvements

- Improved dashboard performance

## Bug Fixes

- Fixed login validation
- Fixed mobile layout
```

---

# Release Assets

A Release can provide downloadable files.

Examples:

```text
Application Package
Executable
ZIP
Binary
Installer
Documentation
```

Not every project needs release assets.

---

# Pre-Releases

A project may publish versions before they are considered stable.

Examples:

```text
v2.0.0-beta.1
v2.0.0-rc.1
```

Common concepts:

```text
alpha
beta
release candidate
stable
```

---

# Alpha

An alpha version is typically an early development version.

It may contain:

```text
Incomplete Features
Known Bugs
Unstable Behavior
```

It is generally not intended for normal production use.

---

# Beta

A beta version is more developed but may still contain bugs or changes.

Example:

```text
v2.0.0-beta.1
```

It can be used for testing and feedback.

---

# Release Candidate

A release candidate is a version that may become the final release if no significant problems are found.

Example:

```text
v2.0.0-rc.1
```

---

# Stable Release

A stable version is intended for normal use.

Example:

```text
v2.0.0
```

---

# Draft Release

A draft release allows release information to be prepared before publishing it publicly.

Typical workflow:

```text
Draft
 ↓
Review
 ↓
Publish
```

---

# Release Workflow

A common workflow:

```text
Feature Development
       ↓
Pull Requests
       ↓
Merge
       ↓
Testing
       ↓
Version Update
       ↓
Git Tag
       ↓
GitHub Release
       ↓
Release Notes
       ↓
Publish
```

---

# Example

Suppose your project currently has:

```text
v1.0.0
```

You add:

```text
Dark Mode
Search
```

No breaking changes.

New version:

```text
v1.1.0
```

Later you fix a bug:

```text
v1.1.1
```

Later you introduce a breaking API change:

```text
v2.0.0
```

---

# Release and Pull Requests

A release can summarize multiple Pull Requests.

Example:

```text
v1.2.0

PR #20
Add search

PR #21
Add filtering

PR #22
Improve documentation
```

Release notes can summarize the combined changes.

---

# Release and Issues

Issues can also be associated with a release.

Example:

```text
v1.2.0

#20 Search
#21 Filtering
#22 Documentation
```

This helps users and maintainers understand what was included.

---

# Changelog

A changelog records changes between project versions.

Example:

```text
CHANGELOG.md
```

Possible structure:

```md
# Changelog

## [1.1.0]

### Added

- Dark mode
- Search

### Fixed

- Login validation

## [1.0.0]

### Added

- Initial release
```

---

# Release Notes vs Changelog

Release Notes:

```text
Information about a particular release
```

Changelog:

```text
Historical record of project changes
```

They can overlap, but they serve slightly different purposes.

---

# Versioning Before 1.0

Some projects use versions such as:

```text
0.1.0
0.2.0
0.9.0
```

These generally indicate that the project has not reached its first stable major release.

Follow the project's own versioning policy.

---

# Versioning Rules

A simple SemVer-style model:

```text
Breaking Change
     ↓
MAJOR++

New Feature
     ↓
MINOR++

Bug Fix
     ↓
PATCH++
```

Examples:

```text
1.2.3
 ↓
1.2.4    Bug fix

1.2.4
 ↓
1.3.0    New feature

1.3.0
 ↓
2.0.0    Breaking change
```

---

# GitHub Release Example

Suppose your project has:

```text
Current:
v1.0.0
```

You completed:

```text
GitHub Actions documentation
Pull Request workflow
Open-source contribution guide
```

Create:

```text
v1.1.0
```

Release title:

```text
GitHub Mastery v1.1.0
```

Release notes:

```md
## Added

- GitHub Actions documentation
- Pull Request workflow
- Open-source contribution guide

## Improved

- Repository collaboration documentation
```

---

# Practice

For your GitHub Mastery repository, create your first release.

Before doing this:

```text
[ ] Repository documentation is complete enough
[ ] Changes are committed
[ ] main is up to date
[ ] No unwanted files
```

Then create a tag:

```bash
git switch main
git pull
git tag -a v1.0.0 -m "Initial GitHub Mastery release"
git push origin v1.0.0
```

Then create a GitHub Release using:

```text
Tag:
v1.0.0
```

Title:

```text
GitHub Mastery v1.0.0
```

---

# Release Notes Practice

Use:

```md
# GitHub Mastery v1.0.0

## Added

- GitHub Issues guide
- Pull Requests guide
- GitHub Projects guide
- GitHub Discussions guide
- GitHub Templates guide
- Open-source contribution guide
- Fork and upstream workflow
- Branching strategies guide
- Releases and versioning guide

## Purpose

This release contains the first organized version of the GitHub Mastery learning repository.
```

---

# Check Your Tag

Run:

```bash
git tag
```

You should see:

```text
v1.0.0
```

You can inspect the tag:

```bash
git show v1.0.0
```

---

# Future Releases

As your repository grows:

```text
v1.0.0
   ↓
v1.1.0
   ↓
v1.2.0
   ↓
v2.0.0
```

Each release should represent a meaningful state of the project.

---

# Common Mistakes

Avoid:

```text
Random version numbers
Creating releases without meaningful changes
Changing published version tags unnecessarily
Skipping release notes
Ignoring project versioning rules
```

Once a public version is released, treat that version identifier as stable.

---

# Professional Release Workflow

```
