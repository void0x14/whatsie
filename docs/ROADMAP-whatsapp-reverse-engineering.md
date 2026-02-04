# 🗺️ WhatsApp Web Reverse Engineering Roadmap & Debug Analysis (2026)

## 🚨 Console Error Analysis (From Screenshot)

### 1. `WhatsieRecovery: Failed to initialize after 30 attempts`
*   **Sebep:** Plugin, `window` objesi üzerinde `webpackChunkwhatsapp_web_client` veya benzeri bir global değişken arıyor ama bulamıyor. WhatsApp Web (meta), botları engellemek için bu değişkenlerin isimlerini dinamik hale getirdi veya değiştirdi.
*   **Çözüm Yolu:** Global scope'u tarayıp (iterate edip) boyutu büyük olan array'leri veya içinde `push` metodu olan objeleri bulman lazım.

### 2. `[uim] Attempting to set multiple UIM tree roots`
*   **Teknik Anlamı:** WhatsApp'ın React render motoru (UIM - UI Manager), sayfada zaten bir "Root" varken ikinci bir Root oluşturmaya çalışıyor.
*   **Bizim Hatamız:** `whatsie-full-view` stili veya scripti, React daha mount olmadan DOM'a müdahale ettiği için (hydration mismatch), WhatsApp React App çökmüş ve kendini yeniden başlatmaya çalışmış (re-mount).
*   **Çözüm:** DOM manipülasyonunu sadece CSS ile yap (ki bunu yaptık), JS ile elementlere dokunma.

### 3. `Content Security Policy (CSP): 'wasm-unsafe-eval'`
*   **Anlamı:** WhatsApp WebAssembly (Wasm) kullanıyor. Bu hatalar genellikle "uyarı" niteliğindedir ve uygulamanın çalışmasını engellemez. Göz ardı edilebilir.

---

## 🔑 Critical Keywords for Research

Bu terimleri Google, GitHub ve Reddit'te (r/whatsapp) araştırmalısın:

### Level 1: Module Extraction (Modülleri Çalma)
*   `Webpack Module Federation WhatsApp`
*   `window.webpackChunk push override`
*   `WhatsApp Web moduleRaid alternative 2025`
*   `Getting require from webpackJsonp`

### Level 2: Hooking & Interception
*   `WebSocket frame interception` (Mesajları ağ seviyesinde yakalamak için en garantili yol)
*   `MsgStore` ve `ChatStore` (WhatsApp'ın internal veritabanı objeleri)
*   `Service Worker injection`

### Level 3: Anti-Detection
*   `WABrowserId` (Browser kimliği)
*   `Puppeteer Stealth Plugin` (Mantığını anlamak için)

---

## 🛠️ Step-by-Step Debugging Roadmap (Kendi Yapacağın)

### Adım 1: Recon (Keşif)
1.  **Tarayıcı Konsolunu Aç:** `http://127.0.0.1:9421` adresine Chrome ile bağlan (DevTools).
2.  **Global Değişken Avı:** Şu komutu çalıştırıp Webpack chunk'ının yeni adını bul:
    ```javascript
    Object.keys(window).filter(k => k.includes('chunk') || k.includes('pack'))
    ```
    *Eğer bu boş dönerse, `window` objesini manuel incele.*

### Adım 2: Injection Fix
1.  **`src/js/messagerecovery.js` dosyasını aç.**
2.  `init()` fonksiyonundaki `window.webpackChunkwhatsapp_web_client` kısmını, Adım 1'de bulduğun yeni isimle değiştir.
3.  Eğer isim dinamikse (her reload'da değişiyorsa), Array.isArray() kontrolü yapan bir döngü yaz.

### Adım 3: Stability
1.  **`webenginepage.cpp`:** Script injection'ı `loadFinished` sinyaline bağla (şu an öyle ama emin ol).
2.  **Delay:** `setTimeout` süresini artır (2000ms yerine 5000ms yap) ki WhatsApp tamamen yüklensin.

---

## 📚 Recommended Resources (Learning)
1.  **GitHub:** `pedroslopez/whatsapp-web.js` (Kaynak kodundaki `src/util/Injected.js` dosyasını incele. En güncel modül bulma yöntemi orada).
2.  **GitHub:** `mukulhase/WebWhatsapp-Wrapper` (Python tabanlı ama mantığı benzer).
3.  **Documentation:** Webpack docs -> "Runtime" & "Global Variables".

**Seviye:**
*   **Module Injection:** Advanced (Level 4/5). Webpack'in nasıl bundle oluşturduğunu (IIFE, closure) anlaman gerek.
*   **Network Interception:** Expert (Level 5/5). WebSocket binary frame decode etmen gerek (Protobuf).

---

Bu roadmap ile sorunu kendin çözebilirsin. Başarılar.
