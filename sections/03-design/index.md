---
title: Design
has_children: false
nav_order: 4
---

# Design

## Architecture 

### Architectural Style

We adopt a layered architecture with object-oriented design. This choice provides:

- Separation of concerns: clear boundaries between presentation, game logic, and data/persistence.

- Maintainability and testability: each layer has focused responsibilities and can be tested in isolation.

- Evolutionary flexibility: UI, rules, and storage can evolve independently with stable interfaces.

Why not other styles?

- Event-based: It would add unnecessary complexity for a single-player game with straightforward game loop.

- Shared dataspace: It is not suitable for a game requiring real-time updates and state management

- Microservices: It is overkill for a single-player desktop application

### Detailed Architecture: N-Tier with MVC Pattern

The architecture is based on a 3-tier model (Presentation, Business Logic, and Data) with the Model-View-Controller (MVC) pattern implemented at the boundary between the Presentation and Business Logic tiers. This design ensures a clear separation of concerns, decoupling rendering (View) from orchestration (Controller) and state management (Model). This inward-pointing dependency structure (UI → Controller → Model) enables robust unit testing of controllers and models without needing a renderer and facilitates the easy substitution of components like different renderers or storage systems with minimal impact. The architecture aligns seamlessly with a real-time game loop while maintaining its clean, modular structure.

![Layered Architecture](../../pictures/ComponentDiagram.png)

### Responsibilities of architectural components

- GameViewImpl / SpriteManager* — compose and render frames; manage sprites/animations; draw HUD via UIRendererImpl.

- GameControllerImpl — main loop, input coordination, state updates, audio triggers; InputHandlerImpl maps device events to commands.

- WorldImpl — authoritative game state and entity lifecycle (Player, Enemies—Bird/Bat, Projectiles, PowerUps); ScoreboardImpl persists scores (local file).

> See Modelling → Object-oriented modelling for class details; see Modelling → DDD for bounded contexts and domain events.

## Infrastructure (mostly applies to distributed systems)

### Infrastructural Components

Avian Blasters is a standalone, single-player desktop application that runs entirely on the user's machine in a single Python process. There are no network protocols or external services involved; persistent data is limited to the local scoreboard file. 

### Component Distribution (Network and Placement)

- All components (presentation, game logic, persistence) execute on the same host (desktop/laptop).
- Single process, in-memory collaboration via direct method calls/objects.
-No inter-machine distribution; no servers, brokers or databases deployed remotely.
-Local filesystem only; scoreboard persisted to a text file (e.g., assets/scoreboard.txt).

### Component Discovery

Since all components exist within the same process and memory space, they communicate through direct method calls and object references. No service discovery, DNS, or load balancing is required. 

### Naming of Components

Standard Python module and class naming conventions are used. 

- Modules: snake_case (e.g., game_controller_impl.py, sprite_manager_enemy.py)

- Classes: PascalCase (e.g., GameControllerImpl, SpriteManagerEnemy)

- Methods: snake_case (e.g., update_game_state(), render_world())

- Files/Assets: relative paths within the project (e.g., assets/scoreboard.txt, sprite atlases).

![Deployment Architecture](../../pictures/DeploymentArchitecture.png)

## Modelling

### Domain driven design (DDD) modelling

**Bounded Contexts**

We partition the domain into three bounded contexts:

**1. Gameplay Context (Core Domain)** : rules, mechanics, entities, collisions, waves.

**2. Presentation Context (Supporting Subdomain)** : rendering, HUD, animations, view models.

**3. Persistence/Scoring Context (Generic Subdomain)** : durable high-scores and related queries.

**Context relationships (high level):**

- Gameplay → Presentation: Gameplay publishes domain events that the Presentation consumes to render the current state.

- Gameplay → Persistence: Gameplay emits score-related events; Persistence records and queries them.

- Presentation → Gameplay: User input is translated into commands directed at Gameplay.

**Domain Concepts per Context**

**1. Gameplay Context (Core Domain)**

**Aggregate roots and aggregates**

- World (aggregate root): authoritative state and lifecycle of in-world objects. 
Invariants: exactly one Player; entities must be registered through the world; collisions are resolved at most once per tick; projectiles have owners and TTL.

**Entities (inside World)**

- Player : health, position, score buffer, active powerups.

- Enemy : base type with variants (Bird, Bat);Health, position, movement state, attack cooldown

