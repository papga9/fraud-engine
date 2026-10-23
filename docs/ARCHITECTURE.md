# Engine Architecture

## 1. The Core Loop
The engine operates on a fixed-timestep simulation loop decoupled from the rendering framerate.
*   `Engine::Initialize()`
*   `Engine::Run()` -> `ProcessInput()`, `Update(dt)`, `Render()`
*   `Engine::Shutdown()`

## 2. Subsystem Boundaries
Subsystems communicate via explicit interfaces, not tight coupling. 
*   **Platform Layer:** Handles OS window creation, raw input events, and file I/O.
*   **Renderer:** A pure data consumer. It takes a list of render commands/primitives and submits them to the GPU. It does not know about game logic.
*   **Entity/Scene System:** [e.g., ECS (Entity Component System) or a traditional Scene Graph].

## 3. Directory Structure Rules
*   `src/core/` - Memory, math, and foundational utilities.
*   `src/platform/` - OS-specific implementations.
*   `src/renderer/` - Graphics API abstractions.
*   `docs/` - Living documentation and ADRs.
