# 🕵️‍♂️ WhatsApp Web Plugin Fix & Debug Plan

## 🚨 Critical Issues
1.  **Plugin Failure:** `WhatsieRecovery: Failed to initialize after 30 attempts`.
    *   *Cause:* `window.webpackChunkwhatsapp_web_client` not found or renamed.
2.  **Console Noise:** `[uim] Attempting to set multiple UIM tree roots`.
    *   *Cause:* React root collision (likely from FullWidth script or internal race condition).
3.  **CSP Violations:** `wasm-unsafe-eval`.
    *   *Cause:* WhatsApp's strict Content Security Policy.

## 🗺️ Roadmap & Strategy

### Phase 1: Reconnaissance (Target: WhatsApp Web Internal Structure)
*   **Objective:** Identify the correct global variable for Webpack chunks.
*   **Keywords:** `window.webpackChunk`, `window.modules`, `self.__webpack_require__`.
*   **Action:**
    *   Use Browser Console to iterate `window` keys filtering for "webpack" or chunk-like arrays.
    *   Check for `parcels` or other bundler artifacts if Webpack is gone.

### Phase 2: Implementation (Bulletproof Injection)
*   **Fix `messagerecovery.js`:**
    *   Implement "Universal Module Finder" (iterates known potential names).
    *   Use `QtWebEngine`'s `Isolated World` if possible (harder in C++, but cleaner).
*   **Fix `webenginepage.cpp`:**
    *   Ensure script runs at `DocumentReady`, not before.

### Phase 3: Education & Documentation
*   **Deliverable:** A guide on "Reverse Engineering WhatsApp Web for 2026".
*   **Levels:**
    *   *Lvl 1:* DOM Manipulation (Selectors).
    *   *Lvl 2:* Network Interception (Service Workers).
    *   *Lvl 3:* Javascript Module Injection (Webpack Internals).

## ✅ Verification Checklist
- [ ] Module Finder returns `true`.
- [ ] "Recovered" logs appear in console.
- [ ] Full Width View works without "Multiple UIM Roots" error.
- [ ] No crash on startup.