- Projectile: owner (player/enemy), type, damage, direction, TTL

- PowerUp: type, duration/effect.

- Wave: formation pattern, remainig enemies, spawn cadence. 

**Value Objects**

- Position (x,y), Velocity, Direction; Bounding Box/ Area (Collision)
- Health (current,max); Damage; Score (non-persistent, in-run points)
- Types (EnemyType, ProjectileType, PowerUpType); TimeStep dt.

**Domain Services**
- Collision Service: Collision detection and resolution across aggregates.
- Spawn Service: Schedles/mints enemies and waves according to rules.
- Scoring Services: Converts in-run events to score deltas (pre-persistence).

**Factories**
- EnemyFactory, ProjectileFactory, PowerUpFactory,WaveFactory: create valid instances honoring invariants.

**Repositories**
- WorldRepository (in-memory): snapshot/load transient world state (useful for testing, pausing).

**Domain Events**
- GameStarted, ProjectileFired, EnemySpawned, EnemyDefeated
- PlayerDamaged, PlayerDied, PowerUpCollected, WaveCompleted, GameOver.

**2. Presentation Context (Supporting)**

It transform domain state/events into frames, HUD, and feedback without altering rules.

**Aggregates/Entities**

- RenderScene (aggregate root): currect frame graph (layers, camera, UI).
- SpriteManager* : sprite/animation resources per entity family.
- UIOverlay/HUD: health, score, game over, pause.

**Value Objects**

- Spriteld, Frame, Animation, Viewport, Color, Dimensions, TextLabel. 

**Services**

- RenderingService: composes scene graph from domain read models.
- AnimationService: advances sprite timelines.

**Repositories**

- SpriteRepository: file based asset catalogue.

**Domain Events**

- EntityAppeared/Updated/Removed, HealthChanged,ScoreUpdated, GameStateChanged.

**3. Persistence/Scoring Context (Generic)**

**Aggregate roots and aggregates**

- Scoreboard (aggregate root): collection of ScoreEntry.

**Entities**

- ScoreEntry: (PlayerName, points, difficulty, timestamp).

**Value Objects**

-ScorePoints, PlayerName, Difficulty, RecordedAt.

**Services**

- ScoreboardService; Manages score persistence

**Repositories**

- ScoreboardRepository : file-backed storage (e.g., assets/scoreboard.txt).

**Domain Events (handled/emitted)**

- ScoreAdded: When a new high score is recorded.
- ScoreboardLoaded:  When scores are retrieved from storage.

**Context Map**

**Upstream/Downstream and Translation Patterns**

- Gameplay is upstream to both Presentation and persistence (it owns the domain language and publishes events).
- Presentation is downstream conformist: it adopts Gameplay's published language and translates events into view models.
- Persistence is downstream via an open-host/published language: it listens to score-related events and persists them.
- Presentation → Gameplay uses an ACL (anti-corruption layer) for input: UI terms (keys, buttons) are translated into commands (Moveleft, shoot, pause). 

![Context Map](../../pictures/Context_Diagram_DDD.png)

### Object-oriented modelling

This section provides the system's principal classes, their responsibilities, salient attributes/methods, and the relationships among them. The modelling aligns with the layered (Presentation/ Business Logic/ Data) + MVC architecture descibed in the previous sections. 

**Main data types (classes)**

**Core domain (gameplay)**

- Entity (Abstract Base) / EntityImpl (Concrete Implementation)
  Responsibility: minimal game object contract (identity, spatial footprint, lifecycle).
  Key attributes: _area , _type, _active
  Key methods: get_area(), get_type (), move(dx,dy,width,height), destroy(), is_active()

- Character (Abstract Base) / CharacterImpl (Concrete Implementation)
  Responsibility: movable, damageable actors with attacks.
  Key attributes: _health_handler, _attack_handler , _speed, _position , _area 
  Key methods: get_health(), take_damag(amount), is_dead(), shoot(), move()

- Player (Abstract Base) / PlayerImpl (Concrete Implementation)
  Responsibility: user-controlled character.
  Key attributes: _score , _status_handler , _power_up_handler , _limit_left , _limit_right 
  Key methods: move(direction), shoot(),is_touched(other),ger_score() 

