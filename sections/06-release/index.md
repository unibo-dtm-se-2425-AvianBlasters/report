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

The Avian Blasters project codebase produces two main types of artifacts:
- Source distribution (`.tar.gz`), which is the package containing the source code and installation scripts;
- Binary distribution (`.whl`), which is the precompiled package ready for installation. 

In addition, each GitHub release automatically provides two extra archives of the source code (`.zip` and `.tar.gz`), generated directly by GitHub.

The repository also contains a `Dockerfile`, which defines the configuration needed to create a Docker image for a possible release on Docker Hub. 

The artifacts are distributed on TestPyPI, so any user can install the package using the command: `pip install -i https://test.pypi.org/simple/ Avian-Blasters`.
They are also published under the **Releases** section of GitHub, so the project can be downloaded or cloned directly.

The release process is fully automated via GitHub Actions: each push to the `main` or `master` branch runs the `check.yml` workflow, which starts the test suite, followed by the `deploy.yml` workflow, which builds the packages and publishes them to TestPyPI.
Each release is associated with a Git tag in the format `X.Y.Z`. The tag, once pushed to GitHub, automatically triggers the release pipeline.

## Choice of the license

- Which license did you choose for your artefacts? Why?
- Which license did you choose for your code? Why?

Both the artifacts and the source code of the project are released under the **Apache 2.0 license**. Choosing this permissive license allows broad usage and redistribution of the package, even under different licenses, while maintaining compatibility with third-party libraries like pygame. 

The license also provides important protections, including **trademark protection**, which safeguards the project's identity, a **patent grant**, which ensures legal safety when using the software, and a **notice requirement**, which ensures all contributors are credited.

## Choice of the versioning schema

- Which versioning schema (e.g. date-based versioning, SemVer, etc.) did you choose for your artefacts? Why?
   + how does the versioning schema work?

- In case of multiple artefacts, are the version numbers aligned or each artefact has its own versioning pace? Why?

- Describe when and how to create a new version of the artefacts in your project
   + e.g. when to increment the major, minor, and patch version numbers
   + e.g. how to create a new release branch
   + e.g. how to create a new tag
   + e.g. how to create a new release on GitHub
  
The project uses **Semantic Versioning** (SemVer), a widely recognized standard that clearly communicates to users and developers the type of changes introduced in a new version of the package.
Versions follow the `Major.Minor.Patch` format:
- **Major** is incremented when a breaking change is introduced;
- **Minor** is incremented when new features are added without breaking API compatibility;
- **Patch** is incremented when bugs are fixed while maintaining API compatibility.

All produced artifacts share the same version number, as they represent the same code in different formats, ensuring consistency across distributions.

To release a new version, follow these steps:
1) If needed, create a dedicated branch for the release, for example ` git checkout -b feature/new-feature`;
2) Run tests and merge the changes into the `master` or `main` branch;
3) Create a Git tag with the appropriate version number, e.g., `git tag -a X.Y.Z -m "description"`;
4) Push the tag to GitHub with `git push --follow-tags`;
5) The automated pipeline will handle packaging and releasing the new version.

