# Engine Project Overview

## Vision
A custom 3D game engine built from scratch for learning purposes. The goal is to understand the first principles of engine architecture, graphics pipelines, and real-time simulation loops.

## Core Tech Stack
*   **Language:** C++ (C++17/C++20)
*   **Build System:** CMake
*   **Graphics API:** [e.g., Vulkan, OpenGL, or DirectX]
*   **Target Platforms:** Linux (Wayland/X11) and Windows
*   **Windowing/Input:** [e.g., GLFW, SDL2, or native]

## Strict Constraints
1.  **Minimal Dependencies:** Rely on standard libraries and OS-level APIs where possible. Third-party libraries (like math or physics) must be strictly justified.
2.  **Data-Oriented Design:** Prefer contiguous memory arrays and cache-friendly data structures over deep object-oriented inheritance trees.
3.  **Explicit Memory:** Minimize dynamic allocations in the core loop. Use custom allocators (arenas, pools) for runtime systems.
4.  **No Global State:** Systems should be passed context explicitly to allow for future multithreading.