- Enemy (Abstract Base) / EnemyImpl (Concrete Implementation), Bird, Bat
  Responsibility: opponents with variant behaviors.
  Representative attributes:
  EnemyImpl: Inherits from Character and Enemy + _laser_damage_timer;
  Bird: _formation_direction, _horizontal_accumulator, _vertical_accumulator;
  Bat: _movement_state, _player_y, _horizontal_direction.
  Key methods: shoot(), move(), set_player_position (y).

- Item (Abstract Base) / ItemImpl (Concrete Implementation)
  Responsibility: base for non-actor interactables (projectiles, power-ups).

- Projectile (Abstract Base) / ProjectImpl (Concrete Implementation)
  Responsibility: time-limited damaging entities.
  Key attributes: _projectile_type, _damage
  Key methods: get_projectile_type(), get_damage(), move()

- PowerUp (Abstract Base) / PowerUpImpl (Concrete Implementation)
  Responsibility: temporary effects applied to the player.
  Key attributes: _power_up_type, _duration.
  Key methods: get_power_up_type(), apply(player), remove(player).

- HealthHandler (Abstract Base) / HealthHandlerImpl (Concrete Implementation)
  Responsibility: encapsulate health arithmetic and invariants.
  Key attributes: _max_health, _current_health.
  Key methods: take_damage(amount), heal(amount), is_dead(), get_current_health().

- AttackHandler (Abstract Base) / GeneralAttackHandlerImpl (Concrete Implementation), EnemyAttackHandler, PlayerAttackHandler
  Responsibility: fire-rate/cooldown logic and projectile creation.
  Key attributes: _cooldown_handler, _fire_chance.
  Key methods: try_attack(), update(dt), set_player_position(y).

- World (Abstract Base) / WorldImpl (Concrete Implementation)
  Responsibility: aggregate root for in-game state and entity lifecycle.
  Key attributes: _entities.
  Key methods: get_all_entities(), add_entities(*entities), get_players(), get_enemies(), remove_entity(entity).

**Application / presentation support**

- GameController (abstract) / GameControllerImpl
  Responsibility: game loop orchestration and coordination among layers.
  Key attributes: _world, _view, _input_handler, _player.
  Key methods: initialize(), run(), update_game_state(dt), handle_input().

- InputHandler (→ InputHandlerImpl)
  Responsibility: translate device input into domain commands.
  Key methods: handle_events(), process_input().

- SoundManager (→ SoundManagerImpl)
  Responsibility: SFX/music playback.
  Key methods: play_sound_effect(id), play_music(track).

- SpriteManager (abstract) / AbstractSpriteManager with specializations
  Responsibility: sprite/animation loading and draw calls for entity families.

- UIRenderer (→ UIRendererImpl)
  Responsibility: HUD and overlay rendering (score, health, game-over).
  Key methods: render_score(), render_health(), render_game_over().

**Persistence**

- Scoreboard (→ ScoreboardImpl)
  Responsibility: durable high-score collection.
  Key methods: add_score(player, value), get_scores(); file-backed (assets/scoreboard.txt).

**Relationships among data types**

**- Inheritance**

- Character is-a Entity; Player is-a Character; Enemy is-a Character; Bird/Bat are Enemy.
- Item is a base for Projectile and PowerUp (with concrete implementations).

**- Composition**

- Character has-a HealthHandler and AttackHandler.
- Player has-a StatusHandler, PowerUpHandler, and Score.
- World has collections of Entity (players, enemies, projectiles, power-ups).

**- Association / Dependency**

- GameController → World, InputHandler, UIRenderer, SoundManager, Scoreboard.
- SpriteManager uses Entity state to render; UIRenderer reads world/score state.
- EnemyAttackHandler and PlayerAttackHandler create Projectile instances (via factories).

**- Factories (creational)**

- EnemyFactory → Bird, Bat; ProjectileFactory → Projectile (e.g., SoundwaveProjectile); PowerUpFactory → PowerUp.

![Class Diagram](../../pictures/ClassDiagram.png)

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

This section explains how components communicate, when, and what they exchange, and the interaction patterns they enact.

### Component Interaction

This section details the nature of communication and data exchange among the system's components, which is critical for the game's operational integrity.

**Communication Modality**

Components interact via synchronous Python method calls and object references within a single process. The View reads the Model’s state in a read-only fashion; the Controller orchestrates updates and coordinates subsystems (rendering, audio, persistence). No network messages are used.

**Timing and Frequency of Interaction**

Component interactions are triggered by specific events and occur at predetermined intervals to maintain the real-time nature of the game:

