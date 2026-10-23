# Engine Architecture

## 1. The Core Loop
For V1, the engine operates on a single-threaded interleaved loop to safely synchronize Editor UI and Vulkan rendering without complex thread barriers.
*   `Engine::Run()`
    *   `glfwPollEvents()`
    *   `ImGui_Impl_NewFrame()`
    *   `If (PlayMode) -> ECS.Update(dt)`
    *   `RenderExtractionSystem()`
    *   `VulkanRenderer.DrawFrame(RenderPacket)`
    *   `glfwSwapBuffers()`

## 2. The Entity Component System (ECS)
The engine utilizes a custom-built Sparse Set ECS for strict Data-Oriented Design.
*   **Entities:** Pure `uint32_t` IDs. No game objects, no inheritance.
*   **Components:** Plain Old Data (POD) structs.
*   **Memory Model:** Components are stored in contiguous dense arrays (`std::vector<T>`). A sparse array maps the Entity ID to the physical index in the dense array.
*   **Destruction:** Deleting an entity swaps its components with the last elements in the dense arrays and pops the back, ensuring zero memory fragmentation.

## 3. Editor and Serialization Pipeline
Because C++20 lacks native reflection, the engine uses an external Python build step.
*   **Header Generation:** A Python script parses C++ headers for `// @reflect` tags and generates C++ serialization boilerplate before CMake compiles the engine.
*   **ImGui Hook:** The generated metadata allows the editor to automatically render sliders and text fields for any component attached to an entity.
*   **State Separation:** The ECS can serialize its entire sparse set memory state to JSON. Entering "Play Mode" saves a temporary state; exiting "Play Mode" restores it, preventing simulation logic from permanently mutating editor data.

## 4. Render Extraction (The Boundary)
Vulkan has zero knowledge of the ECS, Entities, or Components.
*   Before rendering, an Extraction System iterates over the ECS to find all entities with a `Mesh` and `Transform`.
*   It copies this data into a `RenderPacket`—a flat, cache-friendly array of `DrawCommand` structs (containing matrices and Material IDs).
*   The `VulkanRenderer` consumes this packet to record and submit Command Buffers.
