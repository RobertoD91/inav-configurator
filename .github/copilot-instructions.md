# Project Guidelines

## Build And Verification

- Install dependencies with `yarn install`.
- Use `yarn start` for normal development; this starts Electron Forge with the Vite renderer pipeline.
- Use `yarn make` for distributable builds. Platform-specific packaging details live in [README.md](../README.md) and [CLAUDE.md](../CLAUDE.md).
- Do not rely on `yarn lint` for validation; it is a placeholder and there is no general automated test suite.
- After code changes, prefer targeted manual verification in the running app. If you touch the transpiler, run the relevant scripts in `js/transpiler/transpiler/tests/`.

## Architecture

- Treat this as an Electron desktop app with a jQuery-based renderer, not a React or Vue application.
- Main-process code lives in `js/main/`. The renderer entry point is `js/configurator_main.js`, and tab routing and lifecycle live in `js/gui.js`.
- Flight-controller state is centralized in `js/fc.js`. MSP decoding updates `FC` state before UI callbacks run.
- Connection transport code is under `js/connection/`. MSP protocol serialization and decoding are under `js/msp/`.
- Tabs are implemented as paired files in `tabs/` and usually load their HTML via `import('./tab-name.html?raw')` before binding handlers.

## Conventions

- Match the existing ES module style and preserve local file conventions such as `'use strict';`, function declarations, and jQuery-heavy DOM code.
- For renderer work, use existing jQuery patterns and shared helpers instead of introducing new frameworks or large architectural rewrites.
- For MSP-driven flows, follow the existing `MSP.send_message(code, payload, false, callback)` pattern and refresh UI state from `FC` inside callbacks.
- If a feature needs filesystem, dialog, serial, or other system access, add the handler in `js/main/main.js` and expose it through `js/main/preload.js`.
- Keep protocol and state changes focused. Avoid growing `js/msp/MSPHelper.js` or `js/fc.js` with unrelated logic when a helper module or collection is a better fit.
- Add user-facing strings to `locale/en/messages.json` and use `data-i18n` attributes in HTML where the surrounding code does so.
- When changing or adding tabs, keep related markup in `tabs/` and tab-specific styling in `src/css/tabs/`.

## Area-Specific Notes

- Read [CLAUDE.md](../CLAUDE.md) for the top-level repository map, packaging flow, and subsystem overview.
- If you edit anything under `js/transpiler/`, read [js/transpiler/CLAUDE.md](../js/transpiler/CLAUDE.md) first. That area expects round-trip style tests and doc/example updates when behavior changes.