- Game loop tick: The primary communication occurs once per frame during the game loop tick, targeting a rate of 60 frames per second.

- Event-Driven Communication: Interactions are initiated immediately in response to new **user input events** and during significant **state transitions**, such as object collisions, entity destruction, or the activation/deactivation of power-up effects.

- Lifecycle Events: Communication is also vital during the application's initialization and shutdown phases to facilitate proper setup and teardown of components.

**Exchanged Data**

The data exchanged between components can be categorized into several distinct types, reflecting their purpose and origin:

- Control Signals: These include commands and actions derived from user input, such as instructions to move, shoot, or toggle game states (e.g., pause/quit).

- State Updates: Information representing changes in the game world, including positions, health values, spawns, despawns, and management of timers and cooldowns.

- Presentation Data: Information specifically formatted for rendering, such as entity lists, derived view information, and values for the Head-Up Display (HUD).

- System Cues: This category includes audio cues (e.g., for firing or impact sounds) and persistence data related to score submissions and queries to the local scoreboard.

### Interaction Patterns

The system employs several established interaction patterns to manage communication flow and maintain a clean separation of concerns.

- Model-View-Controller (MVC): This core pattern decouples rendering (View) from game state (Model) and input handling (Controller). The flow is User InputHandlerImpl → GameControllerImpl → WorldImpl → GameViewImpl → Display.

- Observer Pattern: This pattern is used to notify observers (e.g., the View) of state changes in the subject (Model). A GameController detects an entity's state change, and the GameView receives an update notification to render the new visual state.

- Factory Pattern: Used to create new entity instances without exposing the creation logic to the rest of the application. For example, the GameController requests new entities from a ProjectileFactory, which then creates and returns them to the World. EnemyFactory, ProjectileFactory, PowerUpFactory are invoked by the controller or attack handlers to mint domain objects.

- Command Pattern: This pattern translates user actions into command objects that are executed by a handler. An InputHandler captures a user action, which is then processed by the GameController to create a command for a Player to execute.

### Detailed Interaction Sequences

- Game Loop Interaction: The main loop orchestrates continuous updates. GameController.run() iteratively calls InputHandler.handle_events(), GameController.update_game_state(), and GameView.render_world() at 60 FPS.

![Main game-loop tick](../../pictures/MainGameloop.png)

- Player Shooting: A user's key press is captured by the InputHandler, processed by the GameController, and results in a Player.shoot() method call. This creates a projectile instance via a factory, which is then added to the World and rendered by the GameView.

![Player Shooting](../../pictures/Playershooting.png)

- Enemy hit → damage, death, score update: A projectile's movement triggers a collision check against an enemy. Upon collision, the Enemy.take_damage() method is called, and if the enemy is destroyed (is_dead()), it is removed from the World, and the player's score is updated.

![Enemy hit](../../pictures/Enemyhit.png)

![Power-up collection (apply effect, HUD/audio feedback)](../../pictures/PowerSeq.png)

## Behaviour

This section describes how each component behaves in response to inputs and events (stateful vs. stateless), and which components update system state, when, and how.

### Individual Component Behaviour

The system's components are designed with distinct roles, categorized by their statefulness.

**Stateful Components**

These components retain information over time and manage a defined internal state. 

- GameControllerImpl: owns the game loop and flow flags (running, paused) and references to the World, Player, View, and I/O subsystems. Responds to normalized actions (move, shoot, pause/quit); advances simulation each frame; triggers rendering/audio; transitions between Running ⇄ Paused and into Game Over. 

- WorldImpl: authoritative container of runtime entities. Adds/removes entities, advances them each tick, resolves collisions, and applies results (damage, destruction, spawns).

- PlayerImpl: maintains position, health, score, status/power-up timers. Moves within limits; fires via attack handler; applies power-ups; enters Invulnerable briefly after damage.

- Enemy (Bird/Bat): keeps movement/attack state and cooldowns.
Bird follows formation sweeps and periodic descent.
Bat switches from Descending to Bird-like/Homing based on proximity to the player.

- Handlers: HealthHandler tracks (current,max) and death; AttackHandler/cooldowns decide when a projectile is created; power-up/status handlers manage effect durations.

**Stateless Components**

These components perform specific actions without retaining any data between calls. 

The InputHandler captures and translates user input into game actions. The SpriteManager provides sprite data on demand for rendering, and the SoundManager triggers audio effects.

