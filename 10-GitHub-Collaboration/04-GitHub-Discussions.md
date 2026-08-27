
Commands
Troubleshooting
```

---

# Marking an Answer

For question-based Discussions, a helpful response can be marked as the answer.

Conceptually:

```text
Question
   ↓
Multiple Responses
   ↓
Best Answer
   ↓
Answered
```

This makes useful information easier to find later.

---

# Ideas

Discussions can be used to propose ideas before turning them into actual Issues.

Example:

```text
Discussion:

Should the project support dark mode?
```

People discuss:

```text
Advantages
Disadvantages
Implementation
User Feedback
```

If the idea is accepted:

```text
Discussion
   ↓
Decision
   ↓
Issue
   ↓
Development
```

---

# Polls

Polls can help gather community opinions.

Example:

```text
Which feature should we build next?

A. Dark Mode
B. Notifications
C. Search
D. Export
```

The community can vote.

---

# Announcements

Announcements can communicate important information.

Examples:

```text
New Release
Major Update
Breaking Change
Community Event
Maintenance
```

---

# Discussion Comments

Discussion comments can be used for:

```text
Questions
Suggestions
Explanations
Feedback
Technical Opinions
```

Keep discussions focused and respectful.

---

# Discussion Workflow

A typical workflow:

```text
Question / Idea
       ↓
Discussion
       ↓
Community Feedback
       ↓
Decision
       ↓
Issue
       ↓
Development
```

Not every Discussion needs to become an Issue.

---

# Discussion → Issue

Suppose someone proposes:

```text
Add dark mode
```

The community agrees.

Convert the idea into actionable work:

```text
Discussion
    ↓
Feature accepted
    ↓
Issue created
    ↓
Branch
    ↓
Pull Request
    ↓
Merge
```

---

# Discussion → Documentation

Some questions are asked repeatedly.

Example:

```text
How do I install the project?
```

Instead of answering the same question repeatedly:

```text
Discussion
   ↓
Good Answer
   ↓
Documentation
   ↓
README / Wiki
```

This improves the project over time.

---

# Community Knowledge

Discussions can become a knowledge base.

Example:

```text
Question
 ↓
Answer
 ↓
Community Feedback
 ↓
Final Solution
```

Future contributors can search the previous conversation instead of asking the same question.

---

# Good Discussion Titles

Good:

```text
Should we support Python 3.13?
```

Good:

```text
Ideas for improving the project documentation
```

Bad:

```text
Question
```

Bad:

```text
Help
```

The title should describe the topic.

---

# Good Discussion Questions

Instead of:

```text
How does this work?
```

Use:

```text
How should I configure the application for local development?
```

Specific questions produce better answers.

---

# Discussion Best Practices

- Use the correct category.
- Write descriptive titles.
- Search before creating a duplicate discussion.
- Provide enough context.
- Keep conversations focused.
- Mark useful answers.
- Convert actionable ideas into Issues.
- Turn repeated solutions into documentation.

---

# Moderation

Repository maintainers can manage community conversations.

Good community management includes:

```text
Removing spam
Handling duplicate discussions
Keeping conversations relevant
Maintaining respectful communication
Directing actionable work to Issues
```

---

# Example Open Source Workflow

Imagine an open-source project.

A contributor asks:

```text
Discussion:
Should we add dark mode?
```

Community:

```text
👍 Yes
👍 Yes
👍 Yes
```

Maintainer decides:

```text
Accepted
```

Then:

```text
Create Issue
      ↓
#25 Add dark mode
      ↓
Contributor creates branch
      ↓
Pull Request
      ↓
Review
      ↓
Merge
```

---

# Discussions + Issues + Projects

These features can work together:

```text
Discussion
    ↓
Idea Accepted
    ↓
Issue
    ↓
Project
    ↓
Todo
    ↓
In Progress
    ↓
Pull Request
    ↓
Review
    ↓
Done
```

This is a complete planning-to-development workflow.

---

# Discussions vs Pull Requests

Pull Request:

```text
Proposed code changes
```

Discussion:

```text
Conversation
```

Example:

```text
"Should we add Redis?"
        ↓
Discussion

"Add Redis caching"
        ↓
Issue

"Implementation of Redis caching"
        ↓
Pull Request
```

---

# Discussions vs README

README:

```text
Official project information
```

Discussion:

```text
Community conversation
```

A Discussion might eventually lead to README improvements.

---

# Practice

If Discussions are enabled on your repository, create a discussion.

Category:

```text
General
```

Title:

```text
What should I learn after GitHub Actions?
```

Description:

```md
## Question

I have completed the GitHub fundamentals and GitHub Actions learning section.

I want to continue improving my GitHub skills and eventually contribute to open-source projects.

## Current Learning

- Git
- GitHub
- Branches
- Commits
- Pull Requests
- Issues
- Projects
- GitHub Actions

## Question

What GitHub skill should I focus on next?
```

Then answer your own Discussion with a short summary of what you learned.

---

# Challenge

Create another Discussion:

Title:

```text
Ideas for improving my GitHub Mastery repository
```

Description:

```md
## Goal

I want to improve this repository as a long-term learning resource.

## Ideas

- Better documentation
- More Git examples
- GitHub Actions projects
- Open-source contribution guides
- Advanced Git workflows
- Useful GitHub templates

## Feedback

Suggestions are welcome.
```

---

# Important Concept

Remember the difference:

```text
Discussion
   ↓
Conversation

Issue
   ↓
Actionable Task

Project
   ↓
Work Management

Pull Request
   ↓
Code Change

Commit
   ↓
Saved Change
```

---

# Complete GitHub Collaboration Model

```text
                 DISCUSSION
                     ↓
                  IDEA
                     ↓
                   ISSUE
                     ↓
                  PROJECT
                     ↓
                  BRANCH
                     ↓
                  COMMIT
                     ↓
                   PUSH
                     ↓
              PULL REQUEST
                     ↓
                 CI CHECKS
                     ↓
                  REVIEW
                     ↓
                   MERGE
                     ↓
                   DONE
```

---

# Interview Questions

### What are GitHub Discussions?

A GitHub feature for community conversations, questions, ideas, and other non-actionable discussions.

### When should you use an Issue?

When something needs to be tracked and acted upon, such as a bug, feature, or task.

### When should you use a Discussion?

When you want to ask a question, explore an idea, or have a community conversation.

### Can a Discussion lead to an Issue?

Yes. A useful idea can be discussed first and then converted into actionable work.

### Why are Discussions useful for open source?

They provide a centralized place for community knowledge, questions, feedback, and ideas.

---

# Summary

Remember:

```text
Discussion
→ Talk about it

Issue
→ Track it

Project
→ Organize it

Branch
→ Work on it

Commit
→ Save it

Pull Request
→ Review it

Merge
→ Integrate it
```

Together, these features create a structured GitHub collaboration workflow.
