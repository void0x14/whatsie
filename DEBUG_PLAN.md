# 🐛 Whatsie Black Screen Debug Plan

## 🚨 Current Status
- **Issue:** Launching Whatsie results in a black screen (Black Screen of Death).
- **Cause:** Likely JS injection (`messagerecovery.js` or `injectFullWidthJavaScript`) crashing the renderer or blocking the main thread.
- **Action:** Disabled `injectMessageRecoveryScript()` to isolate the issue.

---

## 🛠️ Step-by-Step Debugging Protocol

### Phase 1: Isolation (Isolating the culprit)
- [x] **Step 1:** Comment out `injectMessageRecoveryScript()` in `webenginepage.cpp`.
    - *Status:* Done.
    - *Action:* User needs to build & run to confirm UI comes back.
- [ ] **Step 2:** If UI is back, the issue is definitely in `messagerecovery.js`.
    - *Next Check:* Is `moduleRaid` logic causing an infinite loop?
    - *Next Check:* Is `MutationObserver` flooding the event loop?

### Phase 2: Research & Hardening (Using `rtfmbro`)
- [ ] **Step 3:** Research WhatsApp Web Webpack chunk structure (2025/2026).
    - *Tool:* `rtfmbro` (Search GitHub for recent "whatsapp web webpack injection").
- [ ] **Step 4:** Analyze `injectFullWidthJavaScript` logic.
    - *Hypothesis:* The `while` loop or `MutationObserver` there might be conflicting with React's rendering.

### Phase 3: Implementation Fix
- [ ] **Step 5:** Rewrite `messagerecovery.js` to be non-blocking.
    - *Strategy:* Use `requestIdleCallback` or `setTimeout` for heavy lifting.
    - *Strategy:* Add "Safety Switch" (if init fails 3 times, stop trying).
- [ ] **Step 6:** Re-enable injection in `webenginepage.cpp`.

### Phase 4: Verification
- [ ] **Step 7:** Build & Run.
- [ ] **Step 8:** Check DevTools Console for clean logs.

---

## 📝 Notes & Findings
- **Log Analysis:** Console showed `Uncaught TypeError: Cannot read property 'style' of null`.
- **Fix Applied:** Added null check to `injectFullWidthJavaScript`.
