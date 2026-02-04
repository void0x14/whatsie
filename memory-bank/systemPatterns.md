# System Patterns

## Architecture Overview
Whatsie is a Qt Widgets application that wraps a `QWebEngineView`. It utilizes a hybrid approach, combining C++ for system logic and JavaScript/CSS injection for modifying the WhatsApp Web interface.

## Key Technical Decisions
1. **Engine:** `QWebEngine` (Chromium) is used for its modern web standards support and performance.
2. **Single Instance:** Uses `SingleApplication` to prevent multiple instances and handle IPC for opening URLs in the existing process.
3. **Security:** Application Lock relies on a local password stored (base64 encoded - to be improved) in settings.
4. **Theme Management:** Dual-themed (Light/Dark) via Qt Palettes and injected CSS into the web view.

## Core Components
- `MainWindow`: Coordinates all sub-components and handles window/tray logic.
- `WebEnginePage`: Custom `QWebEnginePage` for handling navigation, permissions, and script injection.
- `SettingsWidget`: Unified UI for application configuration.
- `Lock`: Widget overlay for application password protection.
- `NotificationPopup`: Custom implementation for desktop notifications when native ones are insufficient.

## Design Patterns
- **Singleton-ish:** `SettingsManager` acts as a central access point for app settings.
- **Observer:** `MutationObserver` in JavaScript is used to detect DOM changes (like theme classes) and apply fixes dynamically.
- **Bridge Pattern:** Interaction between C++ and JavaScript via `runJavaScript`.
