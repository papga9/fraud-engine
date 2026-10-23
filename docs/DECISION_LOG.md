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
