---
title: Deployment
has_children: false
nav_order: 8
---

# Deployment

## User installation

- Does the user need to install something on their machine(s) to run your software?
    * If yes, what is it?
    * If yes, how to install it? (report the commands to run)
    * If yes, how to configure it? (e.g. creating configuration files, setting environment variables, etc.)

To install and run Avian Blasters on their machine, the user only needs Python 3.9 or higher and standard management tools (`pip`). No additional configuration or environment variables are required.

The game can be obtained in two ways:

1. **Installation from TestPyPI**
   
   Using the command:

    ```
    pip install -i https://test.pypi.org/simple/ Avian-Blasters=='version_number'
    ```

    All the necessary dependencies are handled automatically.

    *Note: Currently, the installation process via TestPyPI does not work with all Python versions due to a required package (meson-python) that is not updated on TestPyPI. A future official release on PyPI is planned, which will allow proper installation on all supported Python versions.*

2. **Installation from GitHub**
   
   By cloning the repository or downloading the archive from the Release section. The commands are:
   
    ```
    git clone https://github.com/unibo-dtm-se-2425-AvianBlasters/artifact.git
    cd artifact
    pip install -r requirements.txt
    ```
After installation, the game can be launched with: `python -m Avian_Blasters`.
This opens a fixed-size game window with keyboard-controlled inputs.

The user can verify a successful installation if:
- the `pip install ...` command installs all dependencies without errors;
- the game launches correctly with `python -m Avian_Blasters`;
- the game window responds to inputs and displays graphics without crashes.

## Server-side installation

No server-side installation is required.