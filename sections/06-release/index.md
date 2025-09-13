---
title: Release
has_children: false
nav_order: 7
---

# Release

- Which and how many artefacts are produced from your project's codebase?
- Onto which repositories (e.g. PyPI, Docker Hub, GitHub Packages, NPM etc.) are they released? Why?
- How are they released (e.g. manually, automatically, etc.)?
   + report the configuration steps and commands to run to release the artefacts

The Avian Blasters project codebase produces twi main types of artifacts:
- **sdist** (source distribution), which is the package containing the source code;
- **wheel** (binary distribution), which is the precompiled package ready for installation. 

In addition, the repository contains a `Dockerfile`, which defines the configuration needed to create a Docker image for a possible release on Docker Hub. 
The artifacts are primarily distributed on PypI, so any user can install the package using the command: `pip install Avian_Blasters`.
In addition to PyPI, the source code is available on GitHub, allowing developers to download the repository directly or integrate it as a dependency via Git. 

The release process is fully automated via GitHub Actions: each push to the `main` or `master` branch runs the `check.yaml` workflow, which starts the test suite, followed by the `deploy.yaml` workflow, which builds the packages and publishes them to PyPI via Twine.
Each release is associated with a Git tag in the format `X.Y.Z`. The tag, once pushed to GitHub, automatically triggers the release pipeline.

## Choice of the license

- Which license did you choose for your artefacts? Why?
- Which license did you choose for your code? Why?

## Choice of the versioning schema

- Which versioning schema (e.g. date-based versioning, SemVer, etc.) did you choose for your artefacts? Why?
   + how does the versioning schema work?

- In case of multiple artefacts, are the version numbers aligned or each artefact has its own versioning pace? Why?

- Describe when and how to create a new version of the artefacts in your project
   + e.g. when to increment the major, minor, and patch version numbers
   + e.g. how to create a new release branch
   + e.g. how to create a new tag
   + e.g. how to create a new release on GitHub

