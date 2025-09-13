---
title: Self-evaluation
has_children: false
nav_order: 12
---

# Self-evaluation

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

## Filippo Velli

Since the very start of the project work, I contributed and participated actively in all its phases, reporting to my colleagues as often as possible in order to address issues or possible miscommunications. This, alongside my initiative in proposing ideas and solutions, led me to become a sort of coordinator of the group.

When we divided the responsibilities each of us had to work on, I chose to mainly develop and implement **player**. On my end, the development itself started once I finalised a UML class diagram for the classes (interfaces and abstract classes) and all responsibilities linked to my part.

My workflow based itself mostly on Object Oriented Programming principles such as:
- responsibility delegation among classes
- DRY principle
- polymorphism and interfaces

**Player Status** and **Score** were the main classes linked to Player I worked on.
All the work on Player led me, on the Model side of the development, to work on **Entity** and **Area**, that are the cornerstones of the Model's implementations as all the elements of the games are entities, and the creation of **character**, that offers the base functionalities for both player and enemy. In the Model, I also worked on **World**, that is the way to access all entities present in the Model, and **Cooldown Handler**, that handles all cooldowns in the Model, delegating this responsibility from the other classes. Furthermore, I handled the integration of Player in the Controller, the **Sprite Manager**'s retooling, and the creation of both the **Menu** and the **Scoreboard**, which act outside of the game's MVC structure and work towards more non-functional aspects of the software. In order to handle both the Entity-Character-Player hierarchy, but also the Sprite Manager retooling, the `Strategy` design pattern was used. Besides the main development duties, I drew all the in-game sprites.

Throughout the project, I sought to stick closely to the requests and the characteristics decided together at the beginning of the project with the rest of the group and to the conventional commits, so to make the work for the team as a whole smooth and predictable as possible, still leaving some space for improvements and deviations whenever necessary.

During development, I encountered mainly 2 difficulties:

1) the handling of the Player movement was not smooth at the beginning. This led me to discover an issue with the movement delta, that was set inside of Entity and this addressed properly the issue. 

2) distinguishing properly between the 2 main refresh rates concepts (one in the View and one in the Model). At one point, they were one and the same, but this lead to undesired results. I decided then to separate the 2 concepts, as the Model is never impacted by the View's FPS, and it worked as intended.

Moreover, I think that the Controller could have benefitted from a further subdivision of responsibilities, instead of relying mainly on one very dense class in terms of code. In other words, more delegation at the Controller could have been better.

As the informal coordinator of the team, I did all I could to make sure every issue was addressed and/or signalled to the member of the group responsible, during our weekly team meeting or through channels we used to communicate to each other. This was in order to ensure that development could be smoothed out for the whole group, and the next goals for the following week were defined.
I gladly took the task to coordinate the group and overall it was a positive experience. However, it was not always easy: I had to keep others and myself accountable, proposing solutions to disagreements and contrasting views, and ensuring that the guidelines and parts we set for everyone were followed through.

All in all, this project work has provided me the chance to train and further develop my software design and development skills, my Git know-how, and to delve more into aspects like Continuous Integration and Continuous Delivery that, while encountered during lectures, I never addressed in the context of a concrete software project.

