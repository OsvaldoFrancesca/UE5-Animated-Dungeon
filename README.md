# 🏰 Animated Procedural Dungeon Generator (UE5)

**Engine:** Unreal Engine 5.3+
**Language:** Blueprints (Visual Scripting)
**Assets:** Quixel Bridge Megascans
**Focus:** Procedural Generation, Runtime Visualization, Async Loops.

## 📝 Overview
A grid-based procedural level generator. The algorithm goes beyond instant map creation by visualizing the build process in real-time (**"Runtime Visualization"**) using a Timer-based iteration system, simulating an "animated construction" effect.

![MapGenerator](https://github.com/user-attachments/assets/d6888461-2eeb-40a1-8aea-e9700fdd6fe5)

## 🌟 Key Features

* **Grid-Based Procedural Generation:** Dynamic creation of maps with variable dimensions (User-defined X and Y).
* **Animated Build Process:** Replaced standard synchronous loops with an asynchronous system (`Set Timer by Event`) to showcase block-by-block generation without blocking the Game Thread.
* **Organic Variation:** Applied random rotation (Yaw 0-360°) and adaptive scaling to every instance to avoid "grid repetition" artifacts.

### 🌍 Biome Logic & Assets
The system manages three distinct block types using high-fidelity assets (**Quixel Bridge**):

1.  ⛰️ **Mountains (Structural & Obstacles):**
    * **Mesh:** Custom composite actor formed by a cubic base and conical top (Cube + Cone) to stylize the environment.
    * **Texture:** Photorealistic materials from Quixel Bridge.
    * **Spawn Logic:** Mandatorily generated along the entire perimeter (Borders) to enclose the map, but also have a percentage chance to spawn internally as obstacles.
2.  🏖️ **Sand (Variation):**
    * **Spawn Logic:** Spawns randomly within the grid to break terrain monotony.
    * **Texture:** Photorealistic materials from Quixel Bridge.
3.  🌿 **Grass (Base):**
    * **Spawn Logic:** The default terrain filling all cells where mountains or sand are not present.
    * **Texture:** Photorealistic materials from Quixel Bridge.

## 🔧 Technical Details (Under the Hood)

### 1. Iteration Algorithm (The Engine)
Instead of a classic *Nested For Loop* (which would generate the level in a single invisible frame), I implemented a **Linear Timed Iterator**:
* Use of two counter variables (`CurrentX`, `CurrentY`).
* A custom event triggered by a Timer (0.05s delay) that positions a single actor and increments coordinates.
* Reset conditions (Row Completion) and timer invalidation (Map Completion).

### 2. Coordinate Calculation (Math)
Conversion from "Grid Index" to "World Coordinates":
WorldLocation.X = CurrentX * TileSize
WorldLocation.Y = CurrentY * TileSize
This ensures the system is scalable regardless of the mesh size used.

3. Decision Logic (Branching Logic)
Every cell passes through a series of conditional controls (Logic Gates):

Border Check: IF (X==0 OR Y==0 OR X==Width-1 OR Y==Height-1) -> Spawn Mountain (Border).

Internal Random Check:

IF (RandomFloat < 0.1) -> Spawn Mountain (Internal Obstacle).

ELSE IF (RandomFloat < 0.2) -> Spawn Sand.

ELSE -> Spawn Grass.

🛠 Tech Stack
Engine: Unreal Engine 5.3+

Scripting: Blueprints Visual Scripting

Assets: Quixel Megascans, Custom Composite Meshes.

Key Concepts: Arrays, Flow Control, Vector Math, Timers & Delegates, Procedural Generation.
