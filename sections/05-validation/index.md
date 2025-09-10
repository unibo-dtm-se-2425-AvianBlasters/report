---
title: Validation
has_children: false
nav_order: 6
---

# Validation

## Testing approach

- Describe what approach you followed for testing your software
- If you followed TDD, describe it here
- Also mention which testing framework you used (e.g. `unittest` or `pytest`) and why

We adopted a TDD approach, initially writing basic tests to guide the implementation before developing the actual code. However, in some cases, tests were written immediately after implementation to validate newly introduced features.

We chose the `unittest` framework primarily for its simplicity and because it is part of the Python standard library, making it readily available without requiring external dependencies.

## Testing (automated)

> General recommendation: when discussing the tests below, please track to which requirement each test is related to.

For automated testing, a total of 86 tests were implemented, organized into separate files corresponding to each component. These tests validate the game's functionality and requirements, and are categorized as either unit tests or integration tests.

- ### test_entity.py
- ### test_world.py
- ### test_player.py
- ### test_enemy.py
- ### test_projectile.py
  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 13              | F1           | Correct creation of projectiles; movement and behavior; destruction; validation of parameters; player's shooting system (single/multiple shoot) |  Unit / Integration |
  
- ### test_power_up.py
  | Tests           | Requirements | Tested features    | Type  |
  |-----------------|------------  |--------------------|-------|
  | 20              | F3           | Correct creation of power-ups via `PowerUpFactory`; application and removal of power-up effects; management of power-up collection, timed power-ups, and power-ups that modify the player’s projectile type; validation of parameters. |  Unit / Integration |
- ### test_scoreboard.py

### Unit testing

- Describe the unit tests you developed, and their rationale
- Report success rate and test coverage here

As previously mentioned, a dedicated test file was created for each component to verify its functionality and methods.
This approach allowed each class and component to be tested in isolation, ensuring correct behavior and reliable operation.
All methods and properties were thoroughly validated, including edge cases involving incorrect inputs and invalid parameters.
The overall test success rate was 100%.


### Integration testing

- Describe couples of components that you tested together, and the corresponding test rationale/plan

- Report success rate and test coverage here

- If you used [test doubles](https://en.wikipedia.org/wiki/Test_double), describe her which type of double you used, and why

Integration testing was employed to verify the behavior and interactions between various components, including:

- **Power-up / Player**: testing the player's ability to collect, apply, and potentially remove power-up effects.
- **Power-up / Projectile**: verifying how power-ups influence the types of projectiles fired by the player.
- **Projectile / Player**: validating the player's firing behavior via the attack handler, based on the configured number and type of projectiles.
- (other cases)

These tests confirmed that the components interacted as expected. The overall test success rate was 100%.

To isolate components and simulate dependent behaviors during integration testing, various test doubles were used:

- **Player mock**: used for simulating access to handlers such as attack, health and status.
- **Projectile mock**: used to test power-ups that interact with existing projectiles.
- **Dummy power-up**: applied to test general logic without applying real effects.
- **Timer stub**: used for timed power-ups to simulate the passage of time and test the expiration of temporary effects without actual delays.



### System testing

- Describe the tests that you developed to automatically test the system as a whole
    + and the corresponding test rationale/plan
    + better would be to have system tests that match the acceptance criteria of the requirements

- Report success rate and test coverage here

- If you adopted containers (e.g. Docker compose) for testing, describe how you used them here
    + e.g. to run the system in a clean environment, or to run the tests in a clean environment

## Acceptance tests (manual)

- If you did any manual testing, describe it here
- Report the test rationale/plan so that another person can repeat the tests
    + better would be for acceptance tests to match the acceptance criteria of the requirements
- Report success rate here

