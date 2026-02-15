# AR-Native

A universal framework for building AR glasses applications. Write once, deploy to any AR platform — like Expo, but for AR glasses.

AR-Native abstracts away the differences between AR glasses SDKs (Rokid, Even Realities, XREAL, Vuzix, RayNeo, Meta, Android XR, visionOS, and more) behind a unified API, so you can build your app once and run it on any supported device.

## Why AR-Native?

The AR glasses landscape is deeply fragmented. Every platform ships its own SDK, its own language requirements, and its own way of doing things:

| Platform | SDK | Language | Target OS |
|---|---|---|---|
| Even Realities | BLE Protocol / Even Hub | Dart (Flutter) | iOS, Android |
| Rokid | UXR SDK | C# (Unity) | Android |
| XREAL | XREAL SDK 3.0 / AR Foundation | C# (Unity) | Android |
| Vuzix Z100 | Ultralite SDK | Java/Kotlin, Swift | Android, iOS |
| RayNeo | ARDK | C# (Unity), Java/Kotlin | Android |
| Android XR | Jetpack XR SDK / Glimmer | Kotlin | Android XR |
| Apple visionOS | RealityKit + ARKit | Swift | visionOS |
| Meta Quest | OpenXR + Spatial SDK | C/C++, C# (Unity) | Horizon OS |
| Meta Ray-Ban | Wearables DAT | Java/Kotlin, Swift | Android, iOS |

If you build an app for one platform, you're locked in. Porting means rewriting from scratch.

**AR-Native solves this.** Build your app against one API. We handle the rest.

### Breaking the Android XR Lock-In

Google's Android XR and Samsung's partnership aim to be the default platform for smart glasses — but that locks developers into Kotlin, Jetpack, and the Android ecosystem. AR-Native exists so that **HUD glasses and AR glasses development is not gated by any single platform**. Your app should be deployable to:

- **Android** devices and Android XR headsets/glasses
- **iOS** — iPhones as companion devices for BLE-connected glasses (Even Realities, Vuzix Z100, Meta Ray-Ban)
- **macOS** — desktop companion apps for development, testing, and always-on HUD glass connectivity
- **visionOS** — Apple Vision Pro and future Apple AR glasses

Whether the glasses run their own OS, pair with a phone over BLE, or connect to a desktop, AR-Native provides the abstraction layer so you write your logic once and target whatever platform your users are on.

### Two Worlds, One API

AR glasses fall into two distinct categories:

**HUD / Smart Glasses** — lightweight, low-power, companion-app-driven:
- Even Realities G1 (monochrome micro-display, BLE, no camera)
- Vuzix Z100 (monochrome waveguide, BLE, no camera)
- Meta Ray-Ban (camera + mic, display coming, BLE companion)
- Android XR AI Glasses (Glimmer composables, small display)

**Spatial AR Glasses / Headsets** — full 6DoF tracking, spatial rendering:
- Rokid Max Pro (6DoF SLAM, MicroOLED stereo, Unity)
- XREAL (6DoF, plane detection, AR Foundation)
- RayNeo X3 Pro (6DoF SLAM, hand tracking, dual cameras)
- Meta Quest 3 (6DoF, hand tracking, passthrough MR)
- Apple Vision Pro (6DoF, hand + eye tracking, spatial UI)
- Android XR Headset (spatial panels, ARCore, Jetpack XR)

AR-Native abstracts both classes behind a single API. HUD devices get `sendText()`, `sendImage()`, and `sendNotification()`. Spatial devices additionally get `renderSpatialContent()`, `detectPlanes()`, and `trackHands()`. Your app queries device capabilities at runtime and adapts.

## Project Structure

