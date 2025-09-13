---
title: Self-evaluation
has_children: false
nav_order: 12
---

# Self-evaluation

- An individual section is required for each member of the group
- Each member must self-evaluate their work, listing the strengths and weaknesses of the product
- Each member must describe their role within the group as objectively as possible
    + Each student is only responsible for their own section

## Beatrice Micolucci
At the beginning of the project, I actively took part in the requirements analysis and design phases, working closely with the rest of the team. During these phases we divided responsibilities, and I chose to focus on the implementation of the **game items** (**power-ups** and **projectiles**). Before moving on to coding, I refined the UML diagrams related to my components to ensure consistency with the overall architecture and the team’s design decisions.

During the implementation, I followed the design principles we had agreed on: I organized classes into interfaces and implementations, applied inheritance and polymorphism, and used design patterns such as the factory for object creation. I also introduced dedicated components to handle responsibilities properly, for example the `PowerUpHandler` for managing power-ups and their duration, and the `AttackHandler` for handling the creation of projectiles. In addition, I contributed to the menu and sound integration, completing parts of the game outside my main area.

I faced some challenges, especially when designing and coding the power-ups, such as handling the size of the laser power-up. I also ran into circular import issues, which I solved by using type annotations, even though I am aware this is not the most elegant solution. I believe the code could be further improved with a proper refactoring process.

From a teamwork perspective, I frequently collaborated with the other members of the group, giving and receiving feedback during both the design and implementation phases. I also contributed to writing tests for the components I developed, ensuring more reliable functionality.

Overall, I consider this project a highly valuable learning opportunity. It allowed me to improve my practical programming skills, deepen my understanding of design, and become more familiar with tools such as Git (branching, conventional commits, GitHub Actions). I think that this experience helped me grow not only technically, but also in terms of working as part of a structured software development team.

## Mariam Baloch

At the beginning of the project I contributed to the initial design discussions and helped bootstrap the game by setting up the core Pygame interface and rendering loop. As responsibilities were divided, I chose to focus on the enemy subsystem, covering the abstractions and concrete behaviors for Bird and Bat enemies, their movement patterns, collision handling, health, and attack logic. Before coding, I refined the design of these components to stay consistent with the overall layered/MVC architecture and the decisions shared within the team.

During implementation I followed the design principles we agreed on, organizing classes into clear interfaces and implementations and applying object-oriented techniques throughout. I used factories to create enemies and projectiles and introduced dedicated handlers where appropriate (e.g., for attacks and health) to separate concerns and enable testing. I integrated enemy–projectile interactions with the world update loop and controller, implemented wave management to support endless gameplay, and tuned visuals by coordinating sprite integration with the view layer. I also worked on gameplay balancing—health, speeds, and cooldowns—to ensure that enemy patterns felt distinct yet fair.

I faced several challenges and addressed them incrementally. Keeping two enemy behaviors coherent within a single update pipeline required careful separation of responsibilities, while balancing randomized attack cooldowns with deterministic tests led me to expose parameters and rely on factories to control variability. Making formation movement feel “Space-Invaders-like” without overcomplicating the code pushed me to extract direction state and boundary checks, and I resolved jitter by using accumulator-based movement. I also encountered occasional circular-import pressure in the enemy → projectile → attack-handler chain; I mitigated this by tightening dependencies and using interfaces, noting that further refactoring would make this cleaner.

From a collaboration perspective, I coordinated closely with teammates on controller updates, rendering needs, and scoring/power-up drops, and I incorporated feedback as the design evolved (e.g., unifying sprite APIs and refining endless wave respawn). I wrote and maintained tests around enemy health, attacks, movement, and factory behavior to increase confidence in the subsystem. Overall, I delivered the enemy system end-to-end—behaviors, attacks, collisions, and wave lifecycle—while helping to bring up the game loop and view. This has been a valuable learning experience: besides deepening my practical skills and architectural discipline, I strengthened my workflow with Git and CI and identified concrete areas for future improvement such as parameterization for difficulty scaling and expanded integration testing.

