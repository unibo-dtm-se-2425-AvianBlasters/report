---
title: Validation
has_children: false
nav_order: 6
---

# Validation

## Testing approach

We adopted a TDD approach, initially writing basic tests to guide the implementation before developing the actual code. However, in some cases, tests were written immediately after implementation to validate newly introduced features.

We chose the `unittest` framework primarily for its simplicity and because it is part of the Python standard library, making it readily available without requiring external dependencies.

## Testing (automated)

For automated testing, a total of 87 tests were implemented, organized into separate files corresponding to each component. These tests validate the game's functionality and requirements, and are categorized as either unit tests or integration tests.

- ### test_entity.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 5               | F1           | Correct creation of Entities; changes to the delta; interaction between different Entities thorugh area overlap; Entity movement |  Unit / Integration |

- ### test_world.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 2               | F1           | Correct initialisation of the World; new Entities being added to the World correctly |  Unit / Integration |

- ### test_player.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 20              | F1, F3, F4, F5           | Correct initialisation of the Player, PlayerStatusHandler and Score; Player movement and its edge cases; Score variation; damage and end game Conditions; attack capabilities; changes of the Player's status (normal, invulnerable, slowed) |  Unit / Integration |

- ### test_enemy.py

  | Tests | Requirements | Tested features | Type |
  |-------|--------------|-----------------|------|
  | 24 | F1, F2, F5 | Enemy health handling (init, damage, heal, death); generic and specialized attack handlers (Enemy, Bird, Bat) including cooldown and firing patterns; core enemy behavior (state, movement, damage) and attack delegation; Bird formation movement and direction switching; bat descending and homing behavior with sound wave attacks; enemy formation factory (3×10 birds, health/position sanity) | Unit / Integration |

- ### test_projectile.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 13              | F1           | Correct creation of projectiles; movement and behavior; destruction; validation of parameters; player's shooting system (single/multiple shoot) |  Unit / Integration |
  
- ### test_power_up.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 20              | F3           | Correct creation of power-ups via `PowerUpFactory`; application and removal of power-up effects; management of power-up collection, timed power-ups, and power-ups that modify the player’s projectile type; validation of parameters. |  Unit / Integration |

- ### test_scoreboard.py

  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 3               | NF3           | Gathering of the scores from the txt file containing the Scoreboard; handling of wrong requests for the getter; addition of new scores to the scoreboard |  Unit |

### Unit testing

As previously mentioned, a dedicated test file was created for each component to verify its functionality and methods.
This approach allowed each class and component to be tested in isolation, ensuring correct behavior and reliable operation.
All methods and properties were thoroughly validated, including edge cases involving incorrect inputs and invalid parameters.

The overall test success rate was 100%.

### Integration testing

Integration testing was employed to verify the behavior and interactions between various components, including:

- **Power-up / Player**: testing the player's ability to collect, apply, and potentially remove power-up effects.
- **Power-up / Projectile**: verifying how power-ups influence the types of projectiles fired by the player.
- **Projectile / Player**: validating the player's firing behavior via the attack handler, based on the configured number and type of projectiles.
- **Entity / Area**: testing the movement capabilities and interactions of Entity objects using Area.
- **Player / Entity**: testing the player's interactions with other entities. 
- **Player / PlayerStatus**: testing handling of status changes during the normal functionality of player.
- **PlayerStatus / CooldownHandler**: testing the status cooldowns.
- **World / Entity**: verifying the correct addition of new elements being all of the Entity class (or one of its subclasses).

These tests confirmed that the components interacted as expected. The overall test success rate was 100%.

To isolate components and simulate dependent behaviors during integration testing, various test doubles were used:

- **Player mock**: used for simulating access to handlers such as attack, health and status.
- **Projectile mock**: used to test power-ups that interact with existing projectiles.
- **Dummy power-up**: applied to test general logic without applying real effects.
- **Timer mock (patch)**: used for timed power-ups to simulate the passage of time and test the expiration of temporary effects without actual delays.

### System testing

- Describe the tests that you developed to automatically test the system as a whole
    + and the corresponding test rationale/plan
    + better would be to have system tests that match the acceptance criteria of the requirements

- Report success rate and test coverage here

- If you adopted containers (e.g. Docker compose) for testing, describe how you used them here
    + e.g. to run the system in a clean environment, or to run the tests in a clean environment

## Acceptance tests (manual)

All functional and non-functional requirements have been successfully verified through manual testing of the game. Manual testing was done by:

- verifying the setup and startup of the application through the commands specified in the Deployment section
- manually trying the software, to test variables setup (changing music on/off, different fps levels and/or difficulty, name being set-up or not) originating from the menu and overall gameplay, as described in the User Guide

The following acceptance criteria were specifically addressed:

- **F1**: The player can move the car left and right and shoot upward.
- **F2**: The game features at least two enemy types—birds and bats—with distinct attack modes and movement patterns.
- **F3**: The game includes at least two power-ups (invulnerability and laser), as well as two additional ones (health recovery and double shoot).
- **F4**: The player's health status is visually represented through the character's color.
- **F5**: The game ends when the player is defeated.
- **NF1**: The game maintains a stable frame rate of 60 FPS by default, with the option to adjust the frame rate via the settings menu.
- **NF2**: Graphic assets follow a visual style inspired by the 1970s/1980s.
- **NF3**: The score is displayed in the top-left corner of the screen.
- **NF4**: Game difficulty can be selected from the main menu.
- **NF5**: All sounds and music are created using free or public domain resources.

The overall test success rate for acceptance tests was 100%.