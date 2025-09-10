---
title: Development
has_children: false
nav_order: 5
---

# Development

A primarily test-driven methodology was adopted for the development of Avian Blasters. After an initial analysis of the project's requirements and design, the development phase involved writing tests for the game's main components (such as the player, enemies, projectiles, and power-ups) before proceeding with implementation. When necessary, testing and implementation were combined to continuously verify the code's correct functioning and ensure greater reliability and software quality.

## DVCS

The project uses Git with a structured workflow, organized into different branches following branching conventions, with each branch serving a specific purpose:

- a `master` branch representing the stable version.
- a `dev` branch containing code under development.
- several `feature/...` branches created for different parts of the game: player, enemy, projectile and power-up features.

For commits, we followed the conventional commit style: `(type)[optional context]: short description`.

The project uses GitHub Actions to automatically run tests on each push and manage deployment, enabling automatic release generation on GitHub. 

Pull requests and issues were not used during development.

## Implementation details

- Which network protocols should be used? Why?
    - Examples: UDP, TCP, HTTP, WebSockets, gRPC, XMPP, AMQP, MQTT, etc.

- How should in-transit data be represented? Why?
    - Examples: JSON, XML, YAML, Protocol Buffers, etc.

- How should databases be queried? Why?
    - Examples: SQL, NoSQL, etc.

- How should components be authenticated? Why?
    - Examples: OAuth, JWT, etc.

- How should components be authorized? Why?
    - Examples: RBAC, ABAC, etc.
  
The implemented game is standalone, meaning it doesn't require an internet connection to be used. Consequently, no network protocols were adopted, since the game runs entirely locally.

There is no need to manage in-transit data or use a database: scores are simply saved locally in a text file (`scoreboard.txt`). Before starting the game, the user can optionally enter their name, which is then associated with the score achieved.

There are no authentication or authorization mechanisms, as the game is single-player and doesn't require the management of multiple users or different privilege levels.

## Technological details

- Which programming languages, frameworks, libraries, and tools were used? Why?
    - Examples: Java, Python, C++, JavaScript, React, Angular, Vue, etc.

- Which libraries do the project depend on? Why?
    - Examples: React, Redux, Express, etc.

- Are there any other external technology or service dependencies? Why?
    - Examples: Google Maps, Firebase, etc.



**Programming Languages**

We used the primary language as Python version 3.12.1. It because of its simplicity, readability, and extensive library ecosystem for game development. Its ideal for rapid prototyping and educational projects. 

**Core Game Development Framework**

We have used Pygame 2.6.1 for our framework because pygame is the primary framework for 2D game development in python. It provides the following:

- Graphics rendering and sprite management
- Input Handling (keyboard, mouse)
- Sound and music playbooko
- Cross-platform compatibility

**UI Framework**
We used Pygame Menu 4.5.2 because it provides pre-built menu components for the game's main menu, settings and scoreboard, renducing development time for UI elements.

**GUI Toolkit**
We used TKinter for GUI toolkit which is used for screen dimension detection in the menu system (menu_imp.py) TKinter is python's builtin GUI toolkit, providing, access to system information like screen resolution. 

