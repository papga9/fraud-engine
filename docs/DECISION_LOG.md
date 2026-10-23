# Architectural Decision Log

## [Format Template]
**Date:** YYYY-MM-DD
**Subject:** [What is the architectural choice?]
**Context:** [Why is this a decision? What is the problem?]
**Decision:** [What are we doing?]
**Rejected Alternatives:** [What else did we consider and why was it rejected?]
**Consequences:** [How does this impact the rest of the engine?]

---

## 001 - Build System Selection
**Date:** [Current Date]
**Subject:** C++ Build System
**Context:** Need a cross-platform way to compile the engine on Linux and Windows.
**Decision:** Use CMake.
**Rejected Alternatives:** Makefiles (too platform-specific), Premake (adds Lua dependency), Ninja (will use as a CMake generator, not directly).
**Consequences:** All new modules must have their `CMakeLists.txt` updated.
