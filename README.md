<div align="center">

# Parx

**An open-source, XR-native engine built for the next generation of immersive applications.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/github/actions/workflow/status/marcobuttiglione/parx/ci.yml)](https://github.com/marcobuttiglione/parx/actions)
[![OpenXR](https://img.shields.io/badge/OpenXR-1.0-green.svg)](https://www.khronos.org/openxr/)
[![Vulkan](https://img.shields.io/badge/Vulkan-1.3-red.svg)](https://www.vulkan.org/)

[Vision](#vision) · [Features](#features) · [Getting Started](#getting-started) · [Roadmap](#roadmap) · [Contributing](#contributing) · [Devlog](#devlog)

</div>

---

## Vision

Most game engines treat VR and AR as an afterthought — a plugin bolted onto a pipeline designed for flat screens.

**Parx is different.** It is built from the ground up with XR as the primary rendering target, not an extension. Every architectural decision, from the scene graph to the renderer to the input system, is made with immersive applications in mind.

The goal is not to replace Unity or Unreal for general-purpose game development. The goal is to give researchers, indie developers, and XR practitioners a **lean, transparent, and hackable engine** that speaks the language of immersive computing natively.

> Parx is vibebuilt: developed publicly, iteratively, and entirely with [Claude Code](https://claude.ai/code) as the AI pair programmer. Every session, decision, and failure is documented.

---

## Why Parx?

| | Unity | Unreal | Godot | **Parx** |
|---|---|---|---|---|
| XR-native architecture | No | No | No | **Yes** |
| Open source | Partial | Partial | Yes | **Yes** |
| Lightweight & hackable | No | No | Yes | **Yes** |
| Vulkan + OpenXR core | No | No | No | **Yes** |
| Research-friendly | No | No | Partial | **Yes** |

---

## Features

> Parx is in early development. This section reflects the planned feature set for v1.0. Current implementation status is tracked in the [Roadmap](#roadmap).

### Core

- **OpenXR-first**: targets any OpenXR-compliant runtime out of the box (SteamVR, Meta Quest, Windows MR, and more)
- **Vulkan renderer**: explicit, low-level, stereo-aware from day one
- **ECS architecture**: data-oriented entity-component-system for performance and modularity
- **GLTF 2.0 asset pipeline**: the open standard for 3D content, no proprietary formats

### XR

- Stereo rendering with built-in reprojection support
- Foveated rendering hooks (application-level and fixed)
- Spatial audio integration points
- Hand tracking and controller abstraction via OpenXR input system
- Passthrough / mixed reality compositing layer support

### Developer Experience

- C++23 codebase, readable and documented
- CMake + vcpkg build system, reproducible on Windows, Linux, macOS
- Hot-reload friendly architecture
- Comprehensive logging and debug overlay in headset

---

## Getting Started

### Prerequisites

- **CMake** >= 3.27
- **vcpkg** (auto-bootstrapped via CMake preset)
- **Vulkan SDK** >= 1.3
- **OpenXR SDK** >= 1.0
- A C++23-capable compiler (MSVC 19.38+, GCC 13+, Clang 17+)

### Build

```bash
git clone https://github.com/marcobuttiglione/parx.git
cd parx
cmake --preset default
cmake --build build
```

### Run (desktop preview mode, no headset required)

```bash
./build/parx_sandbox
```

### Run (XR mode)

Make sure your OpenXR runtime is active (SteamVR, Oculus, etc.), then:

```bash
./build/parx_sandbox --xr
```

---

## Architecture Overview

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

---

## Roadmap

### Milestone 0 — Bootstrap (current)

- [x] Repository structure and build system (CMake + vcpkg)
- [ ] Vulkan context initialization
- [ ] Render loop with clear pass
- [ ] OpenXR session creation and swapchain
- [ ] Stereo clear pass on headset

### Milestone 1 — First Triangle

- [ ] Vertex buffer abstraction
- [ ] Shader compilation pipeline (GLSL -> SPIR-V)
- [ ] First rendered geometry in XR (stereo triangle)
- [ ] GLM integration for math

### Milestone 2 — Scene Foundation

- [ ] ECS core implementation
- [ ] Transform hierarchy
- [ ] GLTF 2.0 loader (cgltf)
- [ ] First GLTF scene rendered in XR

### Milestone 3 — Input and Interaction

- [ ] OpenXR input action system
- [ ] Controller pose tracking
- [ ] Basic interaction primitives (ray, grab)

### Milestone 4 — Developer Experience

- [ ] Hot-reload for shaders
- [ ] In-headset debug overlay
- [ ] Performance profiling hooks

---

## Devlog

Parx is developed in public. Every significant step is documented.

- **GitHub Commits**: atomic, readable, narrative
- **LinkedIn**: weekly deep-dives on architecture decisions and lessons learned
- **Twitter/X**: progress screenshots, GIFs, and short technical threads

> Follow the journey: [@m_buttiglione](https://twitter.com/m_buttiglione) · [LinkedIn](https://linkedin.com/in/marco-dom-buttiglione)

---

## Contributing

Parx is in early development and the architecture is still being shaped. Contributions are welcome, but please open an issue before submitting large PRs — this helps align effort with the current roadmap.

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

### Areas where help is most welcome

- Vulkan render graph design feedback
- OpenXR runtime testing across devices (Quest, SteamVR, WMR)
- Documentation and technical writing
- Performance profiling and optimization

---

## License

Parx is licensed under the **Apache License 2.0**. See [LICENSE](LICENSE) for full terms.

Apache 2.0 was chosen over MIT because it includes an explicit patent grant, protecting contributors and users from patent litigation — important for a project touching hardware-adjacent software.

---

## Acknowledgements

Parx is built on the shoulders of open standards and open projects:

- [Khronos OpenXR](https://www.khronos.org/openxr/) — the open XR runtime standard
- [Vulkan](https://www.vulkan.org/) — explicit GPU programming
- [GLM](https://github.com/g-truc/glm) — mathematics for graphics
- [cgltf](https://github.com/jkuhlmann/cgltf) — single-file GLTF 2.0 parser
- [Catch2](https://github.com/catchorg/Catch2) — C++ testing framework

---

<div align="center">
<sub>Built in public. Vibebuilt with Claude Code.</sub>
</div>
