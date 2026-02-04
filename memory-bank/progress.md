# Progress Tracker

## Message Recovery Plugin (Option A - Pure JS Injection)

### Status: IN PROGRESS

---

### ✅ Phase 1: Core Module Finder + Storage (COMPLETE)
- [x] Research WAIncognito module finder logic
- [x] Create `src/messagerecovery.js` with Webpack module finder
- [x] Implement IndexedDB Storage layer
- [x] Create WhatsAppAPI wrapper (MsgStore, ChatStore, DownloadManager)
- [x] Commit: `9a07e1e` → Pushed to `feature/message-recovery-plugin`

---

### 🔄 Phase 2: Message Interception (IN PROGRESS)
- [ ] Implement message store watcher
- [ ] Hook into revoke detection
- [ ] Cache messages to IndexedDB
- [ ] Atomic commit

### ⏳ Phase 3: Media Download Hook
- [ ] Hook `downloadManager.downloadAndMaybeDecrypt()`
- [ ] Convert media to Base64
- [ ] Store in IndexedDB with message reference
- [ ] Atomic commit

### ⏳ Phase 4: Qt Integration
- [ ] Inject `messagerecovery.js` via `webenginepage.cpp`
- [ ] Add settings toggle in `settingswidget.cpp`
- [ ] Atomic commit

### ⏳ Phase 5: UI Indicators
- [ ] Add 🗑️ icon for recovered messages
- [ ] Create simple overlay for viewing recovered content
- [ ] Atomic commit

### ⏳ Phase 6: Verification
- [ ] Manual testing with real WhatsApp
- [ ] Update documentation
- [ ] Final commit and push

---

## Commits Log
| Hash | Message | Date |
|------|---------|------|
| `9a07e1e` | feat(plugin): add core message recovery JS | 2026-02-04 |

---

## Technical Notes
- **Branch:** `feature/message-recovery-plugin`
- **Approach:** WAIncognito-style webpack module injection
- **Storage:** IndexedDB (browser-native, persistent)
- **Reference:** tomer8007/whatsapp-web-incognito
