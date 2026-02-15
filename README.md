# AR-Native

A universal framework for building AR glasses applications. Write once, deploy to any AR platform — like Expo, but for AR glasses.

AR-Native abstracts away the differences between AR glasses SDKs (Rokid, Even Realities, Meta, etc.) behind a unified API, so you can build your app once and run it on any supported device.

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