### State Update Responsibilities

State updates are managed through a clear hierarchy of responsibilities.

- Primary State Managers: The GameController and World are the main state coordinators. The GameController triggers updates every frame (60 FPS) and manages game flow. The World handles the state of all entities, their interactions, and their lifecycles. Individual entities like the Player and Enemy are responsible for updating their own specific states in response to events or continuous updates.

- Secondary State Managers: Specialized components such as the PowerUpHandler, ScoreHandler, and HealthHandler manage specific, localized state changes. The PowerUpHandler applies temporary effects, while the ScoreHandler and HealthHandler manage score and health values, respectively, in response to events like enemy destruction or damage.

### State Transition Diagrams

**One frame of the game loop**

![Controller-Orchestrated](../../pictures/FrameActivity.png)


**Game Controller State Machine**

![Game State Machine](../../pictures/GameStateMach.png)

**Player State Machine**

![Player State Machine](../../pictures/Playerstate.png)

**Bat Movement State Machine**

![Bat Movement State Machine](../../pictures/BatMov.png)

**Enemy Lifecycle State Machine**

![Enemy Lifecycle State Machine](../../pictures/EnemyLifecycle.png)

**Overall Game State Machine**

![Game State Machine](../../pictures/OverallGameState.png)

### State Persistence and Synchronization

State is handled entirely within the local environment, eliminating the complexity of a distributed system.

- State Persistence: The majority of the game's state (entity positions, health, score) is in-memory and exists only during runtime. The only persistent data is the scoreboard, which is stored in a local text file (assets/scoreboard.txt).

- State Synchronization: As a single-player, single-process application, there is no distributed state. This design choice removes the need for state synchronization, preventing issues related to consistency, race conditions, or network latency. All state consistency is handled internally by the responsible components.

## Data-related aspects (in case persistent storage is needed)

This section describes the system's data management strategy, including what data is stored, how it's persisted, and how it's shared between components.

**Persistent Data and Storage Strategy**

The game requires the storage of high scores for its scoreboard, game configuration for settings, and asset metadata for resources. These are stored locally, with the scoreboard being a simple text file (assets/scoreboard.txt). 

**Storage Type**: **Text File (Key-Value Pairs)**
- **Format**: Simple text file with structured data
- **Location**: `assets/scoreboard.txt`
- **Structure**: 
  ```
  PlayerName,Score,Difficulty,Timestamp
  Player1,1500,Easy,2024-01-15
  Player2,1200,Hard,2024-01-15
  ```

The decision to use a file-based approach instead of a formal database was driven by the need for simplicity, portability, and low overhead, which is appropriate for a small, single-player game. This approach avoids the complexity and cost of managing an external database. Runtime game state, such as entity positions and health, is not stored and is managed in-memory, resetting with each game session.

**Alternative Storage Approaches (Not Used)**

- Relational Database (SQLite): It was not used as it is overkill for simple scoreboard data but used well for Complex user management, multiple game modes, analytics.

- Document Database (JSON): We did not use it as it is more complex than needed for current requirements
but it can be used in complex game state, user profiles, achievements.

- Key-Value Store (Redis): It requires external service, adds complexity. Best used in real-time multiplayer, session management

**Data Queries**

Data queries are handled by the ScoreboardImpl component, which performs file-based read operations to load existing scores and write operations to save new ones. These queries are made at specific moments, such as at game initialization and game over, and do not require concurrent access due to the single-player nature of the application.

**Query Patterns**

Queries:
  - `get_scores()` → Read all scores from file
  - `add_score()` → Append new score to file

Read Operations:
```python
def get_scores(self):
    # Read entire file, parse scores
    with open('assets/scoreboard.txt', 'r') as f:
        return parse_scores(f.read())
```

Write Operations:
```python
def add_score(self, score_data):
    # Append new score to file
    with open('assets/scoreboard.txt', 'a') as f:
        f.write(format_score(score_data))
```

**Data Sharing**

Data is shared between components through established patterns to maintain consistency. Key information like game state, configuration, and asset data is centralized and managed by specific components (World, Manager classes) and then accessed by other components via direct object references. This ensures a consistent view of the data without the need for complex synchronization mechanisms.

**Persistence interactions**

- Save score at Game Over

![Save Score](../../pictures/savescore.png)

- Load and display top scores (MenuMenu → High Scores)

![Load and display score](../../pictures/loadscore.png)