```
AR-Native/
├── .github/workflows/          # CI/CD pipelines (testing, publishing, docs generation)
│
├── docs/                       # Documentation
│   ├── api-reference/          # Auto-generated API docs
│   ├── platform-guides/        # Per-platform setup & quirks (Rokid, Even Realities, Meta)
│   └── examples/               # Walkthrough guides and tutorials
│
├── packages/
│   ├── core/                   # Core abstractions & types — the universal API layer
│   │   └── src/
│   │       ├── types/          # Shared type definitions (display, audio, camera, sensors, connection)
│   │       ├── interfaces/     # Device & controller interfaces that all adapters implement
│   │       ├── errors/         # Standardized error types
│   │       └── utils/          # Logging, validation, and shared helpers
│   │
│   ├── adapters/               # Platform-specific implementations — one per AR glasses SDK
│   │   ├── rokid/              # Rokid AR glasses adapter (UXR protocol)
│   │   ├── evenrealities/      # Even Realities adapter (BLE protocol, image transmit)
│   │   └── meta/               # Meta AR glasses adapter
│   │
│   ├── react-native/           # React Native bindings
│   │   └── src/
│   │       ├── hooks/          # React hooks (useARGlasses, useDisplay, useCamera)
│   │       ├── components/     # Provider and renderer components
│   │       ├── android/        # Native Android bridge modules
│   │       └── ios/            # Native iOS bridge modules
│   │
│   ├── cli/                    # Developer CLI tooling (project init, testing, deployment)
│   │
│   └── examples-shared/        # Shared example code and utilities used across example apps
│       └── src/
│           ├── notifications/  # Notification display patterns
│           ├── navigation/     # AR navigation patterns
│           └── translation/    # Live translation patterns
│
├── examples/                   # Complete, runnable example apps
│   ├── react-native-demo/      # Full-featured demo app
│   ├── notifications-app/      # Simple notification display app
│   ├── translation-app/        # Live translation example
│   └── navigation-app/         # AR navigation example
│
├── tools/                      # Build & dev tooling
│   ├── scripts/                # Build, test, and publish shell scripts
│   └── configs/                # Shared configs (Jest, ESLint, TypeScript base)
│
└── [root config files]         # package.json, tsconfig, lerna, prettier, eslint, license
```

## How It Works

**Core** defines the universal interfaces and types — what a "display", "camera", or "device" looks like regardless of hardware.

**Adapters** implement those interfaces for each specific AR glasses platform, translating the universal API into platform-specific SDK calls. Adding support for a new device means writing a new adapter, not changing app code.

**React Native bindings** expose the core API as hooks and components, so building an AR app feels like building any other React Native app.

**CLI** helps developers scaffold projects, run device tests, and deploy to connected glasses.

## Supported Platforms (Planned)

| Platform | Device Class | Connection Model | Adapter Status |
|---|---|---|---|
| Even Realities G1/G2 | HUD | BLE companion (iOS/Android) | Planned |
| Vuzix Z100 | HUD | BLE companion (iOS/Android) | Planned |
| Vuzix M-series | Full Android | On-device | Planned |
| Meta Ray-Ban | HUD / Wearable | BLE companion (iOS/Android) | Planned |
| Rokid Max Pro | Spatial AR | On-device (Android) | Planned |
| XREAL | Spatial AR | Android phone / BeamPro | Planned |
| RayNeo X3 Pro | Spatial AR | On-device (Android) | Planned |
| Android XR (glasses) | HUD | On-device (Android XR) | Planned |
| Android XR (headset) | Spatial AR | On-device (Android XR) | Planned |
| Meta Quest 3 | Spatial AR / MR | On-device (Horizon OS) | Planned |
| Apple Vision Pro | Spatial AR | On-device (visionOS) | Planned |

## Development Roadmap

### Phase 1 — Foundation & Core Architecture

Define the universal abstraction layer, set up the monorepo, and get the build system working.

- [ ] Set up monorepo tooling (Lerna, TypeScript, ESLint, Prettier, Jest configs)
- [ ] Define `IARGlassesDevice` interface with device class discriminator (`hud` vs `spatial`)
- [ ] Define `IDisplayController` interface (text, image, notification methods)
- [ ] Define `ICameraController` interface (photo capture, video stream)
- [ ] Define `IAudioController` interface (mic stream, speaker output)
- [ ] Define `IConnectionManager` interface (BLE scanning, pairing, connection lifecycle)
- [ ] Define shared types: `DisplayCapabilities`, `SensorCapabilities`, `DeviceInfo`, `ConnectionState`
- [ ] Implement `DeviceLifecycle` manager (initialize → configure → run → disconnect)
- [ ] Implement standardized error types (`ARGlassesError`, `ConnectionError`, `UnsupportedCapabilityError`)
- [ ] Implement logging and validation utilities
- [ ] Set up CI pipeline (GitHub Actions for lint, type-check, unit tests)
- [ ] Write unit tests for all core interfaces and types

### Phase 2 — BLE Transport Layer & First HUD Adapter (Even Realities)

Build the shared BLE communication layer, then implement the first adapter targeting Even Realities G1 as the reference HUD device.

