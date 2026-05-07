# Milestone 0 — Bootstrap

**Tag:** `v0.0.1` (pending)
**Date:** 2026-05-07
**Commits:** b9d5811 → bae7cb3

---

## What was built

A clean, compilable project skeleton for Parx — an XR-native game engine in C++23. No rendering, no XR session, no window — just a solid build system foundation that everything else will grow on top of.

### Deliverables
- **CMakeLists.txt** — root configuration, C++23, CMake 3.27+
- **vcpkg.json** — all six runtime dependencies declared upfront
- **CMakePresets.json** — `debug` and `release` configure/build presets using Ninja
- **engine/** — five INTERFACE library targets (platform, renderer, scene, input, assets) with the correct include path layout (`<parx/module/module.hpp>`)
- **sandbox/main.cpp** — entry point that prints `Parx v0.0.1` to stdout
- **tests/CMakeLists.txt** — Catch2 harness wired to CTest, ready for Milestone 2
- **.gitignore** — CMake, vcpkg, common IDEs
- **.github/workflows/ci.yml** — GitHub Actions build on push (ubuntu-24.04, Ninja, vcpkg)

---

## Key architectural decisions

### INTERFACE library targets for the engine skeleton
Each engine submodule is a CMake `INTERFACE` library. This is the correct pattern for header-only or not-yet-implemented modules: it expresses the dependency graph and sets up include paths without requiring source files. When modules gain implementation, they become `STATIC` or `SHARED` targets with no change to how they are consumed.

The dependency edges encoded at this stage:
- `parx-renderer` → `parx-platform` (renderer will need windowing and XR session context)
- `parx-input` → `parx-platform` (input actions are tied to the XR session)
- `parx-scene`, `parx-assets` — no engine-internal dependencies for now

An umbrella `parx-engine` target aggregates all five for consumers like sandbox.

### vcpkg manifest mode, no baseline pinning yet
All six dependencies are declared in `vcpkg.json` now, even though none are used yet. This front-loads the dependency audit and avoids a future "add dep, rebuild everything" churn mid-milestone. Baseline pinning (`builtin-baseline` in vcpkg.json) is deferred — we will add it when the dep set stabilizes after Milestone 1.

### No `find_package` calls in Milestone 0
The CMakeLists.txt has zero `find_package` calls. vcpkg installs packages from the manifest, but nothing links against them until there is real code. This keeps the CI setup minimal — no Vulkan SDK installation step, no OpenXR headers required to verify a `puts()` call compiles.

### VCPKG_ROOT via environment variable
CMakePresets.json reads `$env{VCPKG_ROOT}`. This is the standard convention: developers export it from their shell profile; CI derives it from `VCPKG_INSTALLATION_ROOT` (the env var GitHub Actions exposes for the pre-installed vcpkg). No hardcoded paths anywhere.

### CI target: compile-only, ubuntu-24.04
Milestone 0 CI does not run tests or launch the sandbox. Its sole job is to prove the project builds from a clean environment. A headset and Vulkan driver are not available in the GitHub Actions runner; there is no point testing them until the code exists. CI will be extended as each milestone adds runnable artifacts.

---

## Dependencies introduced

| Package | vcpkg name | Justification |
|---|---|---|
| GLM | `glm` | Math library for vectors, matrices, quaternions — required everywhere in a graphics engine |
| GLFW | `glfw3` | Cross-platform windowing for the flat-screen debug path and Vulkan surface creation |
| OpenXR Loader | `openxr-loader` | XR session lifecycle, swapchain, input actions — the engine's primary runtime target |
| Vulkan Headers | `vulkan-headers` | Graphics API headers; the loader is provided by the system runtime |
| cgltf | `cgltf` | Single-header GLTF 2.0 parser; chosen over tinygltf to avoid a stb_image transitive pull |
| Catch2 | `catch2` | Unit and integration test framework; v3 required for CMake-native `catch_discover_tests` |

---

## Problems encountered

None of substance. The main judgment call was whether to run `find_package` for all deps at configure time to validate the vcpkg manifest. Decision: no, because it adds CI complexity (Vulkan SDK apt install, OpenXR system deps) for zero benefit at this milestone. The packages are installed by vcpkg regardless; they just are not linked.

---

## What is left open or deferred

- **Baseline pinning** — `vcpkg.json` has no `builtin-baseline`. Add after Milestone 1 once the dep set is confirmed.
- **`v0.0.1` tag** — CLAUDE.md specifies tagging when Milestone 0 is complete. CI must be green first.
- **`editor/` directory** — listed in the architecture but marked planned; not scaffolded to avoid empty directories with no purpose yet.
- **`docs/` structure beyond devlog** — technical documentation referenced in CLAUDE.md is deferred to when there is something to document.

---

## Content ideas for LinkedIn and X

### LinkedIn post angle
*"I'm building an XR game engine from scratch with C++ and AI pair programming — here's what Day 1 looks like."*

- The philosophy: XR-first, not XR-as-plugin. Every API decision starts from stereo rendering and 6DOF input.
- What a "Milestone 0" actually means in a serious engine project: before any rendering, you spend a session just getting the build system right. CMake, vcpkg, presets, CI, module boundaries.
- The two-Claude workflow: Claude Code handles implementation; a Claude.ai instance handles architecture review, devlog writing, and content. Coordinated through me.
- Hook: "The sandbox currently prints one line. That line took real decisions to get right."

### X / short-form angle
- "Started Parx today. It compiles. It prints 'Parx v0.0.1'. CI is green. This is what zero looks like before it becomes something." + repo link
- Thread option: walk through the CMake INTERFACE library pattern — why it's the right skeleton structure for a modular engine before any source files exist.
- Engagement hook: "What's your vcpkg baseline strategy for a new C++ project? Pin from day one or wait?"
