# GitHub Projects

## Introduction

GitHub Projects is used to organize and track work across Issues and Pull Requests.

It helps answer:

```text
What needs to be done?
What is currently being worked on?
What is completed?
What should be done next?
```

A simple workflow is:

```text
Ideas
  ↓
Todo
  ↓
In Progress
  ↓
Review
  ↓
Done
```

---

# Project vs Repository

A repository contains the actual code and files.

A Project is used to organize work.

```text
Repository
   ↓
Code
Issues
Pull Requests

Project
   ↓
Planning
Tracking
Priorities
```

A Project can help organize work across one or multiple repositories.

---

# Creating a Project

Typical process:

```text
GitHub
 ↓
Projects
 ↓
New Project
 ↓
Choose View
 ↓
Create
```

You can then add Issues and Pull Requests.

---

# Project Views

GitHub Projects can represent work using different views.

Common approaches include:

```text
Table
Board
Roadmap
```

Each view presents the same underlying work in a different way.

---

# Table View

A table is useful for detailed tracking.

Example:

| Task | Status | Priority |
|---|---|---|
| Login | Todo | High |
| Dashboard | In Progress | Medium |
| Documentation | Done | Low |

This is useful when you need to see many properties at once.

---

# Board View

A board visually represents work by status.

Example:

```text
TODO          IN PROGRESS       REVIEW          DONE
────          ───────────       ──────          ────

Issue #1      Issue #4           Issue #7        Issue #2
Issue #3      Issue #5                           Issue #6
```

Cards can be moved between columns as work progresses.

---

# Roadmap View

A roadmap is useful for planning work over time.

Conceptually:

```text
August        September       October

Feature A ────────────────┐
                          ↓
Feature B        ────────────────┐
                                 ↓
Feature C                 ────────────────
```

It provides a time-oriented view of planned work.


---

# Status

A Project can use statuses such as:

```text
Todo
In Progress
Review
Done
```

The exact statuses depend on how the Project is configured.

---

# Issue + Project

An Issue can be added to a Project.

Example:

```text
Issue #15
Fix login validation
        ↓
GitHub Project
        ↓
Todo
```

When development begins:

```text
Todo
 ↓
In Progress
```

After completion:

```text
In Progress
 ↓
Review
 ↓
Done
```

---

# Pull Request + Project

Pull Requests can also be tracked.

Example:

```text
Issue
 ↓
Pull Request
 ↓
Project
 ↓
Review
```

This lets the team see both planned work and implementation progress.

---

# Project Fields

Projects can use fields to organize information.

Examples:

```text
Status
Priority
Assignee
Labels
Iteration
Date
```

Example:

| Task | Status | Priority | Assignee |
|---|---|---|---|
| API | In Progress | High | Developer |
| UI | Todo | Medium | Developer |
| Tests | Done | High | Developer |

---

# Priority

A Priority field can help determine what should be handled first.

Example:

```text
High
Medium
Low
```

Conceptually:

```text
High Priority
     ↓
Do First

Medium
     ↓
Do Next

Low
     ↓
Later
```

---

# Assignee

Assign work to the person responsible for completing it.

Example:

```text
Issue #20
Add authentication

Assignee:
Developer A
```

This makes ownership clear.

---

# Labels

Labels provide another way to categorize work.

Examples:

```text
bug
feature
documentation
security
enhancement
```

A Project can use these labels to organize and filter items.

---

# Filtering

Projects can be filtered to show specific work.

For example:

```text
Status = In Progress
```

or:

```text
Priority = High
```

This can make a large Project easier to manage.

---

# Sorting

Items can be sorted based on fields such as:

```text
Priority
Status
Date
Assignee
```

For example:

```text
High
High
Medium
Low
```

This helps prioritize work.

---

# Project Workflow

A simple development workflow:

```text
New Idea
   ↓
Create Issue
   ↓
Add to Project
   ↓
Todo
   ↓
Create Branch
   ↓
Development
   ↓
Pull Request
   ↓
Review
   ↓
Merge
   ↓
Done
```

---

# Example Project

Suppose you are building an AI web application.

Create Issues:

```text
#1 Create project structure
#2 Build login system
#3 Create dashboard
#4 Add API integration
#5 Add testing
#6 Write documentation
```

Project board:

```text
TODO
────────────────
#1 Create structure
#2 Login
#3 Dashboard

IN PROGRESS
────────────────
#4 API integration

REVIEW
────────────────
#5 Testing

DONE
────────────────
#6 Documentation
```

---

# Moving Work

When you start Issue #2:

```text
TODO
 ↓
IN PROGRESS
```

When the Pull Request is ready:

```text
IN PROGRESS
 ↓
REVIEW
```

After merging:

```text
REVIEW
 ↓
DONE
```

---

# Project Automation

GitHub Projects can be connected with repository activity.

For example, work can be organized around:

```text
Issues
Pull Requests
Status
Labels
```

Automation reduces the amount of manual project maintenance.

---

# Project Templates

Templates can help create Projects with a predefined structure.

For example:

```text
Software Development
```

might use:

```text
Todo
In Progress
Review
Done
```

This gives a project a useful starting point.

---

# Iterations

Iterations can divide work into time periods.

For example:

```text
Iteration 1
Week 1

Iteration 2
Week 2

Iteration 3
Week 3
```

