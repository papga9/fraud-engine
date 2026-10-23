# Architectural Decision Log

## 001 - Language Standard & Build System
**Date:** 2026.10.23
**Subject:** C++20 and CMake
**Context:** Need a robust foundation for cross-platform compilation and modern C++ features that support complex simulation templates.
**Decision:** Standardize on C++20 and CMake.
**Rejected Alternatives:** C++17 (lacks Concepts, which are highly valuable for ECS/math templating), Meson/Premake (causes friction with standard C++ dependencies).
**Consequences:** The build pipeline requires CMake generators (e.g., Ninja or Makefiles) and developers must use C++20-compliant compilers (GCC 11+, Clang 13+, or MSVC 19.29+).

## 002 - Graphics API Selection
**Date:** 2026.10.23
**Subject:** Rendering Backend
**Context:** Choosing a graphics API that aligns with explicit engineering, supports data-oriented design, and runs natively on Windows and Linux (Wayland).
**Decision:** Vulkan.
**Rejected Alternatives:** OpenGL (too much hidden driver state, fights data-oriented design by relying on global context binding, and Wayland support is less native/robust than Vulkan's surface extensions).
**Consequences:** High initial complexity. The engine requires extensive upfront boilerplate (Instance, Physical Device, Logical Device, Swapchain, Pipelines, Command Buffers) before rendering.

## 003 - Platform & Windowing Abstraction
**Date:** 2026.10.23
**Subject:** OS Hardware Abstraction
**Context:** Need a cross-platform layer to open windows and capture raw input without writing raw Win32 API and Wayland protocol code.
**Decision:** GLFW.
**Rejected Alternatives:** SDL2/SDL3 (too heavy, relies on event polling instead of callbacks, and manages subsystems like audio and threading that we prefer to control explicitly). Native OS Calls (massive time sink).
**Consequences:** GLFW strictly handles window contexts and inputs. Audio, threading, and robust controller mapping must be implemented manually or via distinct, focused libraries later.

## 004 - Architectural Paradigm
**Date:** 2026.10.23
**Subject:** Entity and Simulation Management
**Context:** Structuring how game entities and logic are stored and processed in the main loop to maximize performance and cache coherency.
**Decision:** Data-Oriented Design via Entity Component System (ECS).
**Rejected Alternatives:** Traditional Object-Oriented Design / Scene Graphs (causes cache misses, memory fragmentation, and tight coupling).
**Consequences:** Game objects are strictly integer IDs. Data is grouped by component type in contiguous arrays. Systems iterate over these arrays. This aligns perfectly with submitting bulk command buffers to Vulkan.

## 005 - Main Loop & State Synchronization
**Date:** 2026.10.23
**Subject:** Threading and Loop Architecture for V1
**Context:** The Editor UI (ImGui), Game Simulation, and Vulkan Renderer must coexist without memory data races.
**Decision:** Single-threaded interleaved loop (`Input -> ImGui -> ECS Update -> Render Extraction -> Vulkan`).
**Rejected Alternatives:** Multi-threaded (Logic vs. Render threads). While this is the industry standard for performance, the synchronization barriers (double-buffered state queues) introduce too much complexity for V1.
**Consequences:** Rendering framerate is directly tied to the simulation update rate. We will design the systems to be thread-safe for a future V2 upgrade.

## 006 - ECS Implementation
**Date:** 2026.10.23
**Subject:** Entity Component System Backbone
**Context:** Need a fast, cache-friendly data structure to store components in contiguous arrays.
**Decision:** Custom C++20 Sparse Set.
**Rejected Alternatives:** EnTT. While EnTT is the industry standard and highly optimized, building a custom sparse set aligns with the project's core goal of learning first-principles memory management and data-oriented design.
**Consequences:** High initial development overhead. Requires manual implementation of dense component arrays, sparse entity indices, intersection view iterators, and "swap-and-pop" memory deletion logic.

## 007 - Reflection & Serialization Pipeline
**Date:** 2026.10.23
**Subject:** Exposing C++ Data to the Editor and Disk
**Context:** The ImGui editor needs to automatically generate UI for components, and scenes must be saved/loaded. C++20 lacks native reflection.
**Decision:** External Code Generation via Python.
**Rejected Alternatives:** Macro-based registration (inflexible, pollutes headers, tedious to maintain).
**Consequences:** Introduces Python as a strict build dependency. A pre-build step in CMake will run a Python script to parse headers for `// @reflect` tags and generate the necessary C++ serialization boilerplate before compilation.
