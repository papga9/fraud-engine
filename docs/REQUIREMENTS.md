# Engine Requirements & Feature Scope

## Core Philosophy
This engine is built as an **Editor-First** environment (similar to Unity/Godot), allowing live manipulation of simulation data, shaders, and entities via a GUI.

## V1: The Minimum Viable Editor (MVE)
These features must be complete before moving to advanced rendering.

*   **Window & UI:** GLFW window hosting a Vulkan context, with **Dear ImGui** overlay for the editor interface.
*   **Math Foundation:** [Decision: Custom Math or GLM] for matrix transformations and camera logic.
*   **Editor Camera:** A free-fly 3D camera (WASD + Mouse) decoupled from the simulation.
*   **Asset Pipeline:** 
    *   Strict support for `glTF 2.0` models only.
    *   Basic Asset Registry to prevent duplicate loading of meshes/textures into VRAM.
*   **Live Material System:** ImGui sliders hooked into Vulkan Uniform Buffers (UBOs) to tweak shader variables in real-time.
*   **Data-Oriented Scene:** A basic Entity Component System (ECS) to store Transforms, Mesh References, and Material IDs.

## V2: Scene Management & Interaction
*   **Serialization:** Save and load ECS states (scenes) to/from JSON.
*   **Mouse Picking:** Raycasting from the editor viewport to select and translate/rotate/scale entities in 3D space.
*   **Render Pass Toggling:** A UI switch to swap the Vulkan pipeline between a "Fast/Wireframe" mode and a "Full PBR Lit" mode.

## V3: Advanced Graphics Research (Future)
*   Hardware Ray Tracing (`VK_KHR_ray_tracing_pipeline`).
*   Compute Shader integration for physics or particle simulations.
*   Render Graph architecture for complex post-processing pipelines.
