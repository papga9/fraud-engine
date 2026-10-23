# Custom C++20 Vulkan Engine

A first-principles, data-oriented 3D game engine built from scratch. This project is designed as an **Editor-First** environment, prioritizing explicit hardware control, deterministic memory management, and cache-friendly simulation architecture.

## Core Architecture
*   **Editor-First:** The engine runs as an editor (via Dear ImGui). Game scenes are data payloads loaded into the ECS, allowing live manipulation of shaders, transforms, and entities.
*   **Data-Oriented ECS:** Logic and data are strictly separated. Components are packed into custom sparse sets (contiguous memory arrays) to maximize CPU cache hits.
*   **Explicit Rendering:** Powered by Vulkan. The graphics API is treated as a pure data consumer. An extraction system sanitizes ECS data into flat Render Packets, completely decoupling the simulation from the GPU command buffers.

## Tech Stack
*   **Language:** C++20
*   **Graphics API:** Vulkan (SPIR-V Shaders)
*   **Window/Input:** GLFW
*   **Editor UI:** Dear ImGui
*   **Build System:** CMake
*   **Tooling:** Python 3 (for pre-build C++ reflection generation)
*   **Asset Pipeline:** glTF 2.0 strictly

## Directory Structure
*   `src/core/` - Memory allocators, custom math headers, and ECS sparse set logic.
*   `src/platform/` - OS windowing, input polling, and file I/O abstraction.
*   `src/renderer/` - Vulkan instance setup, pipeline compilation, and command buffer submission.
*   `src/editor/` - ImGui context, scene hierarchy panels, and live component inspectors.
*   `docs/` - Living documentation, architectural guidelines, and decision logs (ADRs).
*   `tools/` - Python scripts for header parsing and reflection boilerplate generation.
*   `assets/` - glTF models, textures, and compiled SPIR-V shaders.

## Build Instructions (Linux / Windows)

### Prerequisites
*   CMake 3.20+
*   A C++20 compatible compiler (GCC 11+, Clang 13+, or MSVC 19.29+)
*   Vulkan SDK
*   Python 3.x

### Compiling
```bash
git clone [repository-url]
cd [repository-name]
mkdir build && cd build
cmake ..
cmake --build .
