# Chrono Quest: Time-Phase Manipulation Platformer

> A 2D Unity puzzle-platformer built around a custom time-phase state machine: world objects transition between discrete temporal states via player-driven energy expenditure. Includes A*-based enemy pathfinding with dynamic target switching and a production-style telemetry pipeline for playtesting analysis. Developed for **CSCI 526: Advanced Mobile Devices and Game Consoles**, USC.

[![Course](https://img.shields.io/badge/USC_CSCI%20526-9b1b1b?style=flat-square)](https://catalogue.usc.edu/preview_course_nopop.php?catoid=16&coid=250286)
[![Engine](https://img.shields.io/badge/Unity_2D-000000?style=flat-square&logo=unity&logoColor=white)](https://unity.com/features/2d)
[![Language](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=c-sharp&logoColor=white)](https://learn.microsoft.com/en-us/dotnet/csharp/)

---

## Overview

This project explores time manipulation as a core platforming mechanic rather than a special ability layered on top of conventional movement and combat. The player carries a gun that does not deal damage, instead, it shifts the temporal phase of objects in the world, aging or rewinding them between predefined states. A sapling can be grown into a tree to form a platform; a collapsing bridge can be rewound to an earlier, intact state. The same tool is used offensively against time-sensitive enemies and hazards, making world-manipulation and combat expressions of one unified system rather than separate mechanics.

The game is structured as a sequence of puzzle-platforming sections, each requiring the player to read the environment, identify which objects are time-manipulable, and sequence their interactions correctly, often under time or resource pressure. Enemies patrol and pursue using pathfinding-driven movement, and a multi-phase boss encounter caps the prototype's vertical slice.

A secondary goal of the project was treating playtesting as a data problem rather than a purely observational one: every shot, death, and section transition during a playtest is logged to a backend, giving the design team a quantitative basis for balancing decisions rather than relying solely on watching players in the room.

---

## Design Goals

- **One mechanic, many uses.** Time-shifting serves as movement tool, puzzle solver, and combat mechanic simultaneously, so player mastery compounds rather than fragments across unrelated systems.
- **Legible world state.** Every time-manipulable object needs to visually communicate its current phase and what phases are reachable, so puzzles are solvable through observation rather than trial and error.
- **Resource-constrained experimentation.** A finite, regenerating time resource means players can experiment with the mechanic, but can't brute-force every puzzle by spamming interactions.
- **Data-informed iteration.** Design decisions about difficulty, pacing, and section length were validated against actual playtest telemetry rather than intuition alone.

---

## Gameplay Systems

| System | Description |
| :--- | :--- |
| **Time-phase objects** | World objects with multiple discrete states (e.g., sapling → tree → fallen log), each with its own appearance and physical footprint, that the player ages forward or backward |
| **Time bank** | A capped, spendable resource the player draws from to perform time-shifts, preventing unlimited free manipulation |
| **Give / Take interaction** | A single targeting tool with two modes: advancing an object's phase or reversing it, used contextually depending on the object and the puzzle |
| **Rewind hazards** | A family of obstacles (sliding platforms, rotating blades, falling objects, projectiles) that reset to a prior state after a countdown once triggered, requiring the player to act within a window |
| **Player aging** | The player character itself has small/normal/old states affecting movement and access, mirroring the mechanic applied to the world back onto the player |
| **Enemy patrol & pursuit** | Enemies move along patrol routes and switch to chasing the player when within range, using pathfinding to navigate the level geometry |
| **Boss encounter** | A multi-phase boss fight that layers the core time mechanic against a more traditional combat structure with rotating defenses |

---

## Playtesting & Analytics

Rather than relying purely on in-person observation, the game ships with a lightweight telemetry pipeline that records gameplay events during a session, interactions with time objects, damage taken, deaths, and section completions and sends them to a backend database in real time.

This data was used to answer concrete design questions during development: which sections had disproportionately high death counts, whether players were using the "give" and "take" modes as intended, and where players spent the most time stuck. Balancing decisions, difficulty tuning, checkpoint placement, hint timing were made by reviewing this data across playtest sessions rather than guessing from a handful of in-room observations.

---

## Repository Structure

```
CSCI-526-GameProject/
│
├── Assets/
│   ├── Scripts/                  # All core gameplay logic
│   │   ├── Player & movement     # PlayerController, PlayerHealth, CameraController
│   │   ├── Time mechanic         # TimeObject, TimeBank, ShootMechanic
│   │   ├── Rewind hazards        # RewindObject and its variants
│   │   ├── Enemies & bosses      # EnemyAI, BossOne, TurtleEnemyController
│   │   ├── Level structure       # SectionManager, CheckPointManager
│   │   └── Analytics/            # Telemetry event models
│   │
│   ├── AstarPathfindingProject/  # Third-party pathfinding library
│   └── RestClient/               # Third-party REST client for telemetry
│
├── ProjectSettings/
└── README.md
```

---

## Tech Stack

| Layer | Technology |
| :--- | :--- |
| Engine | Unity (2D), C# |
| Pathfinding | A* Pathfinding Project |
| Networking | Proyecto26 RestClient |
| Backend | Firebase Realtime Database |
| Camera | Cinemachine |
| Deployment | WebGL, GitHub Pages |

---

## Installation

```bash
git clone https://github.com/<your-username>/chrono-quest-platformer.git
```

Open the project root in **Unity Editor** (2021.x or later recommended). Required third-party packages (A* Pathfinding Project, Proyecto26 RestClient, Newtonsoft.Json) are vendored under `Assets/`.

---

## My Contribution

Fork of a team project originally developed for **CSCI 526 - Advanced Mobile Devices and Game Consoles**, USC.

My contributions:
- **Time-phase mechanic**: design and implementation of the core time-object and time-bank systems
- **Level & section design**: building out playable sections and integrating them with the section-level analytics system
- **Playtesting & balancing**: analyzing telemetry data to identify friction points and inform iteration on level and enemy difficulty

Original repository: [dattpatel99/CSCI-526-GameProject](https://github.com/dattpatel99/CSCI-526-GameProject)

---

## References

- [A* Pathfinding Project](https://arongranberg.com/astar/) 
- [Proyecto26 RestClient](https://github.com/proyecto26/RestClient)
- [Unity Cinemachine](https://unity.com/unity/features/editor/art-and-design/cinemachine)
