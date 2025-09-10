---
title: Requirements
has_children: false
nav_order: 3
---

# Requirements

## User stories

- Write down [user stories](https://www.atlassian.com/agile/project-management/user-stories) to devise the main __personas__ (user roles) and the activities they will perform via the system to be developed.

- As a gamer, the software should offer a certain level of challenge and/or variety, so that it feels rewarding to play.
- As a general user, the software must be simple to understand visually and easy to control, so that it is as pick-up and play as possible.
- ...

## Requirements analysis

- **Type of requirements**:
    - **Functional**:
        1. The user should be able to control a car able to move from left to right to shoot upward with the press of a button
        2. The user should be able to fend off against an army of avians comprised of at least 2 types, those being birds and bats. Each type of enemy features a unique movement and attack pattern:
            - Birds: move in predictable patterns (left-right sweeping, then descend - Space Invaders style) and occasionally attack the player car with projectiles. They come in waves, meaning that when all the birds on the screen are defeated, a new wave will appear
            - Bats: move left to right close to the car and try to home in on it and attack it with their sound waves to mess up the player’s movement
        3. The user should be able to obtain from time to time power-ups from defeated enemies, that allow the player character to gain temporary boosts/advantages, like temporary invincibility and laser
        4. The user should be able to understand easily the health of the player character and the enmies by just looking at the screen
        5. The game is endless in nature, so it should end when:
            - The car loses all its health points
            - A bird reaches the player ship
    - **Non-functional**:
        1. The game should feel smooth to play
        2. The game should try to imitate graphically games of the late 70's/early 80's
        3. The game might feature a high-score ladder
        4. The game might feature a difficulty select, to allow the players to choose the level of challenge
        5. The game featues music and sound-effects

- **Glossary**:
    - **Player/Car**: the character/vehicle controller by the final user during the game
    - **Enemy/Avians**: the characters trying to attack and defeat the player
    - **Projectile**: the attacks the players and the enemies can shoot to hit to each other
    - **Power-Up**: the items falling down from certain defeated enemies that when collected by the car allow it to gain new temporary abilities/advantages
    - **Bird**: the common type of enemy coming down from the top of the screen. Usually them come in groups/enemy waves
    - **Enemy Wave**: group of birds fighting against the player. When a enemy wave is defeated, a new one is generated
    - **Bat***: another type of enemy that tries to compromise the movement abilities of the player with a special type of projectile, known as wave
    - **Wave**: the bat's unique projectile that compromises the player character's movement abilities if the latter comes into contact with it

- **Acceptance Criteria for the requirements**
    - **F1**: the requirement is an acceptance criteria in and of itself. A 2-Player mode could be considered as a further goal
    - **F2**: the enemies must be of at least 2 types, those being the bird and the bat as desribed in the glossary. More enemy types could be considered.
    - **F3**: the game must feature at least 2 Power-Ups, those being the invincibility (render the player invulnerable for a few seconds) and the laser (change the shot type of player to make it easier to defeat the avians). More power-ups, like the double-shot/clone and/or a health replenisher might be implemented.
    - **F4**: either it can be seen by the colour of the characters or with some kind of bar. The former is more ideal, as it would reduce the visual clutter (should be changed as here a solution is implied)
    - **F5**: the requirement is an acceptance criteria in and of itself. If some kind of pause menu is implemented, then also through it the game could be interrupted
    - **NF1**: the game should feature a frequency of at least 30 FPS, better if reaches 60 FPS
    - **NF2**: if this can be achieved, it will be done. Otherwise, public domain or free-to-use resources might be used
    - **NF3**: if score memorisation will be implemented, this feature will be added
    - **NF4**: if the concept of difficulty is implemented, it will be featured
    - **NF5**: if achieved, this will likely done through the use of free-to-use resources


- The requirements must explain **what** (not how) the software being produced should do.
    - You should not focus on the particular problems, but exclusively on what you want the application to do.
- Requirements must be clearly identified, and possibly numbered.
- Requirements are divided into:
    - **Functional**: some functionality the software should provide to the user.
    - **Non-functional**: requirements that do not directly concern behavioral aspects, such as consistency, availability, security, efficiency, etc.
    - **Implementation**: constrain the entire phase of system realization, for instance by requiring the use of a specific programming language and/or a specific software dependency.
        - These constraints should be adequately justified by political / economic / administrative reasons (which must be written down)...
        - ...otherwise, implementation choices should emerge *as a consequence of* design (and therefore described in the design section).
- If there are domain-specific terms, these should be explained in a **glossary**.
- Each requirement must have its own **acceptance criteria**.
    - These will be important for the validation phase. 

> You may consider adding a use-case diagram here (via PlantUML) to better visualize the requirements and their relationships