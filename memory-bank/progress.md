# Progress

## Project Status: 4.16.x (Maintenance & Enhancement)

### What Works ✅
- Core messaging via WhatsApp Web.
- Application Lock (Password protection).
- System Tray integration and notifications.
- Dynamic Zoom (Maximized/Normal).
- Spell Checker.
- Full Width View toggle.

### In Progress 🔄
- **Avatar Fix (Issue #279):** v2 implementation using CDN URL selectors - testing in progress.
- **Theme Toggle:** Added page reload for reliable theme switching - testing in progress.

### What's Left to Build ⏳
- [ ] Snap revision number in debug info.
- [ ] "Lock app on minimize to tray" setting.
- [ ] Global shortcut for window activation.
- [ ] Automated Test Suite (Unit/Integration).

### Known Issues 🐞
- WhatsApp Web updates frequently break CSS selectors (mitigated with CDN URL approach).
- Wayland-specific quirks in some desktop environments.

### Evolution of Decisions
- **Feb 2026 (v2):** Switched from obfuscated class selectors to CDN URL-based selectors for avatar fix.
- **Feb 2026 (v2):** Added page reload to theme toggle for reliable state synchronization.
- **Feb 2026 (v1):** Adopted the **Memory Bank** structure for better long-term project intelligence.