- [ ] Research and document Even Realities BLE protocol in detail (dual-BLE, packet format, CRC, ACK flow)
- [ ] Implement `BLETransport` base class (scan, connect, disconnect, send/receive packets)
- [ ] Handle Even Realities dual-BLE architecture (left-arm-first, right-arm-after-ACK)
- [ ] Implement `EvenDevice` (implements `IARGlassesDevice` for Even Realities G1/G2)
- [ ] Implement `EvenDisplayController` (text rendering with pagination, BMP image sending at 194-byte packets)
- [ ] Implement Even Realities audio streaming (LC3 format via BLE mic activation)
- [ ] Implement Even Realities touch bar input handling (tap events, long-press)
- [ ] Write integration tests using BLE mock/simulator
- [ ] Create platform guide doc: `docs/platform-guides/evenrealities.md`
- [ ] Build a minimal "Hello World" example that sends text to Even Realities G1

### Phase 3 — Second HUD Adapter (Vuzix Z100) & BLE Transport Reuse

Validate the BLE transport abstraction by building a second HUD adapter. Refine the shared layer based on what differs.

- [ ] Research Vuzix Ultralite SDK protocol (BLE scanning, pairing, Canvas API)
- [ ] Implement `VuzixDevice` (implements `IARGlassesDevice`)
- [ ] Implement `VuzixDisplayController` (monochrome waveguide, text, Canvas drawing)
- [ ] Implement Vuzix tap/input event handling
- [ ] Refactor `BLETransport` if needed to accommodate Vuzix protocol differences
- [ ] Write integration tests with Vuzix BLE mocks
- [ ] Create platform guide doc: `docs/platform-guides/vuzix.md`
- [ ] Build side-by-side example: same notification app running on Even Realities and Vuzix

### Phase 4 — React Native Bindings

Expose the core API to React Native so developers can build apps with familiar tools.

- [ ] Implement `ARGlassesProvider` context component (device discovery, connection state)
- [ ] Implement `useARGlasses()` hook (connect, disconnect, get device info)
- [ ] Implement `useDisplay()` hook (send text, send image, send notification)
- [ ] Implement `useCamera()` hook (capture photo, start/stop video stream)
- [ ] Implement `useAudio()` hook (start/stop mic stream, play audio)
- [ ] Build Android native bridge module (Java/Kotlin ↔ React Native)
- [ ] Build iOS native bridge module (Swift/ObjC ↔ React Native)
- [ ] Write unit tests for all hooks and components
- [ ] Create a React Native example app that connects to any available HUD glasses
- [ ] Publish `@ar-native/react-native` package to npm

### Phase 5 — Meta Wearables Adapter (Ray-Ban Meta)

Add support for Meta's Ray-Ban smart glasses, which bring camera and mic capabilities to the HUD class.

- [ ] Research Meta Wearables Device Access Toolkit (Android SDK + iOS SDK)
- [ ] Implement `MetaWearablesDevice` (implements `IARGlassesDevice`)
- [ ] Implement `MetaWearablesCameraController` (photo capture, video stream at 720p/30fps via BLE)
- [ ] Implement `MetaWearablesAudioController` (mic/speaker access via Bluetooth profiles)
- [ ] Handle Meta Wearables companion app lifecycle (phone-side processing)
- [ ] Write integration tests using Meta Mock Device Kit
- [ ] Create platform guide doc: `docs/platform-guides/meta-rayban.md`
- [ ] Update React Native example to support Meta Ray-Ban alongside HUD glasses

### Phase 6 — Spatial AR Interfaces & First Spatial Adapter (XREAL)

Extend the core API to cover spatial AR capabilities, then implement the first spatial adapter.

- [ ] Define `ISpatialTracker` interface (6DoF pose, world tracking, SLAM)
- [ ] Define `IPlaneDetector` interface (horizontal/vertical plane detection, plane merging)
- [ ] Define `IHandTracker` interface (hand skeleton, gestures)
- [ ] Define `IImageTracker` interface (marker detection, custom image targets)
- [ ] Define `ISpatialRenderer` interface (3D scene rendering, spatial anchors)
- [ ] Define spatial types: `Pose6DoF`, `Plane`, `HandSkeleton`, `SpatialAnchor`, `SceneNode`
- [ ] Research XREAL SDK 3.0 / AR Foundation integration
- [ ] Implement `XrealDevice` (implements `IARGlassesDevice` with spatial capabilities)
- [ ] Implement `XrealSpatialTracker` (6DoF via IMU + point clouds)
- [ ] Implement `XrealPlaneDetector` (horizontal/vertical planes via AR Foundation)
- [ ] Write integration tests for spatial tracking
- [ ] Create platform guide doc: `docs/platform-guides/xreal.md`

### Phase 7 — Rokid & RayNeo Spatial Adapters

Add two more spatial adapters to validate the spatial abstraction layer across different Unity-based SDKs.

