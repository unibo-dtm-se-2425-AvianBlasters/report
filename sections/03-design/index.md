---
title: Design
has_children: false
nav_order: 4
---

# Design

This chapter explains the strategies used to meet the requirements identified in the analysis. 

Ideally, the design should be the same, regardless of the technological choices made during the implementation phase.

> You can re-order the sections as you prefer, but all the sections must be present in the end

## Architecture 

- Which architectural style (e.g. layered, object-based, event-based, shared dataspace)? Why? Why not the others?
- Provide details about the actual architecture (e.g. N-tier, hexagonal, etc.) you are going to adopt. Motivate your choice.
- Provide a high-level overview of the architecture, possibly with a diagram
- Describe the responsibilities of each architectural component

> UML Components diagrams are welcome here


**Architectural Style**

We adopt a layered architecture with object-oriented design. This choice provides:

    - Separation of concerns: clear boundaries between presentation, game logic, and data/persistence.

    - Maintainability and testability: each layer has focused responsibilities and can be tested in isolation.

    - Evolutionary flexibility: UI, rules, and storage can evolve independently with stable interfaces.

    - Pedagogical clarity: it demonstrates fundamental software engineering principles familiar to most developers.

Why not other styles?

- Event-based: It would add unnecessary complexity for a single-player game with straightforward game loop.

- Shared dataspace: It is not suitable for a game requiring real-time updates and state management

- Microservices: It is overkill for a single-player desktop application

**Detailed Architecture: N-Tier with MVC Pattern**

The project adopts a 3-tier architecture(Presentation, Business Logic, Data) with  the Model-View-Controller (MVC) pattern. 

1. **Presentation Tier (View Layer)**

    - GameView (→ GameViewImpl): 

    primary rendering of world and UI.
    Key methods: renderWorld(), renderEntity(entity), updateDisplay().
    Depends on: SpriteManager, UIRenderer.

    - SpriteManager: 

    Handles sprite loading, animation frames and draw calls.
    - SpriteManagerPlayer: Player sprite rendering and animation
    - SpriteManagerEnemy: Enemy sprite rendering (Bird/Bat variants)
    - SpriteManagerProjectile: Projectile sprite rendering
    - SpriteManagerPowerUp: Power-up sprite rendering
    - DefaultSpriteManager: Fallback sprite rendering

    - UIRenderer (→ UIRendererImpl): 

    Manages HUD elements (score, health, game over screen)
    Key Methods: render_score(), render_health(), render_game_over()

2. **Business Logic Tier (Controller Layer)**
    
    - GameController (→ GameControllerImpl): 
    
    Orchestrates game flow and coordinates between layers.
    Key Methods: run(), update_game_state(), handle_input()

    - InputHandler (→ InputHandlerImpl): 
    
    maps raw input to game commands
    Key Methods: handle_events(), process_input()
    Supports: Keyboard input (movement, shooting, pause, quit)

    - SoundManager (→ SoundManagerImpl): 
    
    sound effects and music playback. 
    Key Methods: play_sound_effect(), play_music()
        

3. **Data Tier (Model Layer)** 

    - World (→ WorldImpl): 
    
    Manages game entities and state
    Key Methods: get_*(), add_*(), remove_*()
    Manages: All game entities (players, enemies, projectiles, power-ups)

    - Entities: 
    
    Game objects (Player, Enemy, Projectile, PowerUp)
    Player: Player character with health, score, power-ups
    Enemy: Base enemy class with Bird and Bat implementations
    Projectile: Player and enemy projectiles with different types
    PowerUp: Temporary power-ups (laser, invulnerability, etc.)

    - Scoreboard (→ ScoreboardImpl): 
    
    Handles persistent data
    Key Methods: add_score(), get_scores()
    Storage: File-based (assets/scoreboard.txt)

**Motivation for 3-tier + MVC**

It aligns with the real-time game loop while preserving a clean separation between rendering, control/orchestration, and state. It keeps dependencies pointing inward (UI → controller → model), supports unit testing of controllers/models without renderer, and allows future substitution (e.g., different renderer or storage) with minimal impact. 


## Infrastructure (mostly applies to distributed systems)

- Are there **infrastructural components** that need to be introduced? Which and **how many** of each?
    - e.g. **clients**, **servers**, **load balancers**, **caches**, **databases**, **message brokers**, **queues**, **workers**, **proxies**, **firewalls**, **CDNs**, etc.
- How do components **distribute** over the network? **Where** are they located?
    - e.g. do servers / brokers / databases / etc. sit on the same machine? on the same network? on the same datacenter? on the same continent?
- How do components **find** each other?
    - How to **name** components?
    - e.g. **DNS**, **service discovery**, **load balancing**, etc.

> UML deployment diagrams are welcome here

## Modelling

### Domain driven design (DDD) modelling

- Which are the bounded contexts of your domain? 
- Which are domain concepts (entities, value objects, aggregates, etc.) for each context?
- Are there repositories, services, or factories for each/any domain concept?
- What are the relavant domain events in each context?

> Context map diagrams are welcome here

### Object-oriented modelling

- What are the main data types (e.g. classes) of the system?
- What are the main attributes and methods of each data type?
- How do data types relate to each other?

> UML class diagrams are welcome here

### In case of a distributed system

- How do the domain concepts map to the architectural or infrastuctural components?
    + i.e. which architectural/component is responsible for which domain concept?
    + are there data types which are required onto multiple components? (e.g. messages being exchanged between components)

- What are the domain concepts or data types which represent the state of the distributed system?
    + e.g. state of a video game on central server, while inputs/representations on clients
    + e.g. where to store messages in an instant-messaging app? for how long?

- Are there domain concepts or data types which represent messages being exchanged between components?
    + e.g. messages between clients and servers, messages between servers, messages between clients

## Interaction

- How do components *communicate*? *When*? *What*?

- Which **interaction patterns** do they enact?

> UML sequence diagrams are welcome here

## Behaviour

- How does **each** component *behave* individually (e.g., in *response* to *events* or messages)?
    + Some components may be *stateful*, others *stateless*

- Which components are in charge of updating the **state** of the system? *When*? *How*?

> UML state diagrams or activity diagrams are welcome here

## Data-related aspects (in case persistent storage is needed)

- Is there any data that needs to be stored?
    - *What* data? *Where*? *Why*?

- How should **persistent data** be **stored**? Why?
    - e.g., relations, documents, key-value, graph, etc.

- Which components perform queries on the database?
    - *When*? *Which* queries? *Why*?
    - Concurrent read? Concurrent write? Why?

- Is there any data that needs to be shared between components?
    - *Why*? *What* data?

