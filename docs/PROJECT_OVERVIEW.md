# Engine Project Overview

## Vision
A custom 3D game engine built from scratch for learning purposes. The goal is to understand the first principles of engine architecture, real-time graphics pipelines, and data-oriented simulation loops by avoiding black-box abstractions.

## Core Tech Stack
*   **Language:** C++20
*   **Build System:** CMake
*   **Graphics API:** Vulkan
*   **Target Platforms:** Linux (Wayland/X11) and Windows
*   **Windowing/Input:** GLFW

## Strict Constraints
1.  **Explicit Control:** Avoid implicit state machines. State must be explicitly managed and synchronized, heavily leveraging Vulkan's deterministic design.
2.  **Data-Oriented Design:** Prefer contiguous memory arrays and cache-friendly data structures (Entity Component System) over deep object-oriented inheritance trees.
3.  **Minimal Dependencies:** Rely on standard libraries and OS-level APIs where possible. Use external libraries only when they strictly solve a hardware/platform abstraction problem (like GLFW) without dictating application logic.
4.  **Explicit Memory:** Minimize dynamic allocations in the core loop. Use custom allocators (arenas, linear, pools) for runtime systems and Vulkan device memory.
5.  **No Global State:** Systems should be passed context explicitly to allow for modularity and future multithreading.
