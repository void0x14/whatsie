# Whatsie - Proje Durum Raporu

**Tarih:** 4 Şubat 2026
**Durum:** Aktif / Bakım ve Geliştirme Aşaması
**Versiyon:** 4.16.0

---

## 📊 Proje Özeti
Whatsie, Linux masaüstü kullanıcıları için geliştirilmiş, Qt WebEngine tabanlı, özellik açısından zengin bir WhatsApp web istemcisidir. Sadece bir wrapper olmanın ötesinde, sistem entegrasyonu, güvenlik (Uygulama Kilidi) ve özelleştirme seçenekleri sunar.

### 🛠️ Teknoloji Yığını
- **Dil/Çerçeve:** C++ / Qt 5.15+
- **Motor:** Qt WebEngine (Chromium tabanlı)
- **İnşa Sistemi:** QMake
- **Paketleme:** Snap, Debian (.deb), AUR (Arch Linux), Flatpak
- **Platform:** Linux Masaüstü (X11 & Wayland desteği)

---

## ✅ Tamamlananlar (Son Güncellemeler)
En son **4.16.0** sürümü ile birlikte aşağıdaki geliştirmeler yapılmıştır:

*   **Güvenlik:** Güvenli derleme bayrakları (secure compilation flags) eklendi.
*   **Hata Düzeltmeleri:** 
    *   Uygulama simge durumunda (minimized) başladığında dinamik yakınlaştırma (zoom) hatası giderildi.
    *   Yazım denetleyici (Spell Checker) için sözlük yolu (`QTWEBENGINE_DICTIONARIES_PATH`) düzeltildi.
    *   **Tema ve UI:** 
        *   Tema değiştirme (toggle) mantığı güncellendi ve daha stabil hale getirildi.
        *   WhatsApp Web üzerinde kare şeklinde görünen kullanıcı ikonları (avatarlar) CSS enjeksiyonu ile tekrar daire haline getirildi (GitHub Issue #279).
*   **Kullanıcı Deneyimi:**
    *   Sistem tepsisi (tray) menüsüne hızlı tema değiştirme seçeneği eklendi.
    *   Uygulama kilitliyken "Yeni Mesaj" ve "Yeniden Yükle" seçenekleri devre dışı bırakılarak güvenlik artırıldı.
    *   İtalyanca dil desteği ve çeşitli yerelleştirmeler güncellendi.

---

## 🔄 Mevcut İlerleme (WIP)
Şu anda sistemin genel kararlılığı ve Linux masaüstü entegrasyonu (özellikle Wayland ve farklı masaüstü ortamları) üzerinde odaklanılmaktadır. 

**Aktif Odak Noktaları:**
1.  Hata ayıklama süreçlerinin iyileştirilmesi.
2.  Snap ve Flatpak dağıtım süreçlerindeki izin ve tema uyumsuzluklarının giderilmesi.

---

## ⏳ Yapılacaklar (Roadmap)
`TODO.md` ve planlar doğrultusunda bekleyen özellikler:

*   [ ] **Hata Ayıklama:** Snap paketleri için hata ayıklama bilgilerine snap revizyon numarasının eklenmesi.
*   [ ] **Ayarlar:** "Sistem tepsisine küçültüldüğünde uygulamayı kilitle" seçeneğinin eklenmesi.
*   [ ] **Erişilebilirlik:** Küresel kısayol (Global Shortcut) kullanarak Whatsie penceresini aktif hale getirme yeteneği.
*   [ ] **UI/UX:** Dark/Light tema geçişlerinin daha pürüzsüz hale getirilmesi.

---

## 🤖 Ajan Durum Panosu
*   **Project Planner:** Proje analizi tamamlandı, durum raporu oluşturuldu.
*   **Frontend Specialist:** UI bileşenleri ve Qt WebEngine entegrasyonu incelendi.
*   **Status:** Bekleyen kritik bir hata bulunmuyor, geliştirme "Enhancement" (Geliştirme) fazında.

---

## 📂 Dosya İstatistikleri
*   **Kaynak Kod:** `src/` dizini altında yoğunlaşmış C++ ve UI dosyaları.
*   **Paketleme:** `snap/`, `debianpkg/`, `dist/` dizinleri güncel.
*   **Belgeleme:** `CHANGELOG.md`, `README.md` ve `TODO.md` düzenli olarak güncelleniyor.

---
*Bu rapor otomatik olarak Antigravity tarafından oluşturulmuştur.*