This can be useful for planning work in short cycles.

---

# Sprint Concept

A sprint is a fixed period in which a team aims to complete a selected group of work.

Example:

```text
Sprint 1
────────────
Login
Dashboard
Testing
```

After the sprint:

```text
Completed
Remaining
New Work
```

Projects can help visualize this work.

---

# GitHub Projects + Agile

GitHub Projects can support Agile-style workflows.

Typical process:

```text
Backlog
   ↓
Planning
   ↓
Sprint
   ↓
Development
   ↓
Review
   ↓
Done
```

---

# Backlog

The backlog contains work that has not yet been completed.

Example:

```text
Backlog
────────────

Feature A
Feature B
Bug C
Documentation D
Feature E
```

The team can prioritize items before moving them into active work.

---

# Kanban

A Kanban-style board represents work through stages.

Example:

```text
BACKLOG
   ↓
TODO
   ↓
IN PROGRESS
   ↓
REVIEW
   ↓
DONE
```

The visual board makes the current state of work easy to understand.

---

# Work in Progress

Work in Progress is commonly abbreviated as:

```text
WIP
```

A team can limit the amount of simultaneous work.

Example:

```text
IN PROGRESS

Task A
Task B
```

Instead of starting ten tasks at once, the team focuses on completing existing work.

---

# Why WIP Limits Help

Too much simultaneous work can cause:

```text
Context Switching
Slow Progress
More Bugs
Unfinished Tasks
```

A focused workflow can be:

```text
Start
 ↓
Finish
 ↓
Start Next
```

---

# Milestones vs Projects

Milestone:

```text
Groups Issues around a goal
```

Project:

```text
Provides broader work organization and planning
```

They can be used together.

Example:

```text
Project
   ↓
Version 1.0
   ↓
Milestone
   ↓
Issues
```

---

# Issues + Milestones + Projects

These concepts can work together:

```text
Project
   ↓
Milestone
   ↓
Issues
   ↓
Pull Requests
   ↓
Commits
```

Each serves a different purpose.

---

# Example Software Project

Suppose the goal is:

```text
Build Portfolio Website
```

Project:

```text
Portfolio Website
```

Issues:

```text
Create homepage
Add projects section
Add contact form
Add responsive design
Add SEO
```

Board:

```text
TODO
   ↓
IN PROGRESS
   ↓
REVIEW
   ↓
DONE
```

---

# Project Best Practices

- Keep tasks specific.
- Use clear statuses.
- Prioritize important work.
- Avoid too much work in progress.
- Link Issues to Pull Requests.
- Keep completed work updated.
- Remove unnecessary tasks.
- Review the Project regularly.

---

# Practice

Create a GitHub Project for your learning repository.

Name:

```text
GitHub Mastery
```

Create these statuses:

```text
Todo
In Progress
Review
Done
```

Add your existing Issues.

For example:

```text
Add project documentation
Add automated testing
Improve GitHub Actions workflow
```

Place them initially in:

```text
Todo
```

---

# Practice Workflow

Take:

```text
Add project documentation
```

Move it:

```text
Todo
 ↓
In Progress
```

Create your branch:

```bash
git switch -c docs/project-documentation
```

Make the changes.

Create the Pull Request.

Move the task to:

```text
Review
```

After merging:

```text
Done
```

---

# Challenge

Create at least five Issues:

```text
1. Improve README
2. Add GitHub Actions documentation
3. Add security documentation
4. Add Pull Request documentation
5. Add GitHub Projects documentation
```

Then organize them:

```text
              GitHub Mastery

TODO          IN PROGRESS      REVIEW       DONE
────          ───────────      ──────       ────
Issue 1       Issue 2          Issue 3      Issue 4
Issue 5
```

Move each item as you actually work on it.

---

# Real-World Collaboration System

A professional GitHub workflow can look like:

```text
             PROJECT
                ↓
             BACKLOG
                ↓
              ISSUE
                ↓
             BRANCH
                ↓
             COMMIT
                ↓
              PUSH
                ↓
         PULL REQUEST
                ↓
         AUTOMATED CI
                ↓
             REVIEW
                ↓
              MERGE
                ↓
              DONE
```

This connects GitHub's development and project-management features.

---

# Interview Questions

### What is GitHub Projects?

A GitHub feature for planning and tracking work using Issues, Pull Requests, and customizable project fields and views.

### What is a Project board?

A visual representation of work organized into stages such as Todo, In Progress, Review, and Done.

### What is WIP?

Work in Progress, meaning work that has started but has not yet been completed.

### What is a backlog?

A collection of work that may need to be completed in the future.

### What is Kanban?

A workflow-management approach that visualizes work through stages.

### What is the difference between an Issue and a Project?

An Issue represents a specific task or problem, while a Project organizes and tracks a broader collection of work.

---

# Summary

GitHub Projects turns individual Issues and Pull Requests into an organized workflow.

Remember:

```text
Project
   ↓
Backlog
   ↓
Issue
   ↓
Todo
   ↓
In Progress
   ↓
Pull Request
   ↓
Review
   ↓
Merge
   ↓
Done
```

The goal is simple:

```text
Know what needs to be done
        ↓
Know who is doing it
        ↓
Know what is currently happening
        ↓
Know what is finished
```

That is the foundation of GitHub project management.
