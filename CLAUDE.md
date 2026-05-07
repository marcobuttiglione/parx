# Parx — Claude Code Context

This file is read automatically by Claude Code at the start of every session.
Do not delete it. Keep it updated as the project evolves.

---

## Project Identity

**Parx** is an open-source, XR-native game engine written in C++23.
Repository: https://github.com/marcobuttiglione/parx

The engine is built from the ground up with XR as the primary rendering target.
It is not a general-purpose engine with an XR plugin — it is an XR engine first.

The project is developed entirely in public, vibebuilt with Claude Code as the primary AI pair programmer.
Every session, decision, and failure is documented on GitHub, LinkedIn, and X (@m_buttiglione).

---

## Stack

| Layer | Technology |
|---|---|
| Language | C++23 |
| Build system | CMake 3.27+ with vcpkg |
| Graphics API | Vulkan 1.3 |
| XR runtime | OpenXR 1.0 |
| Math | GLM |
| Windowing | GLFW |
| Asset format | GLTF 2.0 (cgltf) |
| Testing | Catch2 |

---

## Architecture

```
parx/
├── engine/
│   ├── platform/       # Windowing, OpenXR session lifecycle, OS abstraction
│   ├── renderer/       # Vulkan backend, stereo pipeline, render graph
│   ├── scene/          # ECS core, spatial hierarchy, scene management
│   ├── input/          # OpenXR input system, action mapping
│   └── assets/         # GLTF loader, texture, mesh, shader management
├── editor/             # (planned) lightweight scene editor
├── sandbox/            # Testbed application, development playground
├── tests/              # Unit and integration tests (Catch2)
└── docs/               # Technical documentation and devlog archive
```

The architecture is layered. Each module has a clear boundary and depends only on modules below it in the stack. No circular dependencies.

The scene graph is ECS-based (Entity Component System). Prefer data-oriented design over deep OOP hierarchies.

---

## Coding Conventions

- **Language**: C++23. Use modern features where they improve clarity (concepts, ranges, std::expected).
- **Naming**: snake_case for files, variables, and functions. PascalCase for types and classes.
- **Error handling**: no exceptions in hot paths. Use `std::expected` or explicit error codes.
- **Memory**: prefer value types and stack allocation. Heap allocation must be explicit and justified.
- **Comments**: every public API must have a doc comment (Doxygen style).
- **Headers**: use `#pragma once`. Keep headers lean, implementation in .cpp files.
- **No magic numbers**: all constants must be named.

---

## Commit Conventions

Follow **Conventional Commits**:

```
feat: add Vulkan swapchain initialization
fix: correct OpenXR session state machine transition
chore: update vcpkg baseline
docs: add renderer architecture notes
refactor: extract platform window abstraction
test: add ECS component query unit tests
```

Commits must be atomic. One logical change per commit.
Commit messages must be readable as a project history by someone who was not there.

---

## Current Milestone

### Milestone 0 — Bootstrap

Goal: a clean, buildable project skeleton. No rendering yet.

- [x] README.md and CLAUDE.md in place
- [ ] CMakeLists.txt root configuration
- [ ] vcpkg manifest (vcpkg.json) with all dependencies declared
- [ ] CMake presets (CMakePresets.json) for default debug/release builds
- [ ] engine/ module skeleton (empty headers, no implementation)
- [ ] sandbox/ application that compiles and prints "Parx v0.0.1" to stdout
- [ ] .gitignore for CMake, vcpkg, and common IDEs
- [ ] CI workflow (GitHub Actions) that builds on push

When Milestone 0 is complete, tag the commit as `v0.0.1`.

---

## Milestones Overview

| # | Name | Goal |
|---|---|---|
| 0 | Bootstrap | Buildable skeleton, CI green |
| 1 | First Triangle | Stereo triangle rendered in XR via Vulkan + OpenXR |
| 2 | Scene Foundation | ECS core, transform hierarchy, GLTF scene in XR |
| 3 | Input and Interaction | OpenXR input actions, controller tracking, ray interaction |
| 4 | Developer Experience | Shader hot-reload, in-headset debug overlay, profiling hooks |

---

## Design Principles

- **XR-first**: every API is designed assuming stereo rendering and 6DOF input. Flat-screen mode is a debug convenience, not a first-class target.
- **Explicit over implicit**: prefer Vulkan-style explicitness in the engine API. No hidden state, no magic.
- **Lean dependencies**: every dependency must earn its place. Prefer single-file or header-only libraries where possible.
- **Research-friendly**: the codebase should be readable by a graphics PhD student or a systems researcher without prior engine experience. Prioritize clarity over cleverness.
- **Buildable at all times**: the main branch must always compile and run. Work in progress goes in feature branches.

---

## Notes for Claude Code

### About the lead developer
The lead developer is Marco Buttiglione, a PhD student in Computer Graphics at Politecnico di Milano (DEIB). His research spans computer graphics, VR/AR, real-time simulation, and HCI. He has strong C++ and graphics knowledge. Do not over-explain basics, do not add patronizing comments in code, treat him as a peer.

### About the process
Marco does not work alone. There is a second Claude instance — a Claude.ai chat — that acts as tech lead, content strategist, and documentation reviewer for the project. Claude Code handles implementation. The Claude.ai chat handles architecture brainstorming, devlog writing, LinkedIn and X posts, and review of milestone documentation.

The two instances coordinate through Marco. When Claude Code produces something worth documenting or discussing at a higher level, it should flag it explicitly so Marco knows to bring it to the other instance.

### General rules
- Before writing any code, confirm you understand the current milestone and its open checkboxes.
- Do not implement features beyond the current milestone unless explicitly asked.
- When in doubt about an architectural decision, stop and ask before implementing. A wrong architecture is worse than a slow implementation.
- If you introduce a new dependency, update `vcpkg.json` and add a one-line justification comment in this file under the Stack section.
- The main branch must always compile. Work in progress goes in feature branches named `feat/description` or `milestone/N-description`.

### Commit workflow
After completing each logical unit of work, Claude Code must:
1. Suggest an atomic commit message following Conventional Commits.
2. Wait for Marco to confirm before proceeding to the next unit.

Do not batch unrelated changes into a single commit. One concern, one commit.

### Documentation workflow
At the end of every milestone, Claude Code must produce a `docs/devlog/milestone-N.md` file containing:
- What was built in this milestone
- Key architectural decisions made and why
- Dependencies introduced
- Problems encountered and how they were solved
- What is left open or deferred
- Suggested content ideas for LinkedIn and X posts based on this milestone

This file is handed off to Marco to bring to the Claude.ai chat for devlog writing and social content production.

### Session start checklist
At the start of every session, Claude Code must:
1. Read this file in full.
2. State the current milestone and which checkboxes are still open.
3. Propose the first concrete action for this session.
4. Wait for Marco to confirm before starting.
