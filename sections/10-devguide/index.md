---
title: Developer guide
has_children: false
nav_order: 11
---

# Developer Guide

This section explains how can a new person enter the development team and start contributing to the project.
- report any relevant organizational information (e.g. how to contact the team, how to report issues, etc.)
- report any internal convention that the team follows (e.g. naming conventions, coding style, etc.)
- report any relevant information about the development environment (e.g. how to set it up, how to run the tests, etc.)
- report any relevant information about the development workflow (e.g. how to create a new feature branch, how to create a pull request, etc.)
- report any relevant information about the development tools (e.g. how to configure the IDE, how to use the command line, etc.)

## Organizational Information
New contributors can contact the team through GitHub Discussions or by opening GitHub Issues to report bugs, request features, or suggest improvements. Private communication via the institutional email is also possible.
For internal team communication, a group chat is typically used.

## Internal Conventions
Naming conventions:

- Classes: `PascalCase`
- Variables and functions: `snake_case`
- Constants: `UPPERCASE`

The team follows conventional commits to keep the commit history clean and organized, which also facilitates automated release workflows.
For each new feature, contributors are generally expected to add tests to the project’s test suite.

## Development Environment
The project uses Python `3.12.1`.
To contribute:

1. Clone the repository.
   `git clone https://github.com/unibo-dtm-se-2425-AvianBlasters/artifact.git`
2. Create an isolated virtual environment: 
``` 
python -m venv venv 
venv\Scripts\activate  
source venv/bin/activate
```
3. Install dependencies (`pip install -r requirements-dev.txt`).
4. Implement changes.
5. Run tests with:
`python -m unittest discover -s test -t .`

Tests are automatically executed on every push and pull request through GitHub Actions.

## Development Workflow
The `master` branch is reserved for official releases, `dev` contains stable in-progress code, and `feature/--` branches are used for new features.
To work on a new feature:
1. Create a new branch from dev (`git checkout -b feature/feature_name`).
2. Develop following the team’s coding and commit conventions.
3. Push the branch and open a pull request.
A team member will review and approve the merge.

## Development tools
- Recommended IDE: **Visual Studio Code**
- GitHub Actions: the `check.yml` (testing) and `deploy.yml` (release) workflows are pre-configured. Contributors only need to ensure all tests pass before opening a pull request.
