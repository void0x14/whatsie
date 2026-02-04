# Message Recovery Plugin - MVP Implementation Plan

## Araştırma Özeti

### Kaynak: WAIncognito (tomer8007/whatsapp-web-incognito)
- **1.8k+ stars** - En popüler açık kaynak çözüm
- **Yaklaşım:** WebSocket hooking + WhatsApp internal API

### Kritik Bulgular

1. **Mesaj + Medya Yakalama:**
   - Sadece DOM monitoring YETERSİZ
   - WhatsApp'ın internal `downloadManager` API'si kullanılmalı
   - Medya (resim/video/ses) için `mediaKey` ile decrypt gerekli

2. **Teknik Yaklaşım:**
   ```javascript
   // WhatsApp'ın internal download manager'ı
   const decryptedData = await WhatsAppAPI.downloadManager.downloadAndMaybeDecrypt({
       directPath: msg.directPath,
       encFilehash: msg.encFilehash,
       mediaKey: msg.mediaKey,
       type: msg.type
   });
   ```

3. **Storage:** IndexedDB (browser-native, persistent)

---

## MVP Zorluk Değerlendirmesi

| Zorluk | Açıklama |
|--------|----------|
| 🔴 **YÜksek** | WhatsApp internal API'lerine erişim gerekli |
| 🔴 **Yüksek** | Qt WebEngine ≠ Browser Extension (farklı izolasyon) |
| 🟡 **Orta** | WebSocket hooking Qt'de mümkün ama karmaşık |
| 🟢 **Düşük** | IndexedDB → SQLite dönüşümü kolay |

### En Büyük Zorluk
Qt WebEngine, Chromium tabanlı ama **browser extension API'si yok**. WAIncognito'nun yaptığı `WebSocket.prototype.send` hooking Qt'de farklı çalışır.

---

## MVP Stratejileri (3 Opsiyon)

### Opsiyon A: Pure JavaScript Injection (ÖNERILEN - MVP için)
**Süre: 2-3 gün**

1. JavaScript ile WhatsApp internal modüllerine eriş
2. `require('WAWebDownloadManager')` hook et
3. Her mesaj geldiğinde cache'le
4. Silinen mesajları IndexedDB'de sakla
5. Qt WebChannel ile C++'a raporla (optional)

**Avantaj:** Pure JS, minimum C++ değişikliği
**Dezavantaj:** WhatsApp internal API değişirse kırılır

### Opsiyon B: WebSocket Proxy (Karmaşık)
**Süre: 5-7 gün**

1. Qt'de WebSocket trafiğini intercept et
2. Binary node'ları parse et
3. Revoke mesajlarını blokla

**Avantaj:** Daha stabil
**Dezavantaj:** Çok karmaşık, binary node parsing gerekli

### Opsiyon C: Hybrid (IndexedDB Monitor)
**Süre: 1-2 gün**

1. WhatsApp'ın kendi IndexedDB'sini izle
2. Mesajlar silinmeden önce kopyala
3. Minimal invasive

**Avantaj:** En az invasive
**Dezavantaj:** Medya yakalama sınırlı

---

## ÖNERILEN: Opsiyon A (Pure JS) - Detaylı Plan

### Faz 1: Core JavaScript (1 gün)
**Dosya:** `src/messagerecovery.js`

```javascript
(function() {
    'use strict';
    
    // 1. WhatsApp internal modüllerine eriş
    const getModules = () => {
        // WhatsApp Web'in webpack modüllerini bul
        return Object.values(require.c)
            .filter(m => m?.exports)
            .map(m => m.exports);
    };
    
    // 2. Download Manager'ı bul
    let downloadManager = null;
    for (const mod of getModules()) {
        if (mod?.downloadAndMaybeDecrypt) {
            downloadManager = mod;
            break;
        }
    }
    
    // 3. Mesaj store'u izle
    const msgStore = getModules().find(m => m?.Msg);
    
    // 4. IndexedDB storage
    const dbName = 'whatsie_recovery';
    let db = null;
    
    // 5. Revoke event listener
    // ... implementation
})();
```

### Faz 2: Storage Layer (0.5 gün)
- IndexedDB schema: `{ id, chatId, sender, content, mediaBase64, mediaType, timestamp, isDeleted }`
- Auto-cleanup: 30 gün sonra sil

### Faz 3: UI Integration (0.5 gün)
- Silinen mesajı gösterirken 🗑️ işareti ekle
- Settings'e ON/OFF toggle ekle

### Faz 4: Qt Integration (Optional - 0.5 gün)
- Qt WebChannel ile C++'a bildir
- Medya dosyalarını diske kaydet (opsiyonel)

---

## Dosya Yapısı

```
src/
├── messagerecovery.js       # Core JavaScript (embedded resource)
├── messagerecovery.h        # C++ header (optional Qt integration)
├── messagerecovery.cpp      # C++ implementation (optional)
└── webenginepage.cpp        # Inject messagerecovery.js
```

---

## Risk Değerlendirmesi

| Risk | Olasılık | Etki | Mitigasyon |
|------|----------|------|------------|
| WhatsApp API değişikliği | Yüksek | Yüksek | Modular kod, kolay update |
| Hesap banlama | Düşük | Çok Yüksek | Pasif monitoring, aktif manipulation yok |
| Performance impact | Orta | Düşük | Lazy evaluation, batch storage |

---

## Sonraki Adım

**Kullanıcı kararı gerekli:**

1. **Opsiyon A (Pure JS)** - 2-3 gün, MVP için ideal
2. **Opsiyon C (IndexedDB Monitor)** - 1-2 gün, daha basit ama sınırlı
3. **Opsiyon B (WebSocket Proxy)** - 5-7 gün, en stabil ama karmaşık

Hangisini tercih ediyorsun?
