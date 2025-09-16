---
title: CI/CD
has_children: false
nav_order: 9
---

# CI/CD

The Avian Blasters project adopts a **CI/CD workflow** using **GitHub Actions**, aimed at automating key software development and release tasks. 
Each push or pull request to the repository triggers the automatic execution of unit and integration tests on multiple operating systems (Ubuntu, Windows, and macOS) and different Python versions (3.9, 3.10, and 3.11), thus ensuring code correctness and integrity regardless of the development environment. 

Automation reduces manual errors, identifies bugs as early as possible, and enables faster and more stable releases.

Package distribution on TestPyPI is managed via GitHub Actions: developers create semantic tags on master branches, and if all tests pass, the workflow automatically generates the package release. The process leverages repository secrets such as `PYPI_USERNAME`, `PYPI_PASSWORD`, and `RELEASE_TOKEN`, as well as environment variables, to ensure security and protect credentials. The `check.yml` and `deploy.yml` configuration files define the test and release pipelines respectively, including the execution strategy on multiple OSes and Python versions, dependency rollback, and test execution via `unittest`.