- [ ] Research Rokid UXR2.0 SDK (Unity prefabs, input module, 6DoF)
- [ ] Implement `RokidDevice`, `RokidSpatialTracker`, `RokidDisplayController`
- [ ] Implement Rokid multimodal input handling (controller, gesture, head tracking, voice)
- [ ] Write integration tests with Rokid mocks
- [ ] Create platform guide doc: `docs/platform-guides/rokid.md`
- [ ] Research RayNeo ARDK (Unity + Android variants)
- [ ] Implement `RayNeoDevice`, `RayNeoSpatialTracker`, `RayNeoHandTracker`
- [ ] Implement RayNeo dual-camera handling (RGB sensor + depth/SLAM camera)
- [ ] Write integration tests with RayNeo mocks
- [ ] Create platform guide doc: `docs/platform-guides/rayneo.md`

### Phase 8 — Android XR Adapter (Glasses + Headset)

Support Google's Android XR platform for both AI glasses (Glimmer) and spatial headsets (Jetpack XR).

- [ ] Research Jetpack XR SDK, SceneCore, ARCore for XR, and Compose Glimmer
- [ ] Implement `AndroidXRGlassesDevice` (HUD class, Glimmer composables, small display)
- [ ] Implement `AndroidXRHeadsetDevice` (spatial class, Jetpack XR, spatial panels)
- [ ] Implement Android XR spatial tracking (ARCore plane detection, scene understanding)
- [ ] Handle Android XR Kotlin-native bridge from React Native
- [ ] Write integration tests using Android XR emulator
- [ ] Create platform guide doc: `docs/platform-guides/android-xr.md`

### Phase 9 — Apple visionOS Adapter

Bring AR-Native to Apple's spatial computing platform.

- [ ] Research RealityKit, ARKit, and SwiftUI spatial APIs
- [ ] Implement `VisionProDevice` (implements `IARGlassesDevice` with full spatial capabilities)
- [ ] Implement visionOS spatial tracking (plane estimation, scene reconstruction, world anchors)
- [ ] Implement visionOS hand tracking and eye tracking interfaces
- [ ] Implement visionOS display modes (Window, Volume, Immersive Space)
- [ ] Build iOS/visionOS native bridge module for React Native
- [ ] Write integration tests using visionOS Simulator
- [ ] Create platform guide doc: `docs/platform-guides/visionos.md`

### Phase 10 — Meta Quest / Horizon OS Adapter

Add support for Meta Quest headsets and their mixed reality capabilities.

- [ ] Research Meta OpenXR SDK + Meta Spatial SDK
- [ ] Implement `MetaQuestDevice` (implements `IARGlassesDevice` with spatial + MR capabilities)
- [ ] Implement Meta Quest spatial tracking (OpenXR reference spaces, spatial anchors)
- [ ] Implement Meta Quest hand tracking (v83 API)
- [ ] Implement Meta Quest passthrough camera API for mixed reality
- [ ] Write integration tests with Meta Quest emulator
- [ ] Create platform guide doc: `docs/platform-guides/meta-quest.md`

### Phase 11 — CLI Tooling

Build the developer CLI to streamline project creation, testing, and deployment.

- [ ] Implement `ar-native init` command (scaffold new project with adapter selection)
- [ ] Implement `ar-native devices` command (scan and list connected/paired devices)
- [ ] Implement `ar-native test` command (run on-device tests, BLE mock tests)
- [ ] Implement `ar-native deploy` command (build and install to connected glasses)
- [ ] Implement `ar-native doctor` command (check environment, SDK deps, BLE permissions)
- [ ] Add interactive device selector for multi-device setups
- [ ] Write CLI tests and documentation

### Phase 12 — Example Apps, Docs & Community Launch

Polish the developer experience, publish packages, and launch.

- [ ] Build complete notifications example app (works across all HUD adapters)
- [ ] Build live translation example app (mic → translate → display on glasses)
- [ ] Build AR navigation example app (spatial glasses with waypoints)
- [ ] Build full-featured React Native demo app showcasing all adapters
- [ ] Write getting-started guide (`docs/getting-started.md`)
- [ ] Write adapter authoring guide (how to add support for a new device)
- [ ] Generate API reference docs from TypeScript types
- [ ] Set up documentation site (Docusaurus or similar)
- [ ] Publish all packages to npm under `@ar-native/*` scope
- [ ] Set up GitHub issue templates, contributing guide, and code of conduct
- [ ] Write announcement blog post / launch on Product Hunt, HN, AR community forums

## Contributing

This project is in early development. If you're interested in contributing — especially if you have access to any of the hardware platforms listed above — open an issue or reach out. Adapter contributions for new devices are particularly welcome.

## License

See [LICENSE](./LICENSE) for details.
