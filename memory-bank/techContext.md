# Tech Context

## Technology Stack
- **Language:** C++ (C++17)
- **Framework:** Qt 5.15+ (Widgets, WebEngine, Positioning)
- **Engine:** Qt WebEngine (Chromium)
- **Build System:** QMake
- **UI:** Qt Designer (.ui files)

## Dependencies
- **Qt Modules:** `core`, `gui`, `widgets`, `webengine`, `webenginewidgets`, `positioning`.
- **System Libs:** `libX11` (for window management on X11).
- **Internal:** `SingleApplication` (included).

## Development Setup
- Build via `qmake && make` in the `src/` directory.
- Requires Qt 5 WebEngine development headers.
- Spell checker requires dictionary files (.dic/.aff) transformed into .bdic format.

## Deployment Formats
- **Snap:** Configured in `snap/snapcraft.yaml`.
- **Debian:** Configured in `debianpkg/`.
- **Flatpak:** Managed via external manifests.
- **AUR:** community-maintained.

## Technical Constraints
- Requires a functional Chromium-compatible renderer (provided by WebEngine).
- Dependent on WhatsApp Web's HTML structure; changes there often require updates to CSS/JS injection logic in `WebEnginePage`.
