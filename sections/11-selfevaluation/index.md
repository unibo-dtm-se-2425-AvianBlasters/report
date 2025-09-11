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
